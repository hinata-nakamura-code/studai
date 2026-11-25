# Studai — Claude Code への指示（エンジニア役）

<!--
メンテナンス用メモ（このコメントは Claude Code には読み込まれない）
- Claude Code はセッション開始時に毎回このファイルを読む。200行以内に保つ
- 役割・タスクの流れ・Git・記録の置き場所などの共通ルールは docs/rules.md が正本。ここには重ねて書かない
- 「必ず守らせたいこと」は文章ではなく .claude/settings.json の権限設定で強制する
- 公開リポジトリなので、個人的な事情、勤務先のこと、ローカルのパス、Notion の ID、メールアドレスは書かない（docs/rules.md の「8.」）
-->

共通の運用ルール（役割、タスクの流れ、Git、記録の置き場所、公開・非公開、費用）は次のファイルに従う。

@docs/rules.md

## あなたの役割
- このリポジトリでのあなたの役割は **エンジニア（実装担当）**。PM・デザイン・レビューは claude.ai のプロジェクトが担当し、作者が依頼文を渡す
- 作業の単位は TASK-ID。指示されたタスクの範囲だけを変更し、範囲外の改善は提案にとどめる
- 仕様の優先順位は ① この CLAUDE.md と docs/rules.md ② 作者が承認した要件定義・基本設計・詳細設計（レビューを通って develop に入ったファイル。基本設計には画面も含む）③ docs/01-product/carry-over.md（引き継ぎ事項。②がまだないところはこれに従う）。食い違いを見つけたら、実装を止めて指摘する。v1 の資料は参考で、そのまま従わない
- 推測で進めない。分からないこと・未決事項は質問する
- Notion は読んでよいが、更新しない（Notion を書くのは PM だけ）

## プロジェクト概要
- AI資格学習アプリ「Studai（Study + AI）」。自分のメモからAIが問題（多肢選択・○×・穴埋め）を作り、解答履歴から弱点を復習できる。試験日から逆算した学習計画・進捗管理と一体になっている
- 作者のポートフォリオとして、2027年1月上旬に MVP を AWS で公開する予定（工数・日程の見積もりはしない。順番は docs/01-product/plan.md）
- 作者は AWS と Python の経験はあるが、アプリ開発（Next.js・TypeScript、API、DB まわり）は学習中。作者が読んで説明できる、素直な書き方を優先する

## 作者が理解してから進める
- タスク一覧の「理解」が「解説待ち」のタスクは、作者が「理解済み」にするまで判断や実装に進まない。迷ったら着手前に確認する
- 実装の前に「何を作るか・なぜその方法か」を、専門用語を一言で説明しながら示す
- 「解説して」と言われたら、処理の流れと、その書き方にした理由を説明する

## 技術構成（2026-11-20 に決めて引き継いだもの。TASK-H01 で理解し、TASK-P06 で ADR を作り直す）
- Next.js（App Router）＋TypeScript＋Tailwind CSS＋shadcn/ui。API も Next.js の中に置く（Route Handlers / Server Actions。API サーバーは分けない）
- 認証：Auth.js（Google ログインのみ）
- 配信：AWS Amplify Hosting（ap-northeast-1）。main が本番
- DB：Aurora Serverless v2（PostgreSQL、最小 0 ACU で自動停止、最大 1〜2 ACU）＋RDS Data API。停止からの再開待ち（約15秒）は自動で再試行し、画面に「起動中」を表示する
- DB アクセスはリポジトリ層で抽象化する。ローカルは docker compose の PostgreSQL 16 に直接接続、本番は Data API 経由
- AI：Claude API。使うモデルは設定で切り替えられるようにする（モデルの選定は未決）
- テスト・CI：Vitest、Playwright、GitHub Actions（lint・型チェック・テスト）
- 監視：CloudWatch（Sentry は必要なら）
- 秘密情報：本番は Amplify の環境変数、Data API 用は Secrets Manager。.env はローカル専用
- 使わない：FastAPI（Python）、Lambda、API Gateway、Terraform、SSM パラメータストア、X-Ray、ECS Fargate、Prometheus・Grafana

## フォルダ構成
- アプリのコード：まだない（土台を作るタスクで構成を決め、ここを書き換える）
- docs/：設計と記録（置き場所は docs/rules.md の「7. 記録（証跡）の置き場所」）
- docs/private/：非公開のメモ（Git 管理外。読んでよいが、内容を公開ファイルに書き写さない）
- .claude/：Claude Code の設定（settings.json、agents）。settings.local.json は Git に入れない

## コマンド
- まだない（土台を作るタスクで、DB 起動・テスト・lint のコマンドを書く）

## 1タスクの進め方（エンジニア）
1. develop を最新にして、作業用ブランチ `<種類>/TASK-<ID>-<slug>` を作る
2. `docs/03-engineering/tasks/TASK-<ID>-<slug>/progress.md` をひな形（`docs/templates/progress.md`）から作る
3. 着手前に「作るもの・変更するファイル・確認方法」を示し、OK をもらってから実装する（プランモード）
4. 機能と一緒にテストを書き、テストと lint を通す。失敗したまま終えない
5. 判断が入った箇所は ADR の下書きを作り、AI に任せたこと・作者が判断したことを `docs/ai-usage.md` に1タスク1項目で追記する
6. reviewer サブエージェントで差分を確認し、正しさ・要件・公開してよい内容に関わる指摘を直す。結果を `docs/04-quality/reviews/TASK-<ID>-review.md` に書く
7. 依頼文で指示されていれば、push して develop 宛ての PR を作る
8. progress.md の完了報告欄を埋める。チャットには「progress.md を更新しました（パス）」と一言だけ書く
9. 「マージして」と言われたら、レビュー記録の未コミット分と、progress.md のステータスと完了報告の「状態」を「✅ 完了（PR #番号 で develop にマージ済み）」にした変更を、そのタスクの PR にコミットしてから develop にマージし、作業用ブランチを消して、ローカルの develop を最新にする

## 費用（エンジニアとして守ること）
- 置いておくだけで課金される部品（ALB、NAT ゲートウェイ、常時起動の RDS、Fargate、パブリック IPv4）は追加しない。必要なときは、月額の目安を示して確認を取る
- AWS リソースを追加・変更する提案には、必ず月額の目安を書く
- AWS のリソースを初めて作る前に、作者が費用の上限設定（TASK-H03）を済ませているか確認する
- AI 生成には回数制限（1人1日の上限と全体の上限）を必ず入れる

## 秘密情報
- `.env`・鍵ファイルは読まない・表示しない・コミットしない（.claude/settings.json でも禁止している）
- API キーやシークレットを、コード・ログ・テスト・回答に書かない。新しい環境変数は `.env.example` にキー名だけ追加する
- v1 で使っていた API キー・OAuth シークレットは作り直す前提（TASK-H02）。キーが必要なタスクでは、作者が作り直し済みか確認する

## 未決事項（実装前に確認すること）
- データモデルの階層（4階層か3階層か）：TASK-P07
- DB アクセスの方法（Data API 対応の ORM か、SQL を直接書くか）：TASK-P08
- AI モデル：TASK-P09
- 画面の状態管理が必要か：TASK-P10
