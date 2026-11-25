# ログインの仕組み（Auth.js・Better Auth・Cognito）

- 関係するタスク：TASK-H01（技術構成を理解する）の2回目。ライブラリを選び直すかは TASK-P06（技術構成の ADR）で決める
- まとめた日：2026-11-25（作者がまとめ、Claude が確認）

## 作者のまとめ（3行）

1. Auth.js は、Next.js の中で Google などのログイン処理を実装できるライブラリ
2. セッションは JWT 方式がいい。DB 方式だと毎回 DB（Aurora）を見に行く必要があり、費用がかさんでしまうため
3. Auth.js は今は保守だけの状態になっており、Better Auth に切り替えるほうが安全なのでこっちがいいと思う（懸念点はある）。ただ、責任分界点的に Amazon Cognito のほうがいいのではとも思った

## 用語

- 認証：「あなたは誰か」を確かめること（ログイン）。認可は「その人に何をさせてよいか」を決めること
- OAuth／OpenID Connect：本人確認を Google などに任せる仕組み。アプリはパスワードを預からない
- Cookie：ブラウザがサイトごとに保存し、アクセスのたびに自動で送る小さなデータ
- セッション：ログインした状態を、次にページを開いたときも覚えておく仕組み
- JWT：書き換えられないように封をしたデータ。サーバーだけが持つ鍵で封をして、サーバーで確かめる
- proxy：ページを表示する前に必ず通る関所（Next.js 15 までは middleware）

## 何者か、なぜ必要か

ログインのためのライブラリ。Google との OAuth のやり取り、Cookie の発行、なりすまし対策（CSRF）など、自分で書くと間違えやすい部分を引き受ける。

Google ログインの流れ：

1. 利用者が「Google でログイン」を押す
2. Google の画面に移り、Google にログインして同意する
3. Google が「この人は〇〇です」という証明をアプリに返す
4. ライブラリが証明を確かめて、セッションの Cookie を発行する
5. その後は、ページを開くたびに Cookie を見て「ログイン済みか」「誰か」を判断する

## セッションの持ち方

| | JWT 方式 | データベース方式 |
|---|---|---|
| ログイン状態をどこに持つか | Cookie の中（封をした JWT） | DB のテーブル。Cookie には番号だけ |
| 確認のたびに DB を見るか | 見ない | 見る |
| 強制的にログアウトさせられるか | 期限が切れるまでできない | DB から消せばすぐできる |
| Aurora（自動停止）との相性 | 良い。ログインの確認で Aurora を起こさない | 悪い。ページを開くたびに Aurora を起こし、費用と再開の待ち時間（約15秒）が増える |

JWT 方式の弱点（期限前に無効にできない）は、有効期限を短くして自動で更新することで小さくする。

## 他の選択肢との違い

| 選択肢 | どんなものか | 良い点 | 気になる点 |
|---|---|---|---|
| Auth.js v5 | Next.js の中で動くライブラリ | 解説記事が多い。JWT 方式なら DB なしで動く | 2025年9月から Better Auth のチームが保守し、セキュリティ修正と急ぎの不具合対応だけになった。v5 は beta のまま |
| Better Auth | 同じく Next.js の中で動くライブラリ | 開発が続いている。DB なしでセッションを持てる。ゲスト用の部品（Anonymous）がある | 新しい。DB を使う場合に Aurora の Data API につなげるかは未確認 |
| Amazon Cognito | AWS のログインサービス | AWS の標準サービス。Google などでログインする利用者は月1万人まで無料 | AWS 側の設定が増える。Next.js からつなぐ部品も別に要る |
| Clerk などの外部サービス | ログインを丸ごと任せるサービス | 一番早く作れる | AWS の外に利用者のデータを預ける |

### 責任分界点で比べる（Better Auth と Cognito）

| 誰が責任を持つか | Better Auth＋Google | Cognito＋Google |
|---|---|---|
| パスワード・二段階認証・不正ログインの検知 | Google | Google |
| Google との OAuth のやり取り | ライブラリ（アプリのコードの中） | Cognito（AWS） |
| ログイン状態の証明（トークンの発行と、その鍵の管理） | アプリ（`AUTH_SECRET` を守る） | Cognito（AWS） |
| ユーザーの一覧 | アプリの DB | Cognito のユーザープール（アプリの DB にも別に持つ） |
| Next.js 側の Cookie の扱い | ライブラリ | ライブラリ（Next.js 用の部品が別に要る） |
| ライブラリの更新 | アプリ | アプリ（Next.js 側の部品） |
| 設定の量 | Google の設定と、コードのファイル数個 | 左に加えて、ユーザープール・アプリクライアント・ドメイン・Google 連携の設定 |

- 一番重い責任（パスワードと二段階認証）は、Google ログインだけにした時点でどちらも Google 側にある
- Cognito にすると AWS に移るのは、トークンの発行と鍵の管理、ユーザーの一覧
- Cognito が向くのは、メールとパスワードのログインを足すとき、複数のアプリでユーザーを共有するとき、組織として AWS で ID を管理する決まりがあるとき

## Studai でどう使うか（どれを選んでも同じ部分）

- ログインは Google だけ
- セッションは JWT 方式（Aurora の自動停止と相性が良い）
- ログインが必要なページは proxy で守る。DB を読み書きする Server Component や Server Action の中でも「誰か」を確かめる（関所だけに頼らない）
- 初めてログインした人は、Google のアカウント ID をもとに Studai のユーザーとして登録する（テーブルは基本設計で決める）
- ゲストモード（Google アカウントなしで触れる）の仕組みが別に要る
- 秘密情報（ライブラリの鍵、Google のクライアント ID・シークレット）は、ローカルは `.env`、本番は Amplify の環境変数に置く

## 決めること

- ライブラリ（Auth.js のままか、Better Auth か、Cognito か）は TASK-P06 の ADR で決める。Next.js 16＋Amplify で動くかは TASK-E01 のスパイクで確かめる

## 参考

- [Auth.js is now part of Better Auth](https://better-auth.com/blog/authjs-joins-better-auth)
- [Better Auth 1.4（stateless なセッション）](https://better-auth.com/blog/1-4)
- [Amazon Cognito pricing](https://aws.amazon.com/cognito/pricing/)
- [npm: next-auth](https://www.npmjs.com/package/next-auth)
- [npm: better-auth](https://www.npmjs.com/package/better-auth)
