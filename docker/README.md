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

## オプション
* -d(--detach)
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

* --rm
    * 通常、コンテナをstopさせると、状態が「Exited」になりますが、終了させたコンテナを、再び利用せずに、即座に破棄したい場合使用

* --link
    * 同一ホスト上の2つのコンテナを通信させる
    ```shell
    docker run --link <接続先コンテナ名：エイリアス名（短縮コマンド） など>
    ```


docker run -it --name Container2 --link Container1:c1 centos /bin/bash

## 参照
* https://thinkit.co.jp/story/2015/09/08/6383
* https://qiita.com/mmizum/items/bb4a745b8613136c7560
* https://plus-idea.net/docker-web-server-access-denied/
