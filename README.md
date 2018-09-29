# Docker開発環境( nginx + php-fpm + mysql + phpmyadmin + composer )

## 手順
・envファイルコピー
```
$ cp env-example .env
```
・コンテナ起動
```
$ docker-compose up -d nginx php-fpm mysql phpmyadmin
```

## composer使用方法
例えば、composerを使いLaravelをインストールする場合、
インストールするディレクトリを.envの
```
COMPOSER_EXEC_PATH=./app
```
で指定する。

プロジェクト名とVersionを指定して、以下のコマンドを実行する。
デファルトでversionは5.5としてます。
```
docker-compose run --rm composer create-project --prefer-dist laravel/laravel {プロジェクト名} "5.5.*"
```

あとは、nginxの設定とlaravelのインストール後の設定を行なってください。

## 開発手引き
### nginx
nginxの設定ファイルを追加する場合は
./nginx/sites以下にファイルを作ってください。
```
cp ./nginx/sites/app.conf.example ./nginx/sites/app.conf
```
で設定ファイルをコピーして作って開発すると楽です。

nginxのlogは./data/logs/nginx以下に吐き出されますので使ってください。

### mysql
mysqlのログイン情報はコンテナ起動時の.envに依存します。
ログイン情報を確認しておいてください。
また、mysqlコンテナ起動時にsqlインポートしたい場合には、
./mysql/docker-entrypoint-initdb.d以下に*.sqlファイルを配置してください。

### phpmyadmin
phpmyadminはhttp://localhost:8080でアクセスできますが、
PMA_PORTを変更することがポート番号を変更することが可能です。

### コンテナ再ビルド
コンテナ内で使用したいライブラリがあるが、ないという場合には、
nginx,php-fpm,mysqlディレクトリ以下にDockerfileがありますので、
そこでインストールコマンドを追記して、コンテナを再ビルドしてください。
```
$ docker-compose build --no-cache {サービス名}
```