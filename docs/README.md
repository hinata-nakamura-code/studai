# Studai 設計ドキュメント

このディレクトリは Studai の **企画 → 設計 → 実装 → レビュー** の全工程を記録したものです。

> **進め方のルール（役割、タスクの流れ、Git、記録の置き場所、公開・非公開、費用）は [`rules.md`](rules.md) が正本です。** このファイルはディレクトリの案内だけを書きます。食い違ったら rules.md が正しいです。

## ディレクトリ

Studai は **PM / デザイン / エンジニア / レビュー** の役割を AI に分担させて進めています。各ディレクトリは担当の役割に対応します（役割の詳細は [`rules.md` の「1. 役割」](rules.md#1-役割)）。

| ディレクトリ | 役割 | 内容 |
|------------|------|------|
| [`01-product/`](01-product/) | PM | 引き継ぎ事項・要件定義・計画 |
| `02-design/` | デザイン | 画面一覧・画面仕様（まだない） |
| [`03-engineering/`](03-engineering/) | エンジニア | ADR・タスクの作業記録（progress.md）・検証コード |
| [`04-quality/`](04-quality/) | レビュー | レビュー記録 |
| [`learning/`](learning/) | 作者・PM | 技術を理解したまとめ（学習メモ） |
| [`templates/`](templates/) | — | ひな形（[progress.md](templates/progress.md)） |

記録をどこに置くか・誰が書くかは [`rules.md` の「7. 記録（証跡）の置き場所」](rules.md#7-記録証跡の置き場所) を見てください。

## 主要ドキュメント

| ドキュメント | 場所 |
|------------|------|
| 運用ルール | [`rules.md`](rules.md) |
| 引き継ぎ事項 | [`01-product/carry-over.md`](01-product/carry-over.md) |
| 要件定義 | 準備中 |
| 計画 | [`01-product/plan.md`](01-product/plan.md) |
| 画面一覧・画面仕様 | 準備中 |
| DB設計 | 準備中 |
| API仕様 | 準備中 |
| 設計判断記録 (ADR) | 準備中 |
| レビュー記録 | [`04-quality/reviews/`](04-quality/reviews/)（書き方は [README](04-quality/reviews/README.md)） |
| 学習メモ | [`learning/`](learning/) |
| AI の使い方 | [`ai-usage.md`](ai-usage.md) |
