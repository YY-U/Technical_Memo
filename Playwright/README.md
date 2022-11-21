# playwright setup
# playwright(windows and anaconda 環境下)
python > 3.7
```
pip install playwright
python -m playwright install
pip install pytest-playwright
pip install asyncio
```
# python-dotenv
```
pip install python-dotenv
```

## 参考  
https://tech.walkit.net/web-test-playwright-python

# 実行
```
python src/main.py
```
or
```
pytest src/main.py
```
# コード自動生成
コード自動生成
```
playwright codegen wikipedia.org -o test.py
```
デバイス指定
```
playwright codegen wikipedia.org -o test.py --device "iPhone 11"
```

# その他備考
## .env
- 同一ディレクトリである必要あり
- python-dotenvは`.env`を指定する．`.env`を適宜修正する．

## コード等
- `HEADLESS=false`でテスト実行画面を表示

## pytest
- `pytest`はPythonスクリプトをテストするための別フレームワーク
- `def test_*` `def *_test`、`test_*.py` 等で`pytest`実行される
- https://note.com/npaka/n/n84de488ba011