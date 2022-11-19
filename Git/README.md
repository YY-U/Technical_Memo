# Git-command-memo

# ◇ローカルPC ⇒ github 転送への手順
## ローカルリポジトリ処理
- 新規リポジトリ作成
```
git init
```
`git init` ⇒ Initialized empty Git repository in /××/○○/△△/.git/

- add コマンド: インデックスにデータ追加
```
git add XXXX
```
`git add -A` ： 新規作成/変更/削除されたファイル全てを追加（AllのA）

`git add -u` ： 変更/削除されたファイルを追加．新規作成は対象外（updateのu）

`git add .`  ： 新規作成/変更されたファイルを追加．削除は対象外

`git add *.md` ： ファイル形式を指定．

- commit（登録） : インデックスの内容を全てローカルリポジトリに登録
```
git commit -m "commit message"
```

- リモートリポジトリ側（github側）のブランチ指定
メインにするブランチ名を指定
```
git branch -M main
```

- リモートリポジトリを追加
リモートリポジトリをローカルリポジトリへ追加，ローカルリポジトリと連携を行う
```
git remote add [追加するリモートリポジトリ名] [追加したいリポジトリ]
```
```
git remote add origin https://github.com/xxxxx.....
```

- push（送信）によりgithubにデータ送信
```
git push -u origin main
```

`rejecte` される場合
```
git push -f origin main
```
で強制(force)的にプッシュする．もとのリモートリポジトリ内容が強制的に上書きされ，無くなるので注意

（参考）

https://qiita.com/Takao_/items/5e563d5ea61d2829e497

## リモートリポジトリ処理

- リポジトリ作成（新規の場合）
- URL取得

# ◇ローカルリポジトリ更新⇒リモートリポジトリへ反映
```
git add XXX
```
```
git commit -m "commit message"
```
```
git push origin main
```

# ◇リモートリポジトリ⇒ローカルリポジトリへコピー
```
git clone https://github.com/YY-U/xxxxxxx.git {ディレクトリ名(省略可能)}
```
(ディレクトリ名省略した場合、リモートのディレクトリとなる)
```
git clone https://github.com/YY-U/xxxxxxx.git .
```
(現在のディレクトリへクローン．同じ名前のディレクトリが存在すると無効)


# ◇リモートリポジトリ⇒既存のローカルリポジトリに上書き
- ローカルmainを，強制的にリモートmainへ合わせる

```
git fetch origin master
```
// 1) リモートの最新を取得
```
git reset --hard origin/master
```
// 2) ローカルmainを，リモート追跡のmainに強制的に合わせる

（参考）

https://www-creators.com/archives/1097#git_pull

git reset について

https://qiita.com/shuntaro_tamura/items/db1aef9cf9d78db50ffe

- pull
リモート→ローカルを上書きするならpullの方が適している

pullについて

https://qiita.com/wann/items/688bc17460a457104d7d
