# TASK-P17 レビュー記録

## 1回目（reviewer サブエージェント・2026-11-26）

対象：docs/TASK-P17-push-rules → develop（最初のコミットまで）。判定：このまま PR にしてよい。差分は下書きどおりで依頼の範囲に合っている。指摘はどれも下書きの中身かほかのファイルに関するもので、このタスクでは直さない（依頼が「中身は変えない」のため）。

### rules.md・CLAUDE.md・settings.json の食い違いと運用で困りそうな点

| 重要度 | ファイル:行 | 指摘 | 判断 | 反映 |
|---|---|---|---|---|
| 高 | .claude/settings.json:7-10、CLAUDE.md:60 | ask から消しただけでは「作者の承認は要らない」にならないおそれがある。allow には `npm run *` しかないので、権限モードによっては `git push` と `gh pr create` で確認が出続ける（作者がふだん使う権限モードは未確認）。ai-usage.md の「確認を外し」も実際の動きと合わない可能性がある | 直さない。allow に足すかを作者が決める（設定を緩める変更は作者が行う） | 作者 |
| 高 | docs/rules.md:16, 62, 70-71, 105, 160 | claude.ai のプロジェクト（PM・デザイン・レビュー）が自分で push・PR 作成・PR へのコミット・マージ・ブランチ削除・タグ付けをする前提。claude.ai からリポジトリに書き込めるかは未確認。書き込めないと作者が手作業で代わりにやることになり、ルールの文面と実際の運用がずれる | 直さない。書き込めるかを作者が確かめ、書き込めないなら作者が代わりに実行することを PM がルールに足す | 作者・PM |
| 中 | CLAUDE.md:61、docs/rules.md:59 | PR を作ったあとに progress.md を「🔵 レビュー待ち」に直す手順に「コミットして push する」がない（以前はマージのときにまとめてコミットしていたが、その手順がなくなった）。レビューが古い progress.md を読むおそれがある | 直さない。PM に伝える。今回の2つの PR では、直した progress.md をコミットして push する | PM |
| 中 | CLAUDE.md:62、docs/rules.md:61 | レビューがレビュー記録を同じ PR ブランチにコミットするので、Claude Code が pull せずに直して push すると拒否される。push に確認がなくなったので、`--force` で上書きしても止める仕組みがない | 直さない。手順 9 に「直す前に pull する」を足すか PM が決める。`git push --force*` を deny か ask にするかは作者が決める | PM・作者 |
| 中 | docs/rules.md:71 と 104 | 5-2 の「レビューが自分で作った PR は、作者が確認してからマージする」では誰がマージするのか分からず、6. の「PR を作った役割は自分でマージしない」とぶつかって読める | 直さない。PM に伝える | PM |
| 中 | .claude/settings.json:8、CLAUDE.md:62 | CLAUDE.md では「Claude Code は PR をマージしない」だが、`gh pr merge` は ask のままで、作者が承認すればマージできる | 直さない。ask に残したのは作者の判断。deny に移すかは作者が決める | 作者 |
| 中 | docs/rules.md:103, 105 | 承認なしでよいのは「develop 宛て」の PR だけ。リリースの develop → main の PR と、hotfix（main と develop の両方に入れる）を誰が作り誰がマージするかが書かれていない | 直さない。PM に伝える | PM |

### ほかのファイルとの食い違い

| 重要度 | ファイル:行 | 指摘 | 判断 | 反映 |
|---|---|---|---|---|
| 低 | docs/ai-usage.md:15、7-11 | 冒頭の「push 前の確認は権限設定で強制する」が古くなった。役割分担の表にもレビューがマージすることが入っていない | 直さない。別のタスクで直すか PM が決める | PM |
| 低 | docs/templates/progress.md:6, 10, 36 | progress.md は「PR を作った役割」が書くことになったが、ひな形は担当が「Dev - Engineer（Claude Code）」で固定、見出しも Claude Code 向け。「マージ前に ✅ に直す」も誰が直すか（rules.md ではレビュー）が書かれていない | 直さない。別のタスクで直すか PM が決める | PM |
| 低 | docs/04-quality/reviews/README.md:6 | 書く人が「reviewer サブエージェント（PR 前）」だが、reviewer.md はファイルを変更しない決まりで、rules.md ではサブエージェントの結果を書くのはエンジニア。今回より前からある食い違い | 直さない。別のタスクで直すか PM が決める | PM |
| 低 | docs/ai-usage.md:25 | 「理由」は依頼文になく、Claude Code が変更の内容から書いた | 作者に確認してもらう（PR 本文にも書く） | 作者 |

- .claude/agents/reviewer.md と新しいルールの間には食い違いなし（PR を作る前に使う、ファイルは変更しない、結果を書くのはエンジニア）
- 確認できたこと：rules.md・CLAUDE.md が下書きと同じ（`diff --strip-trailing-cr` で差分なし）、settings.json の差分が依頼どおりの2行だけ、未追跡の下書き（architecture-review.md・adr/・requirements.md）がコミットに入っていない、公開してはいけない内容（個人的な事情、勤務先、ローカルのパス、Notion の ID、メールアドレス、秘密情報の値）がない
- テスト・lint：アプリのコードがまだないため対象外

## 2回目（claude.ai【レビュー】・2026-11-26・PR #7）

判定：直してからマージ。1回目の指摘にはすべて同意。下の表のとおりこの PR で直す。

| 重要度 | ファイル:行 | 指摘 | 判断 | 反映 |
|---|---|---|---|---|
| 高 | .claude/settings.json | ask から消しただけでは、通常のモードで push・PR 作成の確認が出続け、ルールの「承認なし」と動きがずれる | allow に push と gh pr create を足し、force push は deny にする（作者承認） | deny はコミット c1067b0。allow の3行は、Claude Code が自分の権限を広げる変更になるため作者が足した（このPR） |
| 高 | rules.md:16, 62, 71, 104／CLAUDE.md:62 | マージの操作ができるのがレビュー（claude.ai）だけ。claude.ai から書き込めるかは未確認で、書き込めないとマージが止まる | マージを決める役割と操作する役割を分け、書き込めないときは作者の指示で Claude Code が操作する手順を足す。gh pr merge は ask に残す | コミット c1067b0 |
| 中 | CLAUDE.md:61、rules.md:59 | progress.md を「レビュー待ち」にしたあと、commit・push する手順がない | 手順に足す | コミット c1067b0 |
| 中 | CLAUDE.md:62 | レビューも同じブランチにコミットするので、pull せずに直すと push が拒否される | 「pull してから直す。force push はしない」と書く | コミット c1067b0 |
| 中 | rules.md:71 | 「作者が確認してからマージ」では誰がマージするか分からず、104 行目とぶつかる | 新しいレビューのチャットで確認してからマージする、に変える | コミット c1067b0 |
| 低 | rules.md:97, 105 | hotfix の PR とマージの扱いがない | 6. に1行足す | コミット c1067b0 |
| 低 | ai-usage.md:15、templates/progress.md:6, 10、reviews/README.md:6 | 新しいルールと食い違う古い記述 | この PR で揃える | コミット c1067b0 |
| 参考 | ai-usage.md:25 | 「理由」は Claude Code が書いた | 作者が確認する | 作者 |

3回目（2026-11-26・eb59392）：allow の3行を確認。マージしてOK
