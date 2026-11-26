# Amplify Hosting

- 関係するタスク：TASK-H01（技術構成を理解する）の3回目。秘密情報の置き場所は TASK-P06（技術構成の ADR）で、Next.js 16 で動くかは TASK-E01 のスパイクで確かめる
- まとめた日：2026-11-25（作者がまとめ、Claude が確認）

## 作者のまとめ（3行）

1. Amplify Hosting は、GitHub と連携していて、push すると自動でビルドから公開までを行ってくれる AWS サービス
2. ブランチごとに別の URL で公開できる。Studai では main を本番にする
3. 今回使う Next.js 16 は公式の対応表に載っていないので、しっかり動作確認を行う

## 用語

- ホスティング：作ったアプリを、インターネットから見られる場所に置くこと
- SSR（サーバーサイドレンダリング）：アクセスがあるたびにサーバーで HTML を作ること。Server Component・Server Action・Route Handler がこれにあたる
- CDN（CloudFront）：世界中にある配信拠点。JS・CSS・画像などの変わらないファイルを利用者の近くから返す
- ビルド：ソースを本番で動く形に変換する作業（`npm run build`）
- amplify.yml：Amplify がビルドのときに実行するコマンドを書くファイル

## 何者か、なぜ必要か

Next.js のアプリを公開するには、ビルド、サーバーでの実行、HTTPS での配信、更新のたびの入れ替えが必要になる。Amplify Hosting はこれをまとめて引き受ける。Next.js のプロジェクトは自動で認識され、コンソールでの手作業の設定はほとんど要らない。

```
git push（main）
   │
   ▼
Amplify Hosting
 ├ ① ビルド：amplify.yml の通りに npm ci → npm run build
 ├ ② 静的ファイル（JS・CSS・画像） → CloudFront から配信
 └ ③ SSR の部分 → Amplify が管理するコンピュート（中身は Lambda。自分では触らない）
   │
   ▼
https://main.xxxx.amplifyapp.com（独自ドメインと SSL 証明書も設定できる）
```

API Gateway＋Lambda＋CloudFront＋CI/CD を、Next.js 専用に1つにまとめたものと考えるとよい。手間が減る代わりに、Lambda のメモリやタイムアウトなどは細かく調整できない。

## 仕組み

- ブランチがそのまま環境になる：ブランチごとに別の URL で公開される。main が本番。develop をつなぐかはデプロイのタスクで決める（つなぐとビルドと費用が増える）
- Node.js のバージョン：ビルドに使ったメジャー版を本番の実行にも使う。対応は 20・22・24。リポジトリと amplify.yml で 24 に固定し、手元・CI・本番をそろえる
- ログ：SSR の実行ログは CloudWatch Logs に出る

## 注意点

### Next.js 16 は公式の対応表にない

- AWS の公式ドキュメントでは、SSR を完全に管理するのは Next.js 12〜15 のままになっている
- 16 の App Router のアプリは公開できたという報告はある。ただしオンデマンド ISR、Edge API Routes、ストリーミングなどは使えないとされている（Studai では使う予定がない）
- TASK-E01 のスパイクで実際に載せて確かめる。だめなら Next.js 15 に下げる

### 環境変数と秘密情報

- Amplify のコンソールで設定した環境変数は、ビルドのときにしか使えない。SSR の実行時にも使うには、amplify.yml で `.env.production` に書き出す必要がある
- AWS は、秘密情報を環境変数に置かないよう勧めている（デプロイの成果物にアクセスできる人が読めるため）。AWS のリソースへのアクセスには IAM ロールを使うよう案内している
- SSR コンピュートロール：Amplify の SSR に IAM ロールを付けられる。一時的な認証情報になるので、アクセスキーを置かずに済む

| 秘密情報 | 今の技術構成 | 解説で分かったこと |
|---|---|---|
| Data API への権限（rds-data・Secrets Manager の読み取り） | Secrets Manager | SSR コンピュートロールに権限を付ける。アクセスキーは作らない |
| ログインの鍵、Google のシークレット、Claude API キー | Amplify の環境変数 | `.env.production` への書き出しが必要で、AWS は勧めていない。Secrets Manager に置いてロール経由で読む案もある（1シークレット月 $0.40） |

どちらにするかは TASK-P06 で決める。

## 他の選択肢との違い

| 選択肢 | どんなものか | Studai で見ると |
|---|---|---|
| Amplify Hosting（採用予定） | push するだけ。SSR・CDN・HTTPS を AWS が管理 | 手間が一番少ない。IaC なしで済む。Next.js 16 は E01 で確認 |
| Vercel | Next.js を作っている会社のホスティング | 相性は一番良いが、AWS の外になり、Aurora と IAM ロールでつなげない |
| S3＋CloudFront（静的書き出し） | 事前に作った HTML を置くだけ | SSR・Server Action・API が使えない |
| SST／OpenNext で Lambda に載せる | 自分のアカウントに Lambda などを組み立てる | 調整の幅は広いが、IaC が前提になる |
| ECS（Fargate）など | コンテナを常時動かす | 置いておくだけで課金される（運用ルールの「9. 費用」に反する） |

## 費用

| 項目 | 料金 | Studai の目安 |
|---|---|---|
| ビルド | 1分 $0.01 | 1回3分×月30回で約 $0.9 |
| 配信（転送量） | 1GB $0.15 | 1GB 未満 |
| SSR のリクエスト数 | 100万回 $0.30 | ほぼ0 |
| SSR の実行時間 | 1GB・時間 $0.20 | ほぼ0 |
| WAF | 1アプリ月 $15＋WAF の料金 | 使わない |

- Amplify の部分は月100〜300円くらいの見込み。費用の大部分は Aurora と Claude API になる
- 無料枠は、アカウントを作った時期で形が違う（従来の12か月の無料枠か、クレジット制か）。TASK-H03（費用の上限設定）のときに確認する

## Studai でどう使うか

- main ブランチを本番としてつなぐ
- Node.js は 24 に固定する
- DB（Data API）には SSR コンピュートロールでアクセスする
- 秘密情報の置き場所は TASK-P06 で決め直す
- Next.js 16 で動くかは TASK-E01 で確かめる
- WAF は使わない。ログは CloudWatch で見る

## 参考

- [Amplify support for Next.js](https://docs.aws.amazon.com/amplify/latest/userguide/ssr-amplify-support.html)
- [SSR supported features（Node.js のバージョン、CloudWatch Logs）](https://docs.aws.amazon.com/amplify/latest/userguide/ssr-supported-features.html)
- [Making environment variables accessible to server-side runtimes](https://docs.aws.amazon.com/amplify/latest/userguide/ssr-environment-variables.html)
- [Adding an SSR Compute role](https://docs.aws.amazon.com/amplify/latest/userguide/amplify-SSR-compute-role.html)
- [AWS Amplify pricing](https://aws.amazon.com/amplify/pricing/)
