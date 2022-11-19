# Django_memo

## my Django memo

## 参考チュートリアル
・Django Girls

https://tutorial.djangogirls.org/ja/

・Django Girls Extensions

https://tutorial-extensions.djangogirls.org/

・Django 公式 

https://docs.djangoproject.com/ja/3.2/intro/tutorial01/

----------------------------------------------------------------------

以下↓、大半がDjango Girlsを準拠したもの

## 導入
・
・
・
## 手順
## ローカル環境操作
### サーバ起動

`manage.py`が存在するディレクトリにて

```
python manage.py runserver
```

### 仮想環境

アクティブ化

`<venv-name>\Scripts\activate`が存在するディレクトリにて

```
<venv-name>\Scripts\activate
```

Ex.
```
myvenv-django-py39\Scripts\activate
```

非アクティブ化
```
deactivate
```

## 設定など

## エラーなど
### OperationalError: no such table:"<APP名>_<class名>"

（dbエラーに関して）

https://qiita.com/minari766/items/0d549e4d86704bfec3c1

解決方法

https://qiita.com/Arata0608/items/dc72286a3532fdbd75cc

https://qiita.com/minari766/items/0d549e4d86704bfec3c1

## デプロイ

```
pip3.6 install --user pythonanywhere
```

```
pip3.9 install --user pythonanywhere
```

## アプリの上書き（再度，デプロイ）

メモ：手動デプロイにおいて，以下にてアプリの上書きができることを確認．

以下
`/home/●●●/●●●.pythonanywhere.com`　にて

↓よりファイルの更新
```
git pull
```
git pullでエラーが起きる場合，以下など参照．

git pull にてローカルをリモートで強制上書きする方法

https://www-creators.com/archives/1097#git_pull

```
// 1) リモートの最新を取ってきておいて
git fetch origin main
// 2) ローカルのmasterを、リモート追跡のmasterに強制的に合わせる
git reset --hard origin/main
```

↓より静的ファイルの更新（html, cssなどに変更を加えた場合）
```
python manage.py collectstatic
```

## 手動デプロイ
以下，pythonanywhereでの手動デプロイ方法

以下参考サイトを準拠

### 参考

・https://tutorial-extensions.djangogirls.org/en/manual_pythonanywhere_deploy

### 0.準備

・pythonanywhereアカウント所持

・そのアカウントにてwebアプリ，それらファイルがある場合は削除する

### 手順

### 1.pythonanywhereにgithubのコードをプルダウン

"Bash"にて，pythonanywhereにgithubに上げたコードをプルダウン(クローン)する．

その際，クローンする"app-name"が，既にディレクトリに存在する場合，エラーとなるので注意

```
git clone https://github.com/<github-username>/<app-name>.git <pythonanywhere-username>.pythonanywhere.com
```

### 2.virtualenv作成

・メモ：virtualenv削除する場合，pythonanywhere → Files から削除する

```
mkvirtualenv --python=python<version> <pythonanywhere-username>.pythonanywhere.com
```
ex．
```
mkvirtualenv --python=python3.6 <pythonanywhere-username>.pythonanywhere.com
```

・仮想環境アクティブ化

```
workon <your-pythonanywhere-username>.pythonanywhere.com
```

・仮想環境非アクティブ化

```
(●●●.pythonanywhere.com) $ deactivate
```

・メモ：pythonanywhere上のvirtualenvのpath 

```
/home/●●●/.virtualenvs/●●●.pythonanywhere.com
```

・virtualenv上にDjango(または，各種ライブラリ)をインストール

```
(●●●.pythonanywhere.com) $ pip install 'django<3'
```
または，
```
(●●●.pythonanywhere.com) $ pip install -r requirements.txt
```

### 3.pythonanywhere上にデータベース作成

仮想環境アクティブ化，かつ，manage.pyが存在するディレクトリにて実行

```
(●●●.pythonanywhere.com) $ python manage.py migrate
```

サーバー上のスーパーユーザー(管理者)を作成
```
(●●●.pythonanywhere.com) $ python manage.py createsuperuser
```

### 4.静的ファイル収集

全ての静的ファイルを収集し，一か所に配置するためのコマンド

```
(●●●.pythonanywhere.com) $ python manage.py collectstatic
```

### 5.webappとして公開
ダッシュボード → Webタブ → 新しいwebアプリの追加

手動構成(Djangoではないやつ)選択 →　virtualenvと同じpythonバージョン選択


### 6.virtualenv設定

virtualenvのpathを入力する．

メモ：
```
/home/●●●/.virtualenvs/●●●.pythonanywhere.com
```

### 7.静的ファイルマッピング追加
URL:
```
/static/
```
Directory:
```
/home/●●●/●●●.pythonanywhere.com/static
```

### 8.WSGIファイル構成

```
/var/www/<pythonAnywhere-username>_pythonanywhere_com_wsgi.py
```
の中身を以下の通りに書き換える
```
import os
import sys

#↓適宜書き換える
path = ('/home/<pythonanywhere-username>/<your-pythonanywhere-username>.pythonanywhere.com')
if path not in sys.path:
    sys.path.append(path)

os.environ['DJANGO_SETTINGS_MODULE'] = '△△△△△.settings' #適宜書き換える

from django.core.wsgi import get_wsgi_application
application = get_wsgi_application()
```

セーブしてリロードすれば完了．

### その他メモ

settings.pyに関して

以下の通りstr型にしないとエラー発生

```
######変更後↓######

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': str(BASE_DIR / 'db.sqlite3'),
    }
}

######変更前↓######
#エラー内容
#TypeError: argument of type 'PosixPath' is not iterable

"""
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
"""
```

