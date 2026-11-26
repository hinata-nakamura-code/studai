# TASK-H01 — 学習メモ（Amplify Hosting・Aurora Serverless v2・Data API）をコミットする

| 項目 | 内容 |
|------|------|
| タスクID | TASK-H01 |
| 担当 | Dev - Engineer（Claude Code） |
| フェーズ | MVP準備 |
| ブランチ | docs/TASK-H01-learning-notes |
| PR | （作成したら URL を書く） |
| ステータス | 🟡 進行中 |

---

## 完了報告（タスクを終えたとき・止まったときに書く）

> PM はまずこの欄を読む。埋めるまで完了にしない。

| 項目 | 内容 |
|------|------|
| 状態 | （PR を作ったら書く） |
| やったこと | |
| 証跡 | |
| 判断したこと | |
| 確認方法と結果 | |
| 説明のポイント | |
| 残課題・次にやることの提案 | |

---

## 完了条件と対応状況

| 完了条件 | 状態 |
|----------|------|
| 学習メモ2本・README・ai-usage.md を入れた develop 宛ての PR ができている | ⬜ |
| 公開チェックで問題がない | ✅ |
| reviewer サブエージェントで確認し、結果を `docs/04-quality/reviews/TASK-H01-review.md` に書いている | ⬜ |
| progress.md の完了報告欄を埋め、ステータスと「状態」を「🔵 レビュー待ち（PR #番号）」にしている | ⬜ |

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

## 動作確認

- テスト：アプリのコードがまだないため対象外（lint も同じ）
- 手動確認：README の一覧のリンク先（nextjs.md・nodejs-dev-environment.md・authentication.md・amplify-hosting.md・aurora-data-api.md）がすべて `docs/learning/` にあることを確認

## 残課題・次への引き継ぎ

- 秘密情報の置き場所：CLAUDE.md は「本番は Amplify の環境変数」だが、amplify-hosting.md では AWS が環境変数に置かないよう勧めていると整理し、TASK-P06 で決め直すとしている。P06 で変えたら CLAUDE.md も直す
- Next.js 16 は Amplify の公式の対応表にないので、TASK-E01 のスパイクで動作を確かめる
- `docs/01-product/2026-11-20-architecture-review.md` と `docs/03-engineering/adr/` はコミットしていない（扱いは TASK-P06 で決める）
