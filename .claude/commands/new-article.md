---
description: 新しいQiita記事を作成
allowed-tools: Bash, Write, Read, Edit
---

新しいQiita記事を作成します。

## 手順

1. ユーザーに記事のテーマとタイトルを確認
2. `npx qiita new <ファイル名>` で記事ファイルを生成
3. フロントマターを適切に設定（タイトル、タグなど）
4. 記事の骨子を作成

## 記事テンプレート

```markdown
---
title: $ARGUMENTS
tags:
  - タグ1
private: false
updated_at: ''
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

# はじめに

## 本文

## まとめ
```
