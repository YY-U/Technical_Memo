# java 設定など
## コンパイル方法
文字化けを出ないようにする方法は以下
```
C:~src > javac -encoding UTF-8 main.java
```
## 実行方法
- 標準入力  
コマンドライン引数からファイル読み込みする方法
```
C:~src > java main < input1.txt
15
```