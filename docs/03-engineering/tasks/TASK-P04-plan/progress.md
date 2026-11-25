# TASK-P04 — 計画（plan.md）をコミットし、関係する記述を合わせる

| 項目 | 内容 |
|------|------|
| タスクID | TASK-P04 |
| 担当 | Dev - Engineer（Claude Code） |
| フェーズ | MVP準備 |
| ブランチ | docs/TASK-P04-plan |
| PR | https://github.com/hinata-nakamura-code/studai/pull/3 |
| ステータス | 🔵 レビュー待ち |

---

## 完了報告（タスクを終えたとき・止まったときに書く）

> PM はまずこの欄を読む。埋めるまで完了にしない。

| 項目 | 内容 |
|------|------|
| 状態 | 完了（PR 作成済み・レビュー待ち。マージはしていない） |
| やったこと | `docs/01-product/plan.md`（PM 作成）を中身を変えずに追加。`docs/README.md` の「計画」を plan.md へのリンクに、`CLAUDE.md` の「プロジェクト概要」で plan.md を案内。`docs/01-product/carry-over.md` の A3・A5・B10 のメモ欄に 2026-11-25 の追記。`docs/ai-usage.md` に TASK-P04 の項目を追加 |
| 証跡 | PR：https://github.com/hinata-nakamura-code/studai/pull/3 ／ レビュー記録：`docs/04-quality/reviews/TASK-P04-review.md` |
| 判断したこと | 下の「判断したこと」を参照（plan.md の食い違いは直さずに PM に渡す、carry-over.md は以前のメモを残して追記、ai-usage.md の理由は作者の言葉のまま） |
| 確認方法と結果 | 公開チェック：問題なし（0件）。git status で、2026-11-20 の下書き（architecture-review.md、adr/）が未追跡のまま残っていることを確認。reviewer サブエージェントの指摘（中2件）を反映。テスト・lint はまだないため対象外 |
| 説明のポイント | ① 計画は工数・日程を見積もらず、段階と依存関係だけを決めた（作者の判断）② 引き継ぎ事項（carry-over.md）は「決まっていたこと」を書き換えず、メモ欄に日付つきで経緯を足した（以前の決定も残すため）③ PM が作った資料は中身を変えずに入れ、食い違いは指摘として PM に返した（書く担当を分けるため） |
| 残課題・次にやることの提案 | ① plan.md への指摘（P05 と P15 の依存、図と表のずれ、階層 Todo の書き方、Phase2 のサンプルコード生成、E03 の扱い、段階2の目印）を PM が直す ② carry-over.md C10 のメモにも学習カレンダーを MVP に移した追記が要るか、PM が決める |

---

## 完了条件と対応状況

| 完了条件 | 状態 |
|----------|------|
| plan.md を中身を変えずにコミットしている | ✅ |
| docs/README.md の「計画」が plan.md へのリンクになっている | ✅ |
| CLAUDE.md の「プロジェクト概要」が plan.md を案内している | ✅ |
| carry-over.md の A3・A5・B10 のメモ欄に追記している（「決まっていたこと」は変えない） | ✅ |
| ai-usage.md に TASK-P04 の項目がある | ✅ |
| 1〜6 が入った develop 宛ての PR ができている | ✅ |
| 公開チェック：問題なし（0件） | ✅ |
| コミットしないもの（2026-11-20 の下書き）が未追跡のまま残っている | ✅ |
| レビュー記録（docs/04-quality/reviews/TASK-P04-review.md）がある | ✅ |
| この progress.md の完了報告欄が埋まっている | ✅ |

## 計画（プランモードで決めたこと）

- 依頼文の1〜6を反映し、reviewer の結果をレビュー記録に書く
- plan.md は中身を変えない。食い違いは残課題に書いて PM に渡す
- carry-over.md は、いまのメモを残したまま末尾に追記する

## 判断したこと

| 判断 | 理由 | ADR |
|------|------|-----|
| plan.md の Phase2 に「サンプルコード生成」がない点は、plan.md を直さずに残課題として PM に渡す | 依頼文で plan.md の中身は変えないことになっているため | — |
| carry-over.md のメモ欄は、いまの文を残して「。2026-11-25：…」と末尾に足した | 決まっていたことの経緯（以前の予定）も残すため | — |
| ai-usage.md の「理由」は作者の言葉のまま書いた | 依頼文で作者に確認して書くことになっているため | — |

## 作業ログ

- 2026-11-25：plan.md を読み、食い違い1件（Phase2 のサンプルコード生成）を指摘。計画を示し、ai-usage.md の理由を作者に確認した
- 2026-11-25：develop を最新にしてブランチを作成。README.md・CLAUDE.md・carry-over.md・ai-usage.md を直した
- 2026-11-25：reviewer サブエージェントで確認（判定：中2件を済ませれば PR にしてよい）。レビュー記録を作り、plan.md の指摘6件と carry-over.md C10 は PM に渡すことにした
- 2026-11-25：push して PR #3 を作成し、完了報告欄を埋めた

## 動作確認

- テスト：対象外（ドキュメントだけの変更）
- 手動確認：公開チェック：問題なし（0件）。git status で、docs/01-product/2026-11-20-architecture-review.md と docs/03-engineering/adr/ が未追跡のまま残っていることを確認（PM が置いた plan.md は、未追跡のファイルを中身を変えずに add した）

## 残課題・次への引き継ぎ

- plan.md の「4. 段階5：Phase2」に、carry-over.md B10 の「サンプルコード生成」（AI 要約と一緒の項目）がない。落としたのが意図どおりか、PM が確認する
- carry-over.md C10（学習カレンダーの記録方法）のメモが「2月以降」のままで、B10 の追記と食い違う。範囲外なので直していない
- plan.md への reviewer の指摘（P05 と P15 の依存、mermaid 図と表のずれ、階層 Todo が3階層前提に見える、E03 の扱いと「理解」の列、段階2の目印）。詳しくは docs/04-quality/reviews/TASK-P04-review.md の「B.」
