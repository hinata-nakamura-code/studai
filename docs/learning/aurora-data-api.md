# Aurora Serverless v2・Data API

- 関係するタスク：TASK-H01（技術構成を理解する）の4回目。ORM か SQL を直接書くか（Data API との相性を含む）は TASK-P08 で決める
- まとめた日：2026-11-25（作者がまとめ、Claude が確認）

## 作者のまとめ

Aurora Serverless v2

1. AWS の RDB サービスの一つで、最大の特徴は、接続がないときは DB を止めておけること
2. 止まっている間は DB のデータ量の料金しかかからないので、費用を減らせる
3. その代わり、アクセスがあってから起動するまでに時間がかかる

Data API

1. DB を HTTPS の Web API で操作できる
2. Data API なら、DB を公開せずに、IAM の権限（SSR コンピュートロール）で安全につなげる。接続の管理も AWS がやってくれる

## 用語

- RDB（リレーショナルデータベース）：表（テーブル）にデータを入れ、SQL で読み書きする DB。PostgreSQL はその代表
- Aurora：AWS が作った、PostgreSQL・MySQL と互換のある DB（RDS の仲間）
- ACU：Aurora Serverless の性能の単位。1 ACU はメモリ約2GiB と、それに見合う CPU
- VPC：AWS の中の自分専用ネットワーク。DB は普通この中に置く
- コネクション（接続）：アプリと DB の間に張る通り道。普通の DB は、使う前にこれを張る

## Aurora Serverless v2

### 何者か

負荷に合わせて性能（ACU）を自動で上げ下げする Aurora。課金は ACU の時間単位（秒単位で計算）。

### 0 ACU まで下がる

- 最小 ACU を 0 にすると、決めた時間のあいだ接続がなければ自動で一時停止する。止まっている間は計算の料金がかからない
- 次のリクエストで自動で再開する。再開には普通は約15秒かかる。24時間以上止まっていたときは30秒以上かかることがある

```
接続なし → 決めた時間たつと一時停止（計算の料金 0）
   │
   ▼ ページが開かれる（DB へのリクエスト）
再開中（約15秒。長く止まっていたら30秒以上）→ 動いている間は ACU × 秒で課金 → 使われなくなったらまた止まる
```

注意点：

- 止まるのは計算だけ。ストレージの料金はかかり続ける（データは少ないので月数十円）
- 費用を左右するのは「起きている時間」。止まるまでの待ち時間を短くすると安くなるが、「起動中」を見る回数が増える
- 接続が開いたままだと止まらない。本番の DB に手元のツールでつないだままにしない
- 0 ACU に対応したエンジンのバージョンを選ぶ必要がある

## Data API

### 何者か

Aurora に HTTPS の窓口を用意し、AWS SDK（`ExecuteStatement` など）で SQL を送れるようにする仕組み。接続は AWS 側でまとめて使い回すので（コネクションプーリング）、アプリはドライバも接続の管理も要らない。

### なぜ Studai に必要か

Aurora は VPC の中に置くが、Amplify の SSR は自分の VPC の中に置けない。

| つなぎ方 | 問題 |
|---|---|
| DB をインターネットに公開して直接つなぐ | セキュリティの心配が大きい。パブリック IPv4 に料金がかかる |
| RDS Proxy を間に置く | Aurora Serverless v2 では最低 8 ACU 分の料金（東京で月約 $144）がかかる |
| Data API（採用予定） | HTTPS と IAM で、VPC の外から安全につなげる |

Data API を呼ぶのに必要なもの：

1. IAM の権限（Data API の呼び出しと Secrets Manager の読み取り）→ Amplify の SSR コンピュートロールに付ける
2. DB のユーザー名・パスワードが入ったシークレット（Secrets Manager）→ 呼ぶときに ARN を渡す

### 再開待ちとの組み合わせ

DB が止まっているときに Data API を呼ぶと、「再開中」のエラー（DatabaseResumingException）が返る。リクエストをきっかけに DB は再開するので、数秒待ってやり直せばよい。Studai では、リポジトリ層でこのエラーを受けたら数秒おきに再試行し、その間は画面に「起動中」と出す。

### 制限

