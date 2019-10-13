# SWAGGER API モックアップ・エディター環境 by Docker

## Features

* **OAS3.0のみが対象です。2.0には対応してません**
* `apisprout`と`Docker`を使ってSwaggerのモックアップ環境を構築できます。
* `Swagger Editor`にも対応しました。
* `swagger-editor-live` を使ってリアルタイムに読み込んだファイルを更新します
* `heroku`を使ってそのままモックアップの環境を公開することも可能です。

## How To Use 

### `docker-compose`を使う場合

`docker-compose.yaml.example`で例を記述してます。

```yaml
version: "3"
services:
  editor:
    build: ./dockers/editor # エディターを立ち上げます
    environment:
      - PORT=8100
      - APINAME=api
    ports:
      - "8100:8100"
    volumes:
      - ./api:/api
  api:
    build: ./dockers/mock # モックサーバーを立ち上げます
    environment:
      - PORT=8101
      - APINAME=api
      - WATCH=true  
      - VALIDATE_REQUEST=true  
    ports:
      - "8101:8101"
    volumes:
      - ./api:/api
```

Swaggerの更新を適用するため、`volumes`で`api`ディレクトリを指定してください。
必要な環境変数として、いくつかを指定することができます

#### `PORT`

`apisprout`を実行するポート番号を指定してください。　**必ず指定してください**
`ports`を使ってホストマシンとゲスト側のポートリンクを行う必要があります。

```yaml
ports:
    - "8100:8100" ## ホスト側のポート:ゲスト側のポート（PORTで指定している値）を指定してください
```

#### `APINAME`

実行する `api` フォルダに配置しているファイルの名前を指定してください。
指定がない場合は`api`が指定され、`api/api.yaml`がファイルとして選択されます。

#### `WATCH` (モックサーバー専用)

`apisprout`の `watch` オプションを設定します。
設定する場合は、`WATCH=true`などを指定してください。

#### `VALIDATE_REQUEST` (モックサーバー専用)

`apisprout`の `validate-request` オプションを設定します。
設定する場合は、`VALIDATE_REQUEST=true`などを指定してください。

`docker-compose up` を実行すれば `localhost:8000` でモックアップサーバーが立ち上がります

複数のAPIサーバーを立ち上げたい場合は、`services`に追加すれば立ち上げることができます。

```yaml
services:
  test:
    build: ./dockers/mock # モックサーバーを立ち上げます
    environment:
      - PORT=8100
      - APINAME=test # api/test.yamlを実行
      - WATCH=true 
      - VALIDATE_REQUEST=true 
    ports:
      - "8100:8100"
    volumes:
      - ./api:/api
  api:
    build: ./dockers/mock # モックサーバーを立ち上げます
    environment:
      - PORT=8100
      - APINAME=api # api/api.yamlを実行
      - VALIDATE_REQUEST=true 
    ports:
      - "8101:8101"
    volumes:
      - ./api:/api
```

上記のようにすると、 `localhost:8100` で `api/test.yaml` 、 `localhost:8101`で`api/api.yaml`のモックがそれぞれ立ち上がります

## Herokuへのデプロイについて

Herokuにログインしていない場合はHerokuにログインしてください。

```shell
$ heroku login
```

モックを公開したいプロジェクトがない場合は `heroku create` を実行して作成してください

```shell
$ heroku create
```

`heroku config`を使って`APINAME`を指定しています。
`APINAME`にはモックにしたい`api/`の`yaml`ファイルの名前を指定してください。
指定がない場合は`api.yaml`が指定されます。

```shell
$ heroku config:set APINAME=test -a your-app-name
```

あとは`heroku container`で `push`　および `release` を実行してコンテナーを公開してください

```shell
$ heroku container:push web -a your-app-name
$ heroku container:release web -a your-app-name
```




## 今後のTodo



