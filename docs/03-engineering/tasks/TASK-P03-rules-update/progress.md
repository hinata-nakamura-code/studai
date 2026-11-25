# TASK-P03 — 運用ルールをリポジトリに反映し、GitHub の運用を始める

| 項目 | 内容 |
|------|------|
| タスクID | TASK-P03 |
| 担当 | Dev - Engineer（Claude Code） |
| フェーズ | MVP準備 |
| ブランチ | docs/TASK-P03-rules-update |
| PR | https://github.com/hinata-nakamura-code/studai/pull/1 |
| ステータス | 🔵 レビュー待ち |

---

## 完了報告（タスクを終えたとき・止まったときに書く）

> PM はまずこの欄を読む。埋めるまで完了にしない。

| 項目 | 内容 |
|------|------|
| 状態 | 完了（PR 作成済み・レビュー待ち。マージはしていない） |
| やったこと | GitHub：main を初めて push、develop を作って既定ブランチにし、main と develop を保護（PR 必須・承認0・管理者にも適用・force push と削除を禁止）。リポジトリ：`docs/rules.md` を追加、`CLAUDE.md`・`docs/templates/progress.md` を置き換え、`docs/README.md` を rules.md 中心に整理、`docs/learning/README.md` を追加、`docs/ai-usage.md` に P01〜P03 を追加、`docs/01-product/carry-over.md` を追加、`.claude/agents/reviewer.md` を develop 宛ての運用に合わせた |
| 証跡 | PR：https://github.com/hinata-nakamura-code/studai/pull/1 ／ レビュー記録：`docs/04-quality/reviews/TASK-P03-review.md` ／ `git diff develop --stat`：10 files changed, 530 insertions(+), 94 deletions(-)（reviewer.md、CLAUDE.md、carry-over.md、progress.md、TASK-P03-review.md、docs/README.md、ai-usage.md、learning/README.md、rules.md、templates/progress.md） |
| 判断したこと | 下の「判断したこと」を参照（CLAUDE.md の3点と reviewer.md は作者・PM の判断、README の整理と reviewer の指摘への対応は Claude Code の判断） |
| 確認方法と結果 | 公開チェック：問題なし（0件）。CLAUDE.md は79行で、13行目の `@docs/rules.md` で読み込む（パスとファイルの存在を確認）。2026-11-20 の下書き3ファイルは未追跡のまま。**`@docs/rules.md` が実際に読み込まれるかは未確認**：このセッションは CLAUDE.md を置き換える前に始まっていて、サブエージェントにも古い CLAUDE.md が渡るため確かめられなかった。新しいセッションで「rules.md の『9. 費用』の目標を、ファイルを読まずに答えて」と聞いて確かめる |
| 説明のポイント | ① ルールの正本を rules.md の1か所にし、CLAUDE.md・docs/README.md からは読み込み・案内だけにした（二重に書くと食い違うため）② main と develop を保護し、承認数は0にした（1人開発なので自分の PR を自分でマージできるようにしつつ、PR を通すことを強制する）③ 仕様の優先順位で、承認済みの要件定義を引き継ぎ事項より上にした（引き継ぎ事項は要件定義の材料のため） |
| 残課題・次にやることの提案 | ① 新しいセッションで rules.md が読み込まれるか確かめる ② 範囲外の直し（README.md・reviews/README.md の例・settings.json の不要な権限）を小さな docs/chore タスクにする ③ ai-usage.md の P01〜P03 の「理由」を公開してよいか、作者が確認する |

---

## 完了条件と対応状況

| 完了条件 | 状態 |
|----------|------|
| main と develop が GitHub にあり、既定ブランチが develop | ✅ |
| main と develop が保護されている（PR 必須・承認0・管理者にも適用・force push と削除を禁止） | ✅ |
| PR（docs/TASK-P03-rules-update → develop）ができている | ✅ |
| CLAUDE.md が200行以内で、docs/rules.md を読み込んでいる | ✅（79行・読み込みの行あり。実際の読み込みは新しいセッションで未確認） |
| 公開チェック：問題なし（0件） | ✅ |
| 2026-11-20 の下書き3ファイルが未コミットのまま残っている | ✅ |
| progress.md の完了報告欄が埋まっている | ✅ |