| 制限 | Studai への影響 |
|---|---|
| クエリを送れるのは書き込み用インスタンスだけ | インスタンスは1台なので問題なし |
| 結果は1行あたり 64KB まで、全体で 1MiB まで | Markdown メモの本文に注意。一覧では本文を取らない |
| 1回の呼び出しで複数の SQL は送れない | マイグレーションの書き方に影響 |
| 一部のデータ型に対応していない | テーブル設計（基本設計）で確かめる |
| 1回の呼び出しは45秒まで | AI の問題生成は DB と別なので、影響は小さい |

## ローカルと本番のつなぎ方

```
ローカル：Next.js ──（ドライバで直接）──→ Docker の PostgreSQL 16
本番　　：Next.js ──（HTTPS・Data API）──→ Aurora Serverless v2
              ↑
        リポジトリ層：DB を使う関数をここにまとめ、中でつなぎ方を切り替える
```

画面や Server Action はリポジトリ層の関数を呼ぶだけにして、つなぎ方の違いはリポジトリ層の中に閉じ込める。ORM によって Data API 用のつなぎ方の有無が違うので、TASK-P08 で比べる。

## 他の選択肢との違い

| 選択肢 | どんなものか | Studai で見ると |
|---|---|---|
| Aurora Serverless v2＋Data API（採用予定） | 使った分だけの PostgreSQL。止められる | SQL を学ぶ目的に合い、AWS の中で IAM でつなげる。再開待ちと Data API の制限がある |
| RDS for PostgreSQL（常時起動） | 普通の PostgreSQL サーバー | 置いておくだけで課金される。Amplify からつなぐ問題も残る |
| DynamoDB | NoSQL | 安くてすぐ使えるが、RDB 設計と SQL を伸ばす目的に合わない |
| Neon・Supabase など | AWS の外のサーバーレス PostgreSQL | 手軽だが AWS の外にデータを預け、IAM でつなぐ構成にならない |

## 費用

| 項目 | 料金（東京） | Studai の目安 |
|---|---|---|
| 計算（ACU） | Aurora Standard で 1 ACU・時間 $0.15 | 0.5〜1 ACU×月30時間で $2.3〜4.5 |
| I/O | 100万リクエスト $0.24 | ほぼ0 |
| ストレージ | 1GB・月あたり | 1GB 未満で月数十円 |
| Data API | 100万リクエスト $0.35（us-east-1。最初の1年は月100万リクエストまで無料） | ほぼ0 |
| Secrets Manager | 1シークレット月 $0.40 | 約60円 |

DB まわりは月400〜800円くらいの見込み。Amplify と合わせて、Claude API を除き月500〜1,100円。

## Studai でどう使うか

- Aurora Serverless v2（PostgreSQL）を最小 0 ACU・最大 1〜2 ACU で作る。エンジンは 0 ACU に対応したバージョンにする
- Data API を有効にし、Amplify の SSR コンピュートロールから呼ぶ。DB のパスワードは Secrets Manager に置く
- 「再開中」のエラーはリポジトリ層で再試行し、画面に「起動中」を出す
- ローカルは Docker の PostgreSQL に直接つなぐ。切り替えはリポジトリ層の中だけで行う

後で決めること：

- ORM か SQL を直接書くか（Data API との相性を含めて）：TASK-P08
- Data API が対応していないデータ型を使わないか：基本設計
- 止まるまでの待ち時間：DB を作るタスク

## 参考

- [Scaling to Zero ACUs with automatic pause and resume for Aurora Serverless v2](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless-v2-auto-pause.html)
- [Introducing scaling to 0 capacity with Amazon Aurora Serverless v2](https://aws.amazon.com/blogs/database/introducing-scaling-to-0-capacity-with-amazon-aurora-serverless-v2)
- [Limitations for the Amazon RDS Data API](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/data-api.limitations.html)
- [Troubleshooting Amazon RDS Data API](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/data-api.troubleshooting.html)
- [DatabaseResumingException（AWS SDK）](https://docs.aws.amazon.com/botocore/latest/reference/services/rds-data/client/exceptions/DatabaseResumingException.html)
- [Amazon Aurora pricing](https://aws.amazon.com/rds/aurora/pricing/)
