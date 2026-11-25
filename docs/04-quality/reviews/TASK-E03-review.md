# TASK-E03 レビュー記録

## 1回目（reviewer サブエージェント・2026-11-25）

対象：chore/TASK-E03-remove-old-stack-leftovers → develop（コミット 9459076）。判定：このまま PR にしてよい（低の指摘は PR までに直すとよい）。

| 重要度 | ファイル:行 | 指摘 | 判断 | 反映 |
|---|---|---|---|---|
| 低 | docs/03-engineering/tasks/TASK-E03-remove-old-stack-leftovers/progress.md（完了報告） | 完了報告欄が空で、完了条件も ⬜ のまま | 直す。CLAUDE.md の手順どおり PR の作成後に埋める | このPR（PR 作成後に記入） |
| 低 | 同上（計画・動作確認の grep の範囲） | grep の対象外として、未追跡の docs/01-product/plan.md が書かれていない | 直す。plan.md も対象外と書き、確認して該当なしと記録した | このPR |
| 低 | .claude/settings.json（末尾） | ファイル末尾の改行がなくなり、目的と関係のない差分が増えている | 直す | このPR |
| 低 | progress.md（完了条件） | レビュー記録の行が完了条件にない。レビュー記録もまだない | 直す。このファイルを作り、完了条件に1行足した | このPR |

- settings.json の JSON の確認は、reviewer の環境では権限の判定で止められて未確認。作成者の環境で PowerShell の ConvertFrom-Json により確認済み（progress.md の「動作確認」）
- 範囲外の古い記述：見つからなかった（FastAPI・Python・Lambda なども探した）

## 2回目（claude.ai【レビュー】・2026-11-25・PR #2）

対象：836ab4f まで。判定：マージしてOK（低1件はマージ前のコミットで直す。再レビュー不要）。

| 重要度 | ファイル | 指摘 | 判断 | 反映 |
|---|---|---|---|---|
| 低 | docs/03-engineering/tasks/TASK-E03-remove-old-stack-leftovers/progress.md（ステータス） | このままマージすると「レビュー待ち」のまま残り、TASK-P03 で直したのと同じ状態になる | 直す | このPR（マージ前のコミット） |
| 低 | docs/rules.md（5-1 の手順7） | マージ前のコミットで progress.md のステータスを完了にする手順がなく、同じことが続く | ルール変更として PM に渡す | PM |
| 参考 | .claude/settings.json（deny） | deny は ask・allow より先に判定され、Edit(.env.*) は .env.example にも当たる。! で例外を作る書き方は公式ドキュメントに見当たらず、rules.md 8. の「.env.example にキー名だけ足す」を Claude Code ができない可能性が高い（変更前からの設定で、このPRの範囲外） | 今回は直さない。環境変数を初めて足すタスクの前に PM が扱う。禁止を緩める変更なので作者が行う | PM |

確認したこと：
- settings.json：残すべき禁止（.env 系・*.pem）と確認（git push・gh pr create・gh pr merge・aws）が残っている。外したのは今の構成で使わないものだけで、確認・禁止を外す4行は作者が消した。JSON として正しく、末尾の改行もある（1回目で未確認だった JSON の確認を補った）
- grep（FastAPI・Lambda なども含めて探した）：残っているのは「使わない」の一覧、費用のルール、.gitignore の Python の節、経緯の記述だけ
- 公開してよい内容か：差分と3つのコミット（作成者を含む）に、個人的な記述・ローカルのパス・Notion の ID・メールアドレスはない
- 仕様との食い違い：依頼文の範囲どおり。未追跡の docs/01-product/plan.md は入っていない
