# Qiita Content - Claude Code 設定

## プロジェクト概要

Qiita CLIを使用した記事管理リポジトリです。`main`ブランチにプッシュすると GitHub Actions が自動で記事を Qiita に投稿/更新します。

## ディレクトリ構成

```
qiita-content/
├── public/              # 記事ファイル（Markdown）
│   ├── *.md            # 公開記事
│   └── .remote/        # リモート同期用
├── .github/workflows/   # GitHub Actions
│   └── publish.yml     # 自動投稿ワークフロー
├── qiita.config.json   # Qiita CLI設定
└── package.json
```

## 記事の作成・編集

### 記事ファイル形式

記事は `public/` ディレクトリに配置し、以下のフロントマター形式を使用:

```markdown
---
title: 記事タイトル
tags:
  - タグ1
  - タグ2
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

本文をここに記述...
```

### フロントマター項目

| 項目 | 説明 |
|------|------|
| `title` | 記事タイトル（必須） |
| `tags` | タグ一覧（最大5つ） |
| `private` | 限定公開にするか（`true`/`false`） |
| `updated_at` | 更新日時（自動設定） |
| `id` | 記事ID（新規は`null`、投稿後に自動設定） |
| `organization_url_name` | Organization URL名 |
| `slide` | スライドモード |
| `ignorePublish` | `true`で投稿対象から除外 |

### 新規記事作成

```bash
npx qiita new 記事ファイル名
```

### ローカルプレビュー

```bash
npx qiita preview
```
ブラウザで http://localhost:8888 を開く

## よく使うコマンド

| コマンド | 説明 |
|----------|------|
| `npx qiita new <name>` | 新規記事作成 |
| `npx qiita preview` | ローカルプレビュー |
| `npx qiita pull` | リモートから記事を取得 |
| `npx qiita publish` | 手動で記事を投稿 |

## 投稿フロー

1. `public/` に記事ファイルを作成・編集
2. `main` ブランチにプッシュ
3. GitHub Actions が自動で Qiita に投稿

## 注意事項

- `QIITA_TOKEN` シークレットがリポジトリに設定されている必要あり
- 画像は Qiita の画像アップロード機能を使用し、URL を貼り付ける
- タグは最大5つまで
- `ignorePublish: true` で投稿対象から除外可能

## 言語設定

**日本語でやり取りを行ってください。**
