# anaconda-memo

# conda コマンド

# 仮想環境一覧確認
```
conda info -e
```
# 仮想環境作成
```
conda create -n 仮想環境名 pythonのバージョン
```
Ex.
```
conda create -n env-py36 python=3.6
```

# 構築済み環境（ymlファイルへ）出力
仮想環境下にて  
```
conda env export > ファイル名.yml
```
# 仮想環境（ymlファイルから）再構築
```
conda env create -n 新環境名 -f ファイル名.yml
```

# 参考
https://qiita.com/ozaki_physics/items/985188feb92570e5b82dhttps://qiita.com/ozaki_physics/items/985188feb92570e5b82d
