# [みんなの未来のかたち](https://minnano.miraino-katachi.com) Docker開発環境
## Docker環境構築参考サイト
[https://qiita.com/si-ma/items/805c3323ea7573f61727](https://qiita.com/si-ma/items/805c3323ea7573f61727)

## 環境構築手順
1. 本リポジトリをクローンする
    ```
    > git clone https://github.com/shibamirai/minnano.miraino-katachi.com.git
    ```
1. コンテナを起動する
    ```
    > cd minnano.miraino-katachi.com
    > docker-compose up -d
    ```
1. ソースコードをDocker環境にデプロイする  
    みんなの未来のかたち(user_management)の\src\html\以下のファイルを./nginx/htmlにコピーする  

1. PHPコンテナで必要ライブラリをインストールする
    ```
    > docker-compose exec php /bin/bash
    -- PHPコンテナに入る --
    $ cd /usr/share/nginx/html
    $ composer install
    $ exit
    > 
    ```
1. データベースのインポート  
    dbコンテナで動いているMySQLのkatachiデータベースにユーザ:root、パスワード:rootで接続し、下記に保存されているバックアップファイルをインポートする。  
    - `Katachi Dropbox/miraino katachi/katachih2/1.事業所全体/12.他/みんなの未来のかたち/データバックアップ`
    
1. Docker環境に合わせて設定(./nginx/html/class/config/Config.php)を変更する
    ```
    /** @var string システムのrootのパス */
    public const SYSTEM_ROOT_PATH = '/katachi_sys/';    <-- '/'に変更

    /** @var string データベースホスト名 */
    public const DB_HOST = 'localhost';    <-- 'db'に変更
    ```
    

## FAQ
1. ログインできない  
    DBホストが'localhost'になっているため。
    このDocker環境ではDBコンテナは'db'なのでclass/config/Config.phpのDB_HOSTを'db'に変更する

1. CSVダウンロードでエラーメッセージ `require_once(): Failed opening required '../vendor/autoload.php' `  
    PHPのライブラリPhpSpreadsheetが入っていないため。phpコンテナ内でcomposer.jsonのある場所(/usr/share/nginx/html/)に行き、compser installを実行する。
    ```
    $ docker-compose exec php /bin/bash
    -- phpコンテナ内 --
    > cd /usr/share/nginx/html
    > composer install
    ```

1. ファイルのパーミッションでエラー  
    Dockerホストのユーザ(1000:1000)とDockerコンテナの実行ユーザがあってないため。
    docker-compose.ymlのphpで実行ユーザ（とグループ）を指定する。
    ```
    [docker-compose.yml]
    php:
        build: ./php
        user: "1000:1000"
    ```
