---
title: 【Docker】コンテナ管理と複数コンテナの連携
tags:
  - Docker
private: false
updated_at: '2025-03-28T17:43:53+09:00'
id: be4c5daab8c70f9718ef
organization_url_name: null
slide: false
ignorePublish: false
---
# 1. Docker Composeの概要
Docker Composeは、複数のコンテナを定義・管理するためのツールです。

* **特徴**:
    * YAMLファイルで設定を記述
    * コンテナの依存関係を管理
* **主な用途**: 開発環境の構築、テスト環境の再現。

# 2. Docker Composeの基本操作
簡単なWebアプリケーションを例に手順を示します。

* **準備**: ディレクトリを作成し、`docker-compose.yml`を以下のように記述します。
```yaml
version: "3"
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: example
```
* **起動**: 以下のコマンドでコンテナを一括起動します。
```bash
docker-compose up
```
* **確認**: http://localhost:8080にアクセスし、Nginxのデフォルトページが表示されることを確認します。
* **停止**: コンテナを停止し、削除します。
```bash
docker-compose down
```

# 3. ボリュームによるデータ永続化
コンテナは一時的であるため、データを永続化するにはボリュームが必要です。

* **ボリュームの作成**:
```bash
docker volume create my-data
```
* **コンテナでの使用**:
```bash
docker run -v my-data:/data ubuntu
```
　※ `/data`に保存されたデータが`my-data`ボリュームに保持。

* **Docker Composeでの設定**: `docker-compose.yml`に追記します。
```yaml
version: "3"
services:
  db:
    image: mysql:latest
    volumes:
      - db-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
volumes:
  db-data:
```

# 4. ネットワーク設定
コンテナ間の通信を管理します。

* **デフォルトネットワーク**: `docker-compose up`で自動作成されるブリッジネットワークを使用。
* **カスタムネットワークの作成**:
```bash
docker network create my-network
```
* **コンテナの接続**:
```bash
docker run --network my-network -d nginx
docker run --network my-network -it ubuntu bash
```
* **Composeでの設定例**:
```yaml
version: "3"
services:
  web:
    image: nginx:latest
    networks:
      - app-net
  db:
    image: mysql:latest
    networks:
      - app-net
networks:
  app-net:
    driver: bridge
```

# 5. 実践例: WebアプリとDBの連携
以下は、Goアプリケーション（Echo使用）とMySQLを連携させる例です。

* **ディレクトリ構成**:
```text
my-app/
├── main.go
├── Dockerfile
└── docker-compose.yml
```
* **Dockerfile**:
```text
FROM golang:1.20
WORKDIR /app
COPY . .
RUN go mod init my-app && go get github.com/labstack/echo/v4 && go build -o app
CMD ["./app"]
```
* **main.go**:
```go
package main

import (
    "database/sql"
    "net/http"
    _ "github.com/go-sql-driver/mysql"
    "github.com/labstack/echo/v4"
)

func main() {
    e := echo.New()
    e.GET("/health", func(c echo.Context) error {
        db, err := sql.Open("mysql", "root:example@tcp(db:3306)/mysql")
        if err != nil {
            return err
        }
        defer db.Close()
        return c.String(http.StatusOK, "DB Connected")
    })
    e.Logger.Fatal(e.Start(":8080"))
}
```
* **docker-compose.yml**:
```yaml
version: "3"
services:
  app:
    build: .
    ports:
      - "8080:8080"
    depends_on:
      - db
    networks:
      - app-net
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: example
    volumes:
      - db-data:/var/lib/mysql
    networks:
      - app-net
volumes:
  db-data:
networks:
  app-net:
    driver: bridge
```
* **実行**: `docker-compose up --build`で起動し、http://localhost:8080/health を確認。

:::note warn
**注意点**
ボリュームやネットワークの削除を忘れないよう、`docker volume rm`や`docker network rm`を使用。
Composeファイルのバージョンに依存する機能を確認し、適切なバージョンを選択。
:::

### その他資料
##### Docker Compose
* [【入門】Docker Composeとは？インストールと使い方](https://www.kagoya.jp/howto/cloud/container/dockercompose/)
* [docker-composeでvolumesを設定する](https://zenn.dev/ajapa/articles/1369a3c0e8085d)
* [Docker Composeについてざっくり理解する【概要 / ymlファイル書き方 / コマンド操作】](https://qiita.com/gon0821/items/77369def082745d19c38)

##### WebアプリとDBの連携
* [【Docker】Rails6/MySQLのコンテナを作って開発環境を構築](https://qiita.com/hiizkk/items/d5e97119fbe69607a5b8)

##### ボリュームによるデータ永続化
* [【Docker】データの永続化方法 - ボリュームとバインドマウントの選び方 -](https://zenn.dev/fire_arlo/articles/docker-volume-vs-bind-mount)

##### ネットワーク設定
* [docker のネットワーク (docker0) 設定変更](https://qiita.com/msi/items/d9cc1a2fd3f0fed3a901)
