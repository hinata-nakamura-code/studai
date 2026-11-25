# TASK-E03 — 古い構成の名残りを直す

| 項目 | 内容 |
|------|------|
| タスクID | TASK-E03 |
| 担当 | Dev - Engineer（Claude Code） |
| フェーズ | MVP準備 |
| ブランチ | chore/TASK-E03-remove-old-stack-leftovers |
| PR | （作成したら URL を書く） |
| ステータス | 🟡 進行中 |

---

## 完了報告（タスクを終えたとき・止まったときに書く）

> PM はまずこの欄を読む。埋めるまで完了にしない。

| 項目 | 内容 |
|------|------|
| 状態 | |
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
| .claude/settings.json から pytest・terraform・tfstate の設定が消えている | ✅ |
| CLAUDE.md の「秘密情報」から tfstate が外れている | ✅ |
| docs/04-quality/reviews/README.md の記入例が今の構成に合っている | ✅ |
| .gitignore の Python の節の見出しが「(spikes)」になっている | ✅ |
| TASK-P03 の progress.md がマージ済みの表記になっている | ✅ |
| grep の結果が、経緯の記述と残してよい行だけになっている | ✅ |
| .claude/settings.json が JSON として正しい | ✅ |
| この progress.md の完了報告欄が埋まっている | ⬜ |

## 計画（プランモードで決めたこと）

- 依頼文の5点を直す。あわせて、このタスクの progress.md、レビュー記録、ai-usage.md の1項目を作る
- grep の完了条件では、CLAUDE.md:42（「使わない」の一覧）と .gitignore:34（`.pytest_cache/`。Python の節は残す指示）も残ってよい行として扱う（作者が承認）
- 未コミットの 2026-11-20 の下書き（architecture-review.md、adr/）は grep の対象から外す

## 判断したこと

| 判断 | 理由 | ADR |
|------|------|-----|
| CLAUDE.md:42 と .gitignore:34 は grep に出ても残す | 前者は使わないものの一覧で、後者は残す指示のある Python の節の中身のため（作者が承認） | — |
| reviews/README.md の記入例は `（例）src/xxx.ts:42` にした | Next.js（TypeScript）の構成に合わせ、フォルダ構成が決まっていないので「（例）」で例だと分かるようにした | — |
| settings.json の確認・禁止を外す4行（ask の terraform apply、deny の tfstate 2つと terraform destroy）は、Claude Code ではなく作者が消す | Claude Code が自分の権限設定を緩める変更は、依頼があってもしない決まりのため。制限を強める変更（allow の pytest を消す）は Claude Code が行った | — |
| JSON の確認は PowerShell の ConvertFrom-Json で行う | この PC に node がないため | — |

## 作業ログ

- 2026-11-25：develop を最新にしてブランチを作成。計画を示して OK をもらった。allow の pytest、CLAUDE.md、reviews/README.md、.gitignore、TASK-P03 の progress.md を直した
- 2026-11-25：作者が settings.json の4行を消した。JSON の確認と grep を行った

## 動作確認

- テスト：対象外（設定とドキュメントだけの変更）
- 手動確認：settings.json を PowerShell の ConvertFrom-Json で読み込めることを確認（JSON OK）
- grep（`grep -rniE "pytest|terraform|tfstate|backend"`。docs/private/ と未コミットの 2026-11-20 の下書きは除く）の結果：
  - `.gitignore:34`：`.pytest_cache/`（残してよい行。Python の節は残す）
  - `CLAUDE.md:42`：「使わない」の一覧（残してよい行）
  - `docs/01-product/carry-over.md:121`：F7 の経緯
  - `docs/ai-usage.md`：TASK-E03 の項目（経緯）
  - `docs/04-quality/reviews/TASK-P03-review.md:16`：過去のレビュー記録
  - `docs/03-engineering/tasks/TASK-P03-rules-update/progress.md`・このファイル：経緯の記述

## 残課題・次への引き継ぎ

-
