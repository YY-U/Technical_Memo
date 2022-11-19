# memo
## Chrome-Remote-Desktop
## 構築環境
windows10ノートPC(接続元) → ubuntu18.04デスクトップ(接続先)

## 修正手順

### 0．導入など
#### 0-1．インストール

chromeのウェブストア → "+CHROMEに追加"からインストール
→ "アプリ起動" → "利用を開始" → "リモート接続を有効にする"にてインストーラーDL 
→以下コマンドにてインストール完了
```
sudo dpkg -i chrome-remote-desktop_current_amd64.deb
```
```
sudo apt-get install -f
```

#### 0-2．グループ追加
カレントユーザをchrome-remote-desktopグループに追加し再起動
```
sudo usermod -a -G chrome-remote-desktop $USER
```
```
sudo reboot
```

### 1．前処理
#### 1-1．chromeRemoteDesktopサービスを止める

```
sudo systemctl stop chrome-remote-desktop.service
```

or

```
cd /opt/google/chrome-remote-desktop
```
```
./chrome-remote-desktop --stop
```

#### 1-2．オリジナルファイルのバックアップを作成
```
sudo cp /opt/google/chrome-remote-desktop/chrome-remote-desktop /opt/google/chrome-remote-desktop/chrome-remote-desktop.orig
```

#### 1-3．ファイルを開く
```
sudo gedit /opt/google/chrome-remote-desktop/chrome-remote-desktop
```

### 2．ファイル編集
#### 2-1．
```
echo $DISPLAY
```
↑で出力された"数字"と同じ"数字"になるよう↓を書き換える

```
FIRST_X_DISPLAY_NUMBER = "数字"
```
#### 2-2．
以下の通りに編集
```python
  @staticmethod
  def get_unused_display_number():
    """Return a candidate display number for which there is currently no
    X Server lock file"""
    display = FIRST_X_DISPLAY_NUMBER
    # while os.path.exists(X_LOCK_FILE_TEMPLATE % display):
    #   display += 1
    return display
```
#### 2-3．

```python
  def launch_session(self, x_args):
    self._init_child_env()
    self._setup_pulseaudio()
    self._setup_gnubby()
    # self._launch_x_server(x_args)
    # self._launch_x_session()
    display = self.get_unused_display_number()
    self.child_env["DISPLAY"] = ":%d" % display
```

すべて完了後，保存して終了．

### 3．再起動

```
sudo systemctl restart chrome-remote-desktop.service
```

or

```
./chrome-remote-desktop --start
```


### 4．その他
#### 4-1．上手くいかない場合
アンインストール後最初からやり直し
```
sudo systemctl stop chrome-remote-desktop.service
sudo apt-get purge chrome-remote-desktop
```
↑の後，Chrome開き，`chrome://apps/`入力．
ChromeRemoteDesktopアイコン右クリックし削除する．

#### 4-2．解像度
接続時，解像度が下がる

設定 → デバイス → ディスプレイ

にて適宜変更


### 参考サイト
・https://xvideos.hatenablog.com/entry/chrome-remote-desktop-ubuntu1804

・https://qiita.com/s5uishida/items/60eb626df030e33642f4

・https://webnetforce.net/ubuntu-chrome-remote-trouble/




