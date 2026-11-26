# TASK-H01 レビュー記録

## 1回目（reviewer サブエージェント・2026-11-26）

対象：docs/TASK-H01-learning-notes → develop（コミット前の差分と、コミットするファイル）。判定：必須の指摘なし。このまま PR にしてよい。

| 重要度 | ファイル:行 | 指摘 | 判断 | 反映 |
|---|---|---|---|---|
| 推奨 | docs/learning/amplify-hosting.md:53-64, 94 | CLAUDE.md の「秘密情報：本番は Amplify の環境変数」に対し、メモは AWS が環境変数に置かないよう勧めていることを書き、TASK-P06 で決め直すとしている。今は未決の扱いだが、P06 で変えたら CLAUDE.md も直す必要がある | メモは直さない。progress.md の残課題に書き、PM に伝える | このPR（progress.md） |
| 推奨 | docs/03-engineering/tasks/TASK-H01-learning-notes/progress.md（残課題） | 「残課題・次への引き継ぎ」が空 | 直す。秘密情報の置き場所、Next.js 16 の確認（TASK-E01）、コミットしなかった下書きの扱いを書いた | このPR |
| 任意 | docs/03-engineering/tasks/TASK-H01-learning-notes/progress.md（動作確認） | ひな形の「テスト：」「手動確認：」の形になっていない | 直す | このPR |
| 任意 | docs/learning/aurora-data-api.md:114 | 表の見出しは「料金（東京）」だが、Data API の行だけ us-east-1 の料金（行の中に明記はある） | 直さない。作者のメモで、費用の目安（ほぼ0）に影響しない | — |
| 任意 | docs/learning/aurora-data-api.md:36 | 再開にかかる時間が、メモは「最大15秒」、CLAUDE.md は「約15秒」 | 対応不要。意味はほぼ同じで、実装に影響しない | — |
| 任意 | docs/learning/amplify-hosting.md:31 | SSR のコンピュートの中身が Lambda と書かれていて、CLAUDE.md の「使わない：Lambda」と食い違って見える | 対応不要。「自分では触らない」と書いてあり、自分で Lambda を組まない方針と合っている。P06 の ADR で触れるかは PM が判断する | PM |

- 確認できたこと：README の一覧に指定の2行が指定どおりの文面で入り、一覧のリンク先がすべてあること。ai-usage.md の TASK-H01 の項目が「記録」の先頭にあり、既存の項目と同じ形であること。公開してはいけない内容（ローカルのパス、メールアドレス、Notion の ID、勤務先、個人的な事情、秘密情報の値）がないこと。費用の目安（Claude API を除き月500〜1,100円）が rules.md の目標と矛盾しないこと。Aurora・Data API の構成が CLAUDE.md と一致していること
- 確認できなかったこと：料金の数字を公式の料金表と照らすことはしていない
- テスト・lint：アプリのコードがまだないため対象外
