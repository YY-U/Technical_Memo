# linux-commamd

## ポート番号一覧確認
* `ss`コマンド
```shell
ss -atnu
```
通信確立しているポートのみ表示
```shell
ss -t
```

* 参考：
    * https://www.linuxmaster.jp/linux_skill/2009/02/linux-4.html

## 圧縮
```
zip [オプション] アーカイブ ファイル名
```
ex. ディレクトリの場合
```
zip -r xxxx.zip folder01
```
ex. ファイルの場合
```
zip xxxx.zip file01
```
-eでパスつけられる

## 解凍
```
unzip [オプション] [ファイル名]
```
ex.
```
unzip xxxx.zip
```
ex. 展開先を指定する場合
```
unzip -d home/YYYY/ xxxx.zip
```

## du コマンド
容量はkByte単位で表示
ディレクトリ内各容量表示
```
du -a ディレクトリ名
```
読みやすい単位をつけて表示
```
du -ah ディレクトリ名
```
ディレクトリ内合計容量表示
```
du -s ディレクトリ名
```
読みやすい単位をつけて表示
```
du -sh ディレクトリ名
```
## ファイル，ディレクトリ削除
```
rm [オプション] ファイル1 ファイル2 ファイル3……
```
```
rm -r [オプション] ディレクトリ1 ディレクトリ2 ディレクトリ3……
```
