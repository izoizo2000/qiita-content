---
title: 【Go】レガシーなGo(1.16未満)環境を壊さずに新しいGo(1.21)を共存させる方法
tags:
  - Go
  - Windows
  - バージョン管理
  - gitbash
private: false
updated_at: '2025-12-02T09:26:27+09:00'
id: 0ff41d596ee947762bac
organization_url_name: null
slide: false
ignorePublish: false
---

Windows環境（Git Bash）で、古いプロジェクトのために **Go 1.11** などをシステムに入れている状態で、新しい **Go 1.21** なども使いたくなった時の対処法です。

`gvm` などのバージョン管理ツールを入れるのは手間だし、公式推奨の `go install golang.org/dl/go1.21.0@latest` コマンドは、ベースのGoが古すぎると（1.16未満だと）動きません。

そこで、**ZIP版を手動配置し、`.bashrc` の関数でパスを切り替える** という、シンプルかつ環境を汚さない方法を試してみました。

## 前提環境

* OS: Windows 10/11
* シェル: Git Bash (MinGW64)
* 既存のGo: 1.11.X (インストーラーでインストール済み。`C:\Go` にある)
* **やりたいこと:** `go1.21` などの別名コマンドではなく、常に **`go` コマンド** を使いつつ、プロジェクトによってバージョンを切り替えたい。

## 起きた問題

通常であれば公式のマルチバージョン機能を使えばよいのですが、親元のバージョンが古すぎると以下のエラーが出てインストールできません。

```console
$ go install golang.org/dl/go1.21.0@latest
can't load package: package golang.org/dl/go1.21.0@latest: cannot use path@version syntax in GOPATH mode
```

これは `go install ...@latest` 構文が Go 1.16 からの機能だからです。「新しいGoを入れるために新しいGoが必要」という状態になってしまいます。

## 解決策：ZIP配置 ＋ PATH切り替え関数

### 1. 新しいバージョンのZIPをダウンロード

[Go公式サイトの Download ページ](https://go.dev/dl/)から、インストーラー（msi）ではなく **ZIPアーカイブ** をダウンロードします。

ファイル名例: `go1.21.0.windows-amd64.zip`

### 2. フォルダに解凍する

既存の `C:\Go` と干渉しない場所に解凍します。今回はCドライブ直下に配置しました。

配置場所: `C:\go1.21.0`

(`C:\go1.21.0\bin\go.exe` が存在する状態)

:::note
go1.21.0フォルダを作成後、ここに.zipを解凍すると`C:\go1.21.0\go\bin\go.exe`という階層になる場合は、goフォルダ以下を移動するか、下記の関数のディレクトリを変更してください。
:::

### 3. .bashrc に切り替え関数を作成

Git Bash の設定ファイル（`~/.bashrc`）に、環境変数 `GOROOT` と `PATH` を書き換える関数を定義します。

テキストエディタで `~/.bashrc` を開くか、以下のコマンドを実行して追記します。

```bash
cat << 'EOF' >> ~/.bashrc

# --- Go Version Switcher ---

# Go 1.21 に切り替える
function use_go21() {
    export GOROOT=/c/go1.21.0
    export PATH=$GOROOT/bin:$PATH
    echo "Switched to Go 1.21.0"
    go version
}

# Go 1.11 (システムデフォルト) に戻す
function use_go11() {
    export GOROOT=/c/Go
    export PATH=$GOROOT/bin:$PATH
    echo "Switched to Go 1.11.5"
    go version
}
# ---------------------------
EOF
```

追記したら設定を反映させます。

```bash
source ~/.bashrc
```

## 使い方

ターミナルを開いた状態で、定義した関数名（`use_go21` 等）を打つだけで、そのセッション中の `go` コマンドの中身が入れ替わります。

### 新しいGoを使いたい時

```shell
$ use_go21
Switched to Go 1.21.0
go version go1.21.0 windows/amd64

$ go run main.go
(1.21で実行される)
```

### 古いGoに戻したい時

```shell
$ use_go11
Switched to Go 1.11.5
go version go1.11.5 windows/amd64

$ go run main.go
(1.11で実行される)
```

## まとめ

* プロジェクト都合などで古いGo環境をアンインストールしたくない場合は **ZIP版** を使うのが便利かなと試してます。
* `alias` で `go21` コマンドを作る方法もありますが、`go` コマンドで統一したい場合は **PATHを上書きする関数** を作るのがよさそうです。
* ターミナルを閉じれば設定はリセットされるので、システム全体への影響もありません。
