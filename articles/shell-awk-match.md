---
title: "シェルでマッチした部分を取り出したい"
emoji: "🇯🇵"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [shell,awk,match,regex,ipv6]
published: true
---
# シェルでマッチした部分を取り出したい

at 2021-01-29
具体例
```
ip a s wlo1 | awk 'match($0,/inet6 (.*)\/[0-9][0-9] scope global/, a){ print a[1]}'
```
```
alias myipv6='ip a s wlo1 | awk '\''match($0,/inet6 (.*)\/[0-9][0-9] scope global/
, a){ print a[1]}'\'''
```
ipコマンドで自PCのアドレスを表示させ、ipv6のglobalなアドレスのみ取り出すコマンドが上記になります。
aliasに指定する場合の書式は、クォーテーションがちょっとわかりにくいかもしれません。

#### 解説
1. ip address show コマンドでインターフェースwlo1（wifiインターフェース）を表示しています。
上記では省略形を使っています。ip a s \<interface\>

2. awkを使って、正規表現で、アドレス部分にマッチする部分のみを取り出します。
matchで、標準入力$0、この場合は、ip a s wlo1 の結果を受けとり、マッチ結果をaに代入します。
一致した部分の中でも、括弧でくくられた部分を取り出すことができます。
今回は括弧でくくった部分が一つなので、1番目ということで、 a[1] をprintして、表示しています。
マッチしたn番目の要素を取り出したい、といった時に応用できます。

#### 補足
正規表現の
/inet6 (.*)\/[0-9][0-9] scope global/ 
は、ipv6でglobalアドレスを示す行を取り出すために指定した正規表現です。

私の環境ではprefixが64ですので、
/inet6 (.*)\/64 scope global/
でも問題ありません。2桁の数字を想定しています。

```
ping `myipv6`
```

