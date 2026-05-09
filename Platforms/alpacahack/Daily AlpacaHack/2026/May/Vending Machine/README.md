## 問題
自販機でフラグを購入してください。

```py
def __init__(self):
    self.stock = 'a'*30 + 'b'*60 + 'c'*20 + 'd'*50 + 'e'*40 + 'f' # 'aaa...eeef'
    self.item_names = {
        'a': 'apple juice',
        'b': 'banana juice',
        'c': 'coke',
        'd': 'draft beer',
        'e': 'energy drink',
        'f': 'flag'
    }
```
stockにflagは存在するがfを選択しても取り出せない。
```py
if mark not in ['a', 'b', 'c', 'd', 'e']: # No 'f'? Hmm...
    print("Invalid choice.")
    return
```

## 解法概要
pythonのpopメソッドの仕様を使う。
どれか一文字を入力し続ければflagが取れる。

## 解法検討

### どうすればflagを取り出せるか
1. ユーザーからの入力受付
2. 入力文字がa~eならfindメソッドが位置を探し当てる
3. findメソッドでヒットしなくなると、-1を返してくる
4. pop(-1)はリストの最後を指定していることになる
5. 最後に位置しているfがpop(-1)でとりだせる

## KPT
### Keep
- 特に苦戦せず解くことができた
- findとpopの扱えるデータ型が違うということを学べた

### Probrem  
- popとかfindの仕様は知らないまま、とりあえず1種類からにしてみるかで取れてしまった。取れてから考えたので、順番が逆

### Try
- 基本Cしか使わないので、pythonのメソッドに対する理解が浅い。CTFをやりながらで勉強してみる。
