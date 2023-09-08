# docker

## コンテナ内部構造（ディレクトリ・ファイル）確認方法
コンテナ内OSへログイン
```shell
docker exec -it ＜起動したコンテナ名＞ bash
```

## コマンド
* run
  * コンテナ実行
    ```shell
    docker run コンテナID
    ```
* stop
  * コンテナ終了
    ```shell
    docker stop コンテナID
    ```
* rm
  * コンテナ破棄、`-f`で強制破棄
    ```shell
    docker rm コンテナID
    docker rm -f コンテナID
    ```
* attach
    * バックグラウンドで稼働しているコンテナに接続、dockerコマンドにattachを付与し、コンテナ名あるいは、コンテナID指定
        ```shell
        # ex
        $ docker attach test0002
        ↓↓↓
        [root@host0002 /]$ cat /etc/redhat-release
        CentOS release 6.6 (Final)
        [root@host0002 /]$
        ```

* exec
    * 動中のコンテナ内で、指定したコマンドを実行
        ```shell
        # ex linuxコマンドls実行
        docker run -dit --init --name testvm2 ubuntu
        docker exec -it testvm2 ls -l /etc
        ```

## オプション
* -i（--interactive）
    * ホストのターミナルからの入力がコンテナの標準入力につなげる役割
    * 標準入力：キー入力、パイプされたデータ、ファイルからのリダイレクトなど
* -t（--tty）
    * コンテナの標準出力をホストの標準出力につなげること、入出力の結果確認などホスト側で実現するなど
    * 疑似端末：
* -it（-i、-t）
    ```shell
    # -iは、Keep STDIN open even if not attached
    # 標準入力を開き続ける。

    # -tは、Allocate a pseudo-TTY
    # 疑似ttyを割りあてる。

    # 標準入力を開き続け、そこを操作出来るようにする。
    # →手元の環境で、docker内入力ができるようにする
    ```

* -e
    * コンテナ内部に環境変数を設定
    ```shell
    docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root mysql
    ```

* -d（--detach）
    * バックグラウンドでコンテナ実行
        * 通常だと、ホストOS側に実行処理結果・エラーなどが表示されてしまう。dコマンドはそれらを非表示にできる
    ```shell
    docker run -d
    ```

* -p
    ```shell
    docker run -p <コンテナへ外部からアクセスするためのポート>:<コンテナ内部ポート>
    # ex
    docker run -p 3000:8080
    ```

* -v
    * ホスト側任意パスをコンテナ内任意パスにマウント
    * コンテナ内部からホスト側ディレクトリを参照できるようにする
    * コンテナ上で作ったファイルがホストの方に残る　など
    ```shell
    docker run -v <Host側のディレクトリ> ：<Container側のディレクトリ>
    ```
    * 参考：https://qiita.com/Mayumi_Pythonista/items/15257212e0822b40a98a

* -w（--workdir）
  * コンテナ内の作業ディレクトリを指定されたパスに設定

* -u
  * コマンドが実行されるユーザーを設定、ユーザー名またはUIDとして指定
  ```shell
  # Dockerコンテナにuser権限で入りたい時など
  docker exec -u $(id -u $USER):$(id -g $USER) -it <コンテナ名> /bin/sh
  # 指定ユーザはLinux上でidコマンドを実行することで確認
  uid=1000(<ユーザ名>) gid=1000(<ユーザ名>) groups=1000(<ユーザ名>) ...
  ```

* --rm
    * 通常、コンテナをstopさせると、状態が「Exited」になりますが、終了させたコンテナを、再び利用せずに、即座に破棄したい場合使用

* --link
    * 同一ホスト上の2つのコンテナを通信させる
    ```shell
    docker run --link <接続先コンテナ名：エイリアス名（短縮コマンド） など>
    # ex
    docker run -it --name Container2 --link Container1:c1 centos /bin/bash
    ```

### 参照
* https://thinkit.co.jp/story/2015/09/08/6383
* https://qiita.com/mmizum/items/bb4a745b8613136c7560
* https://plus-idea.net/docker-web-server-access-denied/

## docker-compose
* Docker Compose使用する際、"compose.yml"という名前のファイルに、各コンテナに対する設定を予め定義する
* Docker（Docker Engine）では一度に一つのコンテナ操作しかできない
* Docker-composeは一度に複数のコンテナ操作可能、コンテナが複数存在する環境を構築可能
* コンテナの作成・起動にはDockerFileが必要（Docker-Compose.ymlに明記）
* docker-compose.ymlの記述詳細
    * 参考
        * https://docs.docker.com/compose/compose-file/03-compose-file/
    * 全体構成
        ```shell
        ## docker-compose.ymlの大枠 ##
        version: "3" #Docker Composeで使用するバージョンを指定
        services: # services以下コンテナ名は何でも可
            コンテナ名1:
            コンテナ名2:
        ...
        networks: #直下にネットワーク名定義
            ネットワーク名1:
            ネットワーク名2:
        ...
        volumes: #直下にボリューム名定義
            ボリューム名1:
            ボリューム名2:
        ...
        ```
    * services
        ```shell
        ### servicesの詳細 ###
        services:
        コンテナ1:
                image: <イメージ名>
                container_name: <コンテナ名>
                networks:
                    - <ネットワーク名>
                volumes:
                    - <ボリューム名>
                ports:
                    - <ポート番号>
                environment:
                    <キー1>: <バリュー1>
                    <キー2>: <バリュー2>
                    ...
                depends_on:
                    - <依存関係にあるサービス>
                restart: <コンテナ停止時の対応>
                command: <実行するコマンド>
        コンテナ2:
        ...
        ```
        * image：指定するイメージ
        * build：指定するDockerfile
            * context：Dockerfileを含むディレクトリへのパス、またはgitリポジトリへのURLを定義（．あるいは明記無しでプロジェクトディレクトリとなる）
            * dockerfile：代替のDockerfileを設定
            * args：ビルド引数、DockerfileのARG値を定義
        * container_name：指定するコンテナ名
        * networks：接続するネットワーク
        * volumes：マウント設定
            * ホスト側相対Path:コンテナ絶対path
        * volumes_from：コンテナ間でマウントする際、マウント先のコンテナを指定
        * ports：マッピングするポート番号
        * environment：設定する環境変数
        * depends_on：依存関係にある別のサービス
        * restart：コンテナが停止した際の再試行設定
            * always：（必ず再起動）
            * no ：（何もしない）
        * command：実行するコマンド
        * env_file：実行時に読み込みたい環境設定ファイル
        * entrypoint：実行時に上書きするENTRYPOINT
        * logging：ログを出力するパス
        * external_links：設定する外部リンク
        * network_mode：ネットワークモード設定