# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 現状

レシートOCRを使った個人向け支出管理Androidアプリのリポジトリ。現在は基本設計フェーズで、`docs/` に設計書があるだけでソースコードはまだない。ビルド・テストのコマンドは、Gradleプロジェクトを作成した時点でここに追記する。

- ドキュメントは日本語で書き、既存の用語（支出、タグ、統計など）に合わせる
- コミットメッセージは `docs: <日本語の要約>` 形式（例: `docs: 画面遷移を新規作成`）

## ドキュメント

| ファイル | 内容 |
|------|------|
| `docs/dev_proc.md` | 開発工程と各工程の成果物。未作成の設計書の確認に使う |
| `docs/01_project_overview.md` | 企画書（アプリ名は未定、開発方式はハイブリッド） |
| `docs/02_requirements.md` | 機能要件（支出CRUD、OCR、週・月・年別分析、タグCRUD） |
| `docs/screen_list.md` | 画面一覧（ホーム、支出登録、支出詳細、タグ管理、統計、設定） |
| `docs/screen_transition.md` | 画面遷移図（Mermaid。コードブロックなしで直接記述） |
| `docs/db_table_list.md` | テーブル一覧（`expenses` と `tags` を `expense_tags` で多対多に関連付け） |
| `docs/tech_stack_list.md` | 技術スタック |

## 設計方針（`docs/tech_stack_list.md` より）

- 言語は **Java**（Kotlinではない）、レイアウトはXML
- **Single Activity + Fragment** 構成で、画面遷移は **Navigation Component** を使う。1画面を1Fragmentにする
- **MVVM**: Fragment → ViewModel → Repository → Room DAO
- データは **Room（SQLite）** に保存し、設定は **SharedPreferences** に保存する
- レシート読取の流れ: **CameraX** で撮影 → **OpenCV** で前処理（傾き補正・ノイズ除去） → **ML Kit Text Recognition** で文字認識
- 統計画面の円グラフは **MPAndroidChart** で描く
