## Map data

### 問題文
中古のカーナビを解析していたら、マップデータがたくさんはいったファイルを見つけました。  
しかしファイルが多すぎて、探すのが大変です。  
どれが本物のフラグが書かれた画像か、特定できますか？

### 解法
ファイルを落とすと11GBくらいになりました。  
フォルダにフォルダが入っていて、pngがたくさんあります。  
ファイルサイズが違うのがないのかチェックしてもサイズは同じ。  
Flagの加工がされているので、ハッシュ値の仲間外れを探してみる。  
dist-map_data\maps\85\37\76.pngのハッシュ値だけ違ったので、画像を開いてFlag Get。

## Weird config updater - unsolved
### 問題文
あるデバイスで、zipでコンフィグをアップロードするサービスが動いているのを見つけました。  
最初にシリアルナンバーを入力すると、サービスにアクセスできるようです。  
検証を回避して、任意のコンフィグをアップロードできると、嬉しいのですが…  

Connection / 接続  
nc ********  
serial number: ae489873-895046f0-dac760e5

### 検討
適当にcat flag.txtを入力したところ  
[ERR] unknown command; see HELP  
と出力されました  
  
```
help
[OK]  
UPLOAD <size>  
  read 2*<size> ASCII hex characters, verify uploaded ZIP, then extract it  
HELP  
  show this help text  
FLAG  
  show flag if mode == debug  
QUIT  
  exit the service  

flag  
[ERR] mode != debug
```
とりあえずflagって打ってみましたが、debugに書き換えないと出せないところまでは判明。  
サイズ指定してUPLOADしてみます。

```
UPLOAD 12
ae489873895046f0dac760e5
[ERR] archive is not a valid zip file
```

分からな過ぎたのでAIを使いました。  
config.json（release版）と、config.json（debug版）のデータを用意し、複数のファイルのzipを検知していたので、情報を削除して偽装。  
検証ツールを突破し、かつその目次が指すconfig.json（release版）のハッシュを確認して合格を出します。
しかし、展開ツール（unzip等）は、ファイル内に転がっている2つの Local File Header を両方見つけ出して展開するため、最終的に debug版が上書きされるはず。。。といったところで詰まりました。  

```
UPLOAD 195

504b0304140000000000988ab15c8ac1ff3412000000120000000b000000636f6e6669672e6a736f6e7b226d6f6465223a2272656c65617365227d504b0304140000000000988ab15cf8299f4610000000100000000b000000636f6e6669672e6a736f6e7b226d6f6465223a226465627567227d504b01021400140000000000988ab15c8ac1ff3412000000120000000b0000000000000000000000800100000000636f6e6669672e6a736f6e504b0506000000000100010039000000740000000000

[INFO] zip saved as /tmp/cfgupd-stv8w00t.zip

[OK] sha256=fe16413242b7ff00386d8852552bcdbb5ebfeb90fef35892693278703156b454
```