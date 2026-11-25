# Next.js

- 関係するタスク：TASK-H01（技術構成を理解する）の1回目
- まとめた日：2026-11-25（作者がまとめ、Claude が確認）

## 作者のまとめ（3行）

1. Next.js は Node.js 上で動く Web アプリを作るためのフレームワークである
2. Studai だと役割分担することなく、画面と API をまとめて使える
3. Server Component と Client Component が取り扱う範囲がごちゃごちゃにならないことに気をつける（サーバーで動く処理：DB から持ってくるなど／ブラウザで動く処理：クリックしてメモが出てくるなど）

## 何者か、なぜ必要か

- React（画面を部品に分けて作るライブラリ）に、Web アプリとして公開するのに必要なもの（URL ごとのページ、サーバーで動く処理、API、ビルド）を足したフレームワーク
- Node.js（JavaScript をブラウザの外で動かすソフト）の上で動く。Python で例えると、Node.js が Python 本体、Next.js が FastAPI のようなもの（ただし画面も作れる）
- 画面と API を、1つのプロジェクト・1つの言語（TypeScript）で書ける

## App Router の4つの部品

| 部品 | 何をするか | Studai での例 |
|---|---|---|
| `page.tsx` | ページ。フォルダの場所が URL になる | `app/dashboard/page.tsx` → `/dashboard` |
| Server Component（既定） | サーバーで動く画面部品。DB を読んで HTML を作る。秘密情報はブラウザに渡らない | 資格一覧を DB から読んで表示する |
| Client Component（先頭に `'use client'`） | ブラウザで動く部品。クリックや入力を担当する | Todo のチェックボックス、メモの入力欄 |
| Server Action／Route Handler | API。Server Action はボタンからサーバーの関数を直接呼ぶ。Route Handler は普通の REST API | タスクを完了にする、AI に問題を作らせる |

## 他の選択肢との違い

| 選択肢 | 内容 | Studai で選ばなかった理由 |
|---|---|---|
| React だけ（SPA） | 画面だけ。API は別に作る | API サーバーを別に持つことになり、管理するものが2つになる |
| Next.js＋FastAPI | 画面は Next.js、API は Python | 言語2つ・デプロイ2つ・CORS の設定が要る。費用と手間で見送った |
| Next.js だけ（採用） | 画面と API を1つにまとめる | 1人で回しやすい。Amplify Hosting にそのまま載せられる |

弱いところ：

- 覚えることが多い（サーバーとクライアントの区別、キャッシュ）
- 更新が速い。16.x（2025年10月公開）が今の主な版で、セキュリティ修正が定期的に出る
- Amplify Hosting の公式ドキュメントで SSR に完全対応と書かれているのは Next.js 15 まで。16 で動くかは TASK-E01 のスパイクで確かめる

## Studai でどう使うか

```
ブラウザ
   │ HTTPS
   ▼
Amplify Hosting
 ┌ Next.js アプリ ─────────────────────────┐
 │ 画面：Server Component（DB を読んで表示）  │
 │       Client Component（クリック・入力）    │
 │ API ：Server Action / Route Handler         │
 │ リポジトリ層：DB へのアクセスを1か所にまとめる │
 └──────────┬─────────────┬─────┘
                ▼                        ▼
     Aurora（Data API 経由）        Claude API
```

例：Todo のチェック → Client Component がクリックを受け取る → Server Action → リポジトリ層 → DB を更新 → 画面を描き直す

## 参考

- [Next.js Support Policy](https://nextjs.org/support-policy)
- [Amplify support for Next.js](https://docs.aws.amazon.com/amplify/latest/userguide/ssr-amplify-support.html)
