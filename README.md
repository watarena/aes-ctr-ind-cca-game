# openssl を使って AES の CTR モードで IND-CCA ゲームをする

## 鍵生成

``` bash
openssl rand 32 -out private.key -base64
```

## 平文のどちらかを暗号化する

``` bash
echo $((${RANDOM} % 2)).txt > answer.txt
openssl enc -e -aes-256-ctr -kfile private.key -in $(cat answer.txt) -out cipher.txt
```

## 復号オラクルにクエリ

``` bash
cp cipher.txt query.txt
# query.txt をバイナリエディタで開き, 最後の bit を変える
openssl enc -d -aes-256-ctr -kfile private.key -in query.txt
```

出力が `0...01` なら `0.txt`, `1...10` なら `1.txt` が答えになる.

## 確認

``` bash
cat answer.txt
```
