# ShellScript
#
## 構文

```shell
# ↓宣言文
#!/bin/bash
 
echo "Hello World"
```
* 特殊変数
    * `$?`：直前のコマンドの実行結果（0／1）
    * `$#`：シェルスクリプト実行時の引数の数
    * `$0`：シェルスクリプトのファイル名
    * `$1`,`$2`,`$3` ...：シェルスクリプトのn番目の引数の値
    * `$*`または`$@`：シェルスクリプト実行時のすべての引数の値
    * `$$`：プロセス番号

* 参考：
    * https://x-tech.pasona.co.jp/media/detail.html?p=7481
    * https://www.wakuwakubank.com/posts/347-linux-shell/#index_id1