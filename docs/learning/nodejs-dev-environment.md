# Node.js を入れる場所（Windows か WSL2 か）

- 関係するタスク：TASK-H06（開発環境に Node.js を入れる）
- まとめた日：2026-11-25（作者がまとめ、Claude が確認）

## 作者のまとめ（3行）

1. A：Windows に Node.js 24 を入れる
2. 理由は、今まで通りの運用ができることが大きく、ルール決めなどが再発生しないため
3. Amplify はビルドに使った Node.js と同じメジャー版を本番の実行にも使い、対応しているのは 20・22・24 だから

## 何者か、なぜ必要か

- Node.js は、JavaScript をブラウザの外で動かすソフト。Next.js はこの上で動く
- ライブラリのインストール（npm）、開発用サーバー（`npm run dev`）、テスト、ビルドのすべてに必要。Claude Code が実装・テストを回すにも必要

## 前提

- Claude Code のデスクトップ版は、Windows 側（ローカルセッション）でコマンドを動かしている。WSL2 の中で動かす「WSL セッション」もあるが、リポジトリを WSL 側に置く前提で、ターミナル・コネクタ・プラグインなど使えない機能がある
- Next.js の公式ガイドは、WSL2 で開発するならプロジェクトを Linux 側（`/home/...`）に置き、C: や `/mnt/c` には置かないよう案内している（遅くなり、変更が反映されないことがある）
- Docker（ローカルの PostgreSQL）は `localhost:5432` でつながるので、Node をどちらに入れても使える
- Node.js 26 が 2026年10月に LTS になったが、Amplify Hosting が対応しているのは 20・22・24

## 他の選択肢との違い

| | A. Windows に入れる（採用） | B. WSL2 に入れ、プロジェクトも WSL に移す | C. WSL2 に入れ、プロジェクトは C: のまま |
|---|---|---|---|
| プロジェクトの場所 | 今のまま | `/home/…` に移す | 今のまま |
| Claude Code | 今のまま | WSL セッションに切り替え（使えない機能あり） | Windows の Claude Code から Node を呼べない |
| 本番（Linux）との近さ | 差がある（ファイル名の大文字・小文字、改行コード） | 近い | 近い |
| 速さ | 普通（Defender の除外で改善） | 速い | 遅い・変更が反映されないことがある |
| 切り替えの手間 | 小さい | 大きい（移行、ルールの書き換え） | ― |

C は Next.js の公式ガイドが勧めていないので外した。

## Studai でどう使うか

- Windows に Node.js 24 を入れる
- Claude Code デスクトップ版は PowerShell のプロフィールを読まない。PATH は Windows の環境変数に入れ、入れた後にアプリを一度終了して開き直す
- 本番（Linux）との差は、Node のバージョンをリポジトリで固定することと、GitHub Actions（Linux）で毎回テストすることで埋める
- 遅い・動きがおかしいと困るようになったら、B に移ることを考える

## 参考

- [Next.js：ローカル開発環境の最適化](https://nextjs.org/docs/app/guides/local-development)
- [Claude Code Desktop in WSL](https://code.claude.com/docs/en/desktop-wsl)
- [Amplify：SSR supported features（Node.js のバージョン）](https://docs.aws.amazon.com/amplify/latest/userguide/ssr-supported-features.html)
