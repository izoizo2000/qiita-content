---
title: Vercelでポートフォリオサイトを公開する方法
tags:
  - デプロイ
  - ポートフォリオ
  - Next.js
  - Vercel
private: false
updated_at: '2026-01-22T13:08:33+09:00'
id: 421925a56ddce7a7e85a
organization_url_name: null
slide: false
ignorePublish: false
---

# はじめに

Next.jsで作成したポートフォリオサイトをVercelで公開する手順をまとめました。
Vercelは無料プランでも十分な機能があり、GitHubと連携することで自動デプロイも可能です。

**公開したサイト**: https://portfolio-site-livid-one-87.vercel.app/

## なぜVercelなのか

ポートフォリオサイトのホスティング先として、Vercelを選んだ理由は以下の通りです。

| 項目 | Vercel | GitHub Pages | Netlify |
|------|--------|--------------|---------|
| Next.js対応 | 最適（開発元） | 静的のみ | 対応 |
| 無料枠 | 十分 | 十分 | 十分 |
| 自動デプロイ | GitHub連携で自動 | GitHub連携で自動 | GitHub連携で自動 |
| SSR/API Routes | 対応 | 非対応 | 対応 |
| 設定の簡単さ | 非常に簡単 | 簡単 | 簡単 |

**Vercelを選んだ主な理由**:
- **Next.jsとの相性**: VercelはNext.jsの開発元であり、最適化されたデプロイ環境を提供
- **ゼロコンフィグ**: Next.jsプロジェクトなら設定なしでそのままデプロイ可能
- **プレビュー機能**: プルリクエストごとにプレビューURLが自動生成される
- **エッジネットワーク**: 世界中のCDNで高速配信

## 前提条件

- GitHubアカウントを持っている
- ポートフォリオサイトのソースコードがGitHubリポジトリにプッシュされている
- Next.jsプロジェクトが正常にビルドできる状態

# 1. Vercelアカウントの作成

## 1-1. Vercelにアクセス

[Vercel公式サイト](https://vercel.com)にアクセスし、右上の「Sign Up」をクリックします。

## 1-2. GitHubアカウントで登録

「Continue with GitHub」を選択してGitHubアカウントで登録します。
GitHubと連携することで、リポジトリのインポートがスムーズになります。

## 1-3. アカウント設定

- プラン: Hobbyプラン（無料）を選択
- 必要な権限を許可してアカウント作成を完了

# 2. GitHubリポジトリとの連携

## 2-1. 新規プロジェクトの作成

Vercelのダッシュボードで「Add New...」→「Project」をクリックします。

## 2-2. リポジトリのインポート

1. 「Import Git Repository」セクションで、公開したいリポジトリを選択
2. GitHubの権限設定を求められた場合は「Configure GitHub App」をクリック
3. 対象のリポジトリへのアクセスを許可

## 2-3. プロジェクト設定

- **Project Name**: 任意のプロジェクト名を入力
- **Framework Preset**: Next.jsが自動検出されます
- **Root Directory**: プロジェクトのルートディレクトリ（通常は`./ `）
- **Build and Output Settings**: デフォルトのままでOK

## 2-4. デプロイ

「Deploy」ボタンをクリックしてデプロイを開始します。
ビルドが完了すると、自動的にURLが発行されます。

# 3. デプロイ完了後の確認

## 3-1. 公開URLの確認

デプロイが成功すると、`https://プロジェクト名.vercel.app` の形式でURLが発行されます。

## 3-2. 自動デプロイの確認

GitHubリポジトリにプッシュすると、自動的に再デプロイされます。
- `main`ブランチへのプッシュ → 本番環境に反映
- プルリクエスト → プレビュー環境が自動作成

# まとめ

Vercelを使えば、Next.jsで作成したポートフォリオサイトを簡単に公開できます。

**ポイント**
- GitHubアカウントがあれば数クリックでデプロイ可能
- Hobbyプラン（無料）で十分な機能
- GitHubとの連携で自動デプロイが実現
- プルリクエストごとにプレビュー環境が作成される

ぜひ皆さんもポートフォリオサイトを公開してみてください。

# 参考リンク

- [Vercel公式サイト](https://vercel.com)
- [Next.js公式ドキュメント - Deploying](https://nextjs.org/docs/deployment)
