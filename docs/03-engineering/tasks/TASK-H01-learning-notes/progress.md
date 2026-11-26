# TASK-H01 — 学習メモ（Amplify Hosting・Aurora Serverless v2・Data API）をコミットする

| 項目 | 内容 |
|------|------|
| タスクID | TASK-H01 |
| 担当 | Dev - Engineer（Claude Code） |
| フェーズ | MVP準備 |
| ブランチ | docs/TASK-H01-learning-notes |
| PR | https://github.com/hinata-nakamura-code/studai/pull/6 |
| ステータス | 🔵 レビュー待ち（PR #6） |

---

## 完了報告（タスクを終えたとき・止まったときに書く）

> PM はまずこの欄を読む。埋めるまで完了にしない。

| 項目 | 内容 |
|------|------|
| 状態 | 🔵 レビュー待ち（PR #6） |
| やったこと | `docs/learning/amplify-hosting.md`・`docs/learning/aurora-data-api.md` を中身を変えずにコミット。`docs/learning/README.md` の一覧の2行（PC で入っていたもの）をそのままコミット。`docs/ai-usage.md` の「記録」の先頭に TASK-H01 の項目を追加 |
| 証跡 | PR：https://github.com/hinata-nakamura-code/studai/pull/6 ／ レビュー記録：`docs/04-quality/reviews/TASK-H01-review.md` |
| 判断したこと | 学習メモは作者の記録なので、reviewer の指摘（表記の不揃いなど）があっても直さず、PM への申し送りにした |
| 確認方法と結果 | 公開チェック：問題なし。`git diff --stat develop` は6ファイルだけ（学習メモ2本・README・ai-usage.md・progress.md・レビュー記録）。README の一覧のリンク先はすべて存在。reviewer：必須の指摘なし。テスト・lint はアプリのコードがまだないため対象外 |
| 説明のポイント | ① Amplify Hosting は push で自動公開。Next.js 16 は公式の対応表にないため E01 で確かめる ② Aurora Serverless v2 は 0 ACU で止まり、再開待ちはリポジトリ層で再試行する ③ Amplify の SSR は VPC の外なので、Data API と IAM（SSR コンピュートロール）でつなぐ |
| 残課題・次にやることの提案 | ① 秘密情報の置き場所を TASK-P06 で決め、変えたら CLAUDE.md も直す ② Next.js 16 の動作確認は TASK-E01 で行う ③ コミットしていない `2026-11-20-architecture-review.md` と `adr/` の扱いを P06 で決める ④ 再開時間は24時間以上止まると30秒以上かかる。再試行の上限と CLAUDE.md の「約15秒」は P06 で見直す ⑤ 費用の見積もり（月30時間）は自分だけが使う前提。公開後にクローラー・ゲストで DB が起きる対策を P06・基本設計で決める |

---

## 完了条件と対応状況

| 完了条件 | 状態 |
|----------|------|
| 学習メモ2本・README・ai-usage.md を入れた develop 宛ての PR ができている | ✅（PR #6） |
| 公開チェックで問題がない | ✅ |
| reviewer サブエージェントで確認し、結果を `docs/04-quality/reviews/TASK-H01-review.md` に書いている | ✅ |
| progress.md の完了報告欄を埋め、ステータスと「状態」を「🔵 レビュー待ち（PR #番号）」にしている | ✅ |

## 計画（プランモードで決めたこと）

- develop を最新にして `docs/TASK-H01-learning-notes` を作る
- `docs/learning/amplify-hosting.md`・`docs/learning/aurora-data-api.md` は中身を変えずにコミットする
- `docs/learning/README.md` の一覧の2行は PC で入っていたので、そのままコミットする
- `docs/ai-usage.md` の「記録」の先頭に TASK-H01 の項目を足す
- `docs/01-product/2026-11-20-architecture-review.md` と `docs/03-engineering/adr/` はコミットしない（ファイル名を指定して add する）
- reviewer サブエージェントで確認してから、作者の承認を得て push・PR を作る

## 判断したこと

| 判断 | 理由 | ADR |
|------|------|-----|
| 学習メモは書き換えずにコミットする | 作者が自分の言葉でまとめた記録で、依頼文も中身の変更を求めていないため | — |

## 作業ログ

- 2026-11-26：develop（8afd218）が origin と同じで最新であることを確認し、ブランチを作成
- 2026-11-26：公開チェック。学習メモ2本と README を全文読み、grep もかけた。ローカルのパス・メールアドレス・Notion の ID・勤務先・個人的な事情・秘密情報の値はなし（「シークレット」「パスワード」は置き場所の説明だけで、値はない）
- 2026-11-26：ai-usage.md に TASK-H01 の項目を追加
- 2026-11-26：reviewer サブエージェントで確認。必須の指摘なし。推奨2件・任意1件を progress.md に反映し、学習メモへの指摘は PM への申し送りにした（`docs/04-quality/reviews/TASK-H01-review.md`）
- 2026-11-26：作者の承認を得て push し、develop 宛ての PR #6 を作成。ステータスを「🔵 レビュー待ち（PR #6）」に更新
- 2026-11-26：2回目のレビュー（PR #6）の指摘を反映。aurora-data-api.md の再開時間を公式に合わせて訂正（「作者のまとめ」は変えていない）。レビュー記録に2回目を追記し、残課題に再開時間と公開後の費用の2点を足した

## 動作確認

- テスト：アプリのコードがまだないため対象外（lint も同じ）
- 手動確認：README の一覧のリンク先（nextjs.md・nodejs-dev-environment.md・authentication.md・amplify-hosting.md・aurora-data-api.md）がすべて `docs/learning/` にあることを確認

## 残課題・次への引き継ぎ

- 秘密情報の置き場所：CLAUDE.md は「本番は Amplify の環境変数」だが、amplify-hosting.md では AWS が環境変数に置かないよう勧めていると整理し、TASK-P06 で決め直すとしている。P06 で変えたら CLAUDE.md も直す
- Next.js 16 は Amplify の公式の対応表にないので、TASK-E01 のスパイクで動作を確かめる
- `docs/01-product/2026-11-20-architecture-review.md` と `docs/03-engineering/adr/` はコミットしていない（扱いは TASK-P06 で決める）
- 再開時間は24時間以上止まると30秒以上かかる。再試行の上限と CLAUDE.md の「約15秒」は P06 で見直す
- 費用の見積もり（月30時間）は自分だけが使う前提。公開後にクローラー・ゲストで DB が起きる対策を P06・基本設計で決める
