# TASK-P17 — 運用ルールの変更（push・PR・マージの進め方）

| 項目 | 内容 |
|------|------|
| タスクID | TASK-P17 |
| 担当 | Dev - Engineer（Claude Code） |
| フェーズ | MVP準備 |
| ブランチ | docs/TASK-P17-push-rules |
| PR | （作成したら URL を書く） |
| ステータス | 🟡 進行中 |

---

## 完了報告（タスクを終えたとき・止まったときに書く）

> PM はまずこの欄を読む。埋めるまで完了にしない。

| 項目 | 内容 |
|------|------|
| 状態 | （PR を作ったら書く） |
| やったこと | `docs/rules.md` と `CLAUDE.md` を PM の下書き（docs/private/drafts/TASK-P17/）で置き換えた（中身は変えていない）。`docs/ai-usage.md` の「記録」の先頭に TASK-P17 の項目を追加。作者が直した `.claude/settings.json`（ask から `git push` と `gh pr create` を消した2行）を同じ PR に含めた |
| 証跡 | PR：（作成したら書く） ／ レビュー記録：`docs/04-quality/reviews/TASK-P17-review.md` |
| 判断したこと | 下書きに直したほうがよい点があっても直さず、レビュー記録と残課題に書いた（依頼が「中身は変えない」のため）。ai-usage.md の「理由」は依頼文に書かれていないため、変更の内容から書いた（作者の確認が要る） |
| 確認方法と結果 | 下書きと置き換えたファイルの差分（改行コードを除く）がないこと。`git diff --stat develop` が依頼の5ファイル＋settings.json だけ。テスト・lint はアプリのコードがまだないため対象外 |
| 説明のポイント | ① push・PR の作成は作者の承認なしで行い、マージは OK を出したレビューだけが行う（作った役割は自分でマージしない）② その線引きを指示文だけでなく権限設定でも表し、settings.json の ask に `gh pr merge` を残した ③ progress.md とレビュー記録を書く担当を「PR を作った役割」「レビュー」に変えた |
| 残課題・次にやることの提案 | レビュー記録の「食い違い」の表を参照（reviewer.md の書き方、ai-usage.md の冒頭、ひな形の記述など）。直すかどうかは PM が決める |

---

## 完了条件と対応状況

| 完了条件 | 状態 |
|----------|------|
| rules.md と CLAUDE.md が下書きと同じ中身になっている | ✅ |
| ai-usage.md の「記録」の先頭に TASK-P17 の項目がある | ✅ |
| reviewer サブエージェントで確認し、レビュー記録に食い違いを書き出している | ⬜ |
| `git diff --stat develop` が rules.md・CLAUDE.md・ai-usage.md・progress.md・レビュー記録（＋settings.json）だけ | ⬜ |
| develop 宛ての PR ができ、ステータスを「🔵 レビュー待ち（PR #番号）」にしている | ⬜ |

## 計画（依頼文で決まっていたこと）

- develop を最新にして `docs/TASK-P17-push-rules` を作る
- 下書きで rules.md と CLAUDE.md を置き換える（中身は変えない）
- 作者が settings.json を直していたら、その2行も含める
- `docs/01-product/2026-11-20-architecture-review.md`、`docs/01-product/requirements.md`、`docs/03-engineering/adr/` はこの PR に入れない

## 判断したこと

| 判断 | 理由 | ADR |
|------|------|-----|
| 下書きは改行コード以外まったく同じにする | 依頼が「中身は変えない」のため。改行コードは Git が保存時に揃える | — |
| ai-usage.md の「理由」を変更の内容から書いた | 依頼文に理由がなかったため。作者に確認してもらう | — |

## 作業ログ

- 2026-11-26：develop（df8786d）が origin と同じであることを確認し、ブランチを作成。settings.json の作者の変更が依頼どおりの2行だけであることを確認。rules.md・CLAUDE.md を置き換え、ai-usage.md に項目を追加

## 動作確認

- `diff --strip-trailing-cr docs/private/drafts/TASK-P17/rules.md docs/rules.md`、CLAUDE.md も同じ：差分なし
- テスト・lint：アプリのコードがまだないため対象外

## 残課題・次への引き継ぎ

- 完了報告の「残課題」を参照