## 計画（プランモードで決めたこと）

- A. GitHub の準備：未コミットのファイルと最初のコミットを確認 → main を push → develop を作って push・既定ブランチにする → 設定内容を確認してからブランチ保護
- B. ルールの反映：drafts の3ファイルを読み、矛盾を指摘してから反映 → docs/README.md・learning/README.md・ai-usage.md を直す → reviewer.md を直す → reviewer サブエージェント → push・PR

## 判断したこと

| 判断 | 理由 | ADR |
|------|------|-----|
| CLAUDE.md の「1タスクの進め方」で、ADR の下書き・ai-usage.md の追記をマージの後から reviewer の前に移した | マージの後だと ADR と ai-usage.md が PR に入らず、レビューもされないため（作者・PM が承認） | — |
| 仕様の優先順位を「① CLAUDE.md と rules.md ② 作者が承認した要件定義・基本設計（develop に入ったもの）③ carry-over.md（②がないところ）」にした | 後から作る要件定義が、その材料である引き継ぎ事項より下になっていたため（作者・PM が決めた文） | — |
| CLAUDE.md の技術構成の見出しを「確定」から「2026-11-20 に決めて引き継いだもの。TASK-H01 で理解し、TASK-P06 で ADR を作り直す」に変えた。中身はそのまま | rules.md の「理解してから進める」と、carry-over.md の対応タスク（H01 → P06）に合わせるため | — |
| .claude/agents/reviewer.md をこのタスクの範囲に入れて直した（比較先を PR の宛先ブランチに、テストと lint は CLAUDE.md の「コマンド」に従う、backend 前提の記述を消す） | develop を宛先にする運用と、FastAPI を使わない構成に合わせるため（作者・PM が範囲に追加） | — |
| docs/README.md は記録の置き場所の表を消し、rules.md の「7.」への案内にした | 正本を rules.md の1か所にして、二重管理で食い違わないようにするため | — |
| docs/private/drafts/ の下書きには1〜4の直しを反映しない | これからはリポジトリの方を正とするため（作者の指示） | — |
| reviewer の指摘で、rules.md の 5-1 手順3に「依頼文で指示されていれば」「実行は作者が承認する」を足した | 同じファイルの 6. と CLAUDE.md と食い違っていたため。新しい決まりではなく、表記を揃えただけ | — |
| CLAUDE.md のメンテナンス用メモで、書かない事情の例を rules.md の 8. と同じ並びにした | 公開リポジトリで、具体的な事情を例に挙げると、事情があること自体が読み取れてしまうため | — |

## 作業ログ

- 2026-11-25：未コミットのファイルと最初のコミットを確認（公開チェック：問題なし（0件））。origin を設定して main を push。develop を作って push し、既定ブランチにした。設定内容の確認を取り、main と develop を保護した
- 2026-11-25：drafts の3ファイルを読み、3点（作業の順番、仕様の優先順位、技術構成の見出し）を指摘。作者・PM の回答をもとに反映した
- 2026-11-25：reviewer.md の差分を作者に見せて承認を得て反映。reviewer サブエージェントで確認し、中1件・低1件を直した。push して PR #1 を作成

## 動作確認

- テスト：対象外（ドキュメントと設定だけの変更）
- 手動確認：GitHub で既定ブランチが develop であること、main と develop の保護設定（API の応答）、PR #1 の作成を確認

## 残課題・次への引き継ぎ

- `docs/03-engineering/adr/README.md` は、TASK-P06 で新しく作る。それまで未追跡の `adr/` を add しない（2026-11-20 の下書きが入ってしまうため）
- 範囲外の直し（提案）：README.md がルールの置き場所として rules.md を案内していない／`docs/04-quality/reviews/README.md` の記入例に backend 前提のパスが残っている／`.claude/settings.json` に、今の構成で使わないコマンドの権限設定が残っている
- ai-usage.md の P01〜P03 の項目の「理由」は、作者自身のことを書いた文になっている。公開してよいか作者が確認する（reviewer の指摘。作者が指定した文なので、そのままにした）
