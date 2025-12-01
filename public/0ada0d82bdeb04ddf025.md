---
title: 【Docker】コンテナの基礎を理解する
tags:
  - Docker
private: false
updated_at: '2025-03-27T16:06:03+09:00'
id: 0ada0d82bdeb04ddf025
organization_url_name: null
slide: false
ignorePublish: false
---
# 1. Dockerとは
![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/3562533/1a6f82c1-c20b-4bd9-8893-e876e4786bba.png)

Dockerは、コンテナ技術を利用してアプリケーションとその依存関係を1つのパッケージにまとめるツールです。

* **コンテナの特徴**:
    * 軽量で迅速に起動
    * OSや環境に依存せず動作
* **主な用途**:
    * 開発環境の再現
    * 本番環境へのデプロイ
* **仮想マシンとの違い**: コンテナはOSを共有するため、リソース消費が少ない。

# 2. Dockerの基本用語
以下はDockerを理解する上で重要な用語です。

* **イメージ**: アプリケーションやライブラリをまとめたテンプレート。
* **コンテナ**: イメージを実行可能なインスタンスにしたもの。
* **Dockerfile**: イメージを作成するための設定ファイル。
* **Docker Hub**: 公開イメージを共有するリポジトリ。
  


# 3. 環境準備
Dockerを使用するには、まずインストールが必要です。

* **インストール手順**:
    1. [公式サイト](https://www.docker.com)からDocker Desktop（Windows/Mac）またはDocker Engine（Linux）をダウンロード。
    2. インストール後、以下のコマンドで動作を確認します。
        ```text
        docker --version
        ```

* **動作確認**: 以下のコマンドでサンプルコンテナを起動します。
```text
docker run hello-world
```
※ 「Hello from Docker!」と表示されれば成功。


# 4. 基本操作の実践
Dockerの基本的なコマンドを紹介します。

### イメージの取得
Docker Hubから既存のイメージを取得します。

```text
docker pull ubuntu
```
※ Ubuntuの公式イメージをダウンロード。

### コンテナの起動
取得したイメージからコンテナを起動します。

```text
docker run -it ubuntu
```
* `-i`: インタラクティブモード
* `-t`: ターミナル割り当て ※ コンテナ内でUbuntuのシェルが利用可能。

### コンテナの確認
実行中のコンテナを確認します。

```text
docker ps
```

停止したコンテナも含めて表示するには:
```text
docker ps -a
```

### コンテナの停止と削除
コンテナを停止するには:

```text
docker stop <コンテナID>
```

削除するには:
```text
docker rm <コンテナID>
```
※ `<コンテナID>`は`docker ps -a`で確認。

# 5. 簡単なDockerfileの作成
独自のイメージを作成します。

* **手順**:
    1. 新しいディレクトリを作成し、`Dockerfile`を以下のように記述。
    ```text
    FROM ubuntu:latest
    RUN apt-get update && apt-get install -y curl
    CMD ["echo", "Hello, Docker"]
    ```
    2. イメージをビルド。
    ```text
    docker build -t my-ubuntu .
    ```
    3. コンテナを起動。
    ```text
    docker run my-ubuntu
    ```
    ※ 「Hello, Docker」と表示される。

:::note warn
**注意点**
コンテナは一時的なものと認識し、永続データはボリュームやバインドマウントで管理します。
イメージサイズを小さく保つため、不要なファイルを削除する習慣が推奨されます。
:::

---
### その他資料
* [公式ドキュメント](https://docs.docker.jp/)
* [ゼロから始めるDocker入門](https://qiita.com/suzu12/items/315fba934de11af826bd)
* [Dockerチュートリアルを日本語で説明（第2章）](https://moritomo7315.hatenablog.com/entry/2019/01/18/105849)
* [Dockerを体系的に学べる公式チュートリアル和訳](https://qiita.com/Michinosuke/items/5778e0d9e9c04038903c)
* [実践 Docker - ソフトウェアエンジニアの「Docker よくわからない」を終わりにする本](https://zenn.dev/suzuki_hoge/books/2022-03-docker-practice-8ae36c33424b59)
