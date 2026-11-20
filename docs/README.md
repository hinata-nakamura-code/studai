# Studai 設計ドキュメント

このディレクトリは Studai の **企画 → 設計 → 実装 → レビュー** の全工程を記録したものです。
本番コードは [`../frontend/`](../frontend/) / [`../backend/`](../backend/) にあります。

## 進行体制

Studai は **PM / Designer / Engineer / Reviewer の4役割** を AI に分担させて進めています。各ディレクトリは担当役割に対応します。

| ディレクトリ | 役割 | 担当する AI | 内容 |
|------------|------|------------|------|
| [`01-product/`](01-product/) | PM | claude.ai プロジェクト【PM】 | 要件定義・MVPスコープ・タスクボード |
| [`02-design/`](02-design/) | Designer | claude.ai プロジェクト【デザイン】 | 画面一覧・ワイヤーフレーム・デザインシステム・画面仕様 |
| [`03-engineering/`](03-engineering/) | Engineer | Claude Code | 技術選定・DB設計・API設計・ADR・進捗ログ・検証コード |
| [`04-quality/`](04-quality/) | Reviewer | reviewer サブエージェント／claude.ai【レビュー】 | チェックリスト・レビュー記録 |

AI の使い方と役割分担の詳細は [`ai-usage.md`](ai-usage.md) にあります。

## 記録のルール

1. **成果物と記録は、必ずこのリポジトリ内のファイルとして残す。** チャットや Notion だけで終わらせない。Notion はタスクの状態管理、中身はリポジトリに置く
2. **ローカル（作者の PC）に保存し、コミットして GitHub にも上げる。** どちらか一方だけにしない
3. **タスク由来の成果物は `TASK-<ID>-<slug>` フォルダにまとめる。** タスクIDは Notion「Studai タスク一覧」と対応する

| 記録 | 置き場所 | 書く担当 |
|------|----------|----------|
| 実装タスクの進捗ログ | `03-engineering/tasks/TASK-<ID>-<slug>/progress.md`（ひな形：[`templates/progress.md`](templates/progress.md)） | Claude Code |
| 使い捨ての検証コード | `03-engineering/spikes/TASK-<ID>-<slug>/` | Claude Code |
| 設計判断（ADR） | `03-engineering/adr/<連番4桁>-<kebab-case>.md`（[一覧](03-engineering/adr/README.md)に追加） | Claude Code／claude.ai |
| 画面仕様・デザイン | `02-design/TASK-<ID>-<slug>/`、作業ログは [`02-design/WORKLOG.md`](02-design/WORKLOG.md) | claude.ai【デザイン】 |
| 計画・決定事項 | `01-product/` | claude.ai【PM】 |
| レビュー記録（指摘・判断・反映） | `04-quality/reviews/TASK-<ID>-review.md`（[書き方](04-quality/reviews/README.md)） | reviewer／claude.ai【レビュー】 |
| AI に任せたこと・自分で判断したこと | [`ai-usage.md`](ai-usage.md) | 全員 |

- 本番に組み込むと決めた検証コードは、整理して `frontend/` または `backend/` に移す
- 秘密情報（APIキー、シークレット）はどの記録にも書かない

## 成果物の命名規則

- タスク由来の成果物フォルダは **`TASK-<ID>-<slug>`**（例：`TASK-D02-wireframes`）
- ファイル・フォルダ名は kebab-case

## 主要ドキュメント

| ドキュメント | 場所 |
|------------|------|
| 要件定義 | 準備中 |
| タスクボード | 準備中 |
| 画面詳細設計 | 準備中 |
| デザインシステム | 準備中 |
| DB設計 | 準備中 |
| API仕様 | 準備中 |
| 技術選定 | 準備中 |
| 設計判断記録 (ADR) | 準備中 |
| レビュー記録 | 準備中（書き方は [`04-quality/reviews/README.md`](04-quality/reviews/README.md)） |
| AI の使い方 | [`ai-usage.md`](ai-usage.md) |
