## 問題
アルパイエって何者？

```py
import os
from Crypto.Util.number import getPrime, GCD, getRandomRange

def main():
    flag = os.getenv("FLAG", "Alpaca{REDACTED}").encode()

    bits = 1024
    p = getPrime(bits // 2)
    q = getPrime(bits // 2)
    n = p * q
    n2 = n * n
    g = n + 1

    r = getRandomRange(2, n - 1)
    while GCD(r, n) != 1:
        r = getRandomRange(2, n - 1)

    cs = []
    rn = pow(r, n, n2)
    for b in flag:
        c = (pow(g, b, n2) * rn) % n2
        cs.append(c)

    print(f"n = {n}\n")
    print(f"c = {cs}\n")

if __name__ == "__main__":
    main()
```

## 方針
Paillier暗号によってFlagが暗号化されたものがtxtファイルとして与えられている。  
公開鍵の一部であるnと暗号文cの配列が与えられているので、復号してFlagを取り出す。

## 解法概要

### Paillier暗号の概要を調べる
- 加法準同型性暗号の一種
- Wikiに暗号化と復号の方式が紹介されている

というところまでは確認。  
数学久しぶりすぎて半分くらい何言ってるか分からない。

### 今回のコードの暗号化の仕方を確認
AIに助けてもらいながら確認。
2項定理とか合同とか懐かしすぎて。。。

$$
\begin{aligned}
c = g^b \cdot r^n \ (mod\ n^2) \\
2項定理から\\
g^b ≡ 1 + bn \ (mod \ n^2) \\
K = r^n \ (mod\ n^2) として\\
c ≡ (1 + bn) \cdot K \ (mod\ n^2)
\end{aligned}
$$

FlagはAlpaca{}で与えられるので、とりあえずASCIIだろうと仮定してb = 65 ('A')と設定。  

$$
\begin{aligned}
c_0 ≡ (1 + 65n) \cdot K \ (mod\ n^2)\\
K ≡ c_0 \cdot (1 + 65n)^{-1} \ (mod\ n^2)
\end{aligned}
$$

すでにoutput.txtで与えられているので、$K$が求まるので各文字を復号していく

$$
\begin{aligned}
c \cdot K^{-1}  ≡ (1 + bn) \ (mod\ n^2)\\
b = \frac{c \cdot K^{-1} - 1}{n}
\end{aligned}
$$

これで各文字がAlpaca{}になるかチェック。  
今回は問題なかったし、実はフラグがBase64でエンコードされていたからAで求めてはいけないとか、そういったひっかけはなく安心。

## KPT
### Keep
- モチベが落ちる前にAIとかの力を借りて維持した
- かなり時間をかけたが、解くところまでいけた
- CTFでよく使われるライブラリが少しわかった
- Paillier暗号では文字ごとに異なる乱数 $r$ を使う必要があるということを知れた

### Probrem  
- かなりAIの力に頼った
- 数学の知識が使わな過ぎて抜けている
- やり方はわかった状態で、復号のプログラムを生成してもらった

### Try
- 数学の知識を使わないもので、AIを使わず挑戦してみる
- 解読するプログラムを自分で触ってみる
