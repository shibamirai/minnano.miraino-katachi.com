# [みんなの未来のかたち](https://minnano.miraino-katachi.com) Docker開発環境
## Docker環境構築参考サイト
[https://qiita.com/si-ma/items/805c3323ea7573f61727](https://qiita.com/si-ma/items/805c3323ea7573f61727)

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
