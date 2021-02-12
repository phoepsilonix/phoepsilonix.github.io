---
title: "シェルでマッチした部分を取り出したい"
emoji: "🇯🇵"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [shell,awk,match,regex,ipv6]
published: true
marp: true
---
# シェルでマッチした部分を取り出したい

at 2021-01-29
(2021-02-12 last update)

具体例
mawkの場合

```
ip a s wlo1 | mawk '$0 ~ /^ * inet6 .* scope global dynamic /{print $2}' | mawk 'BEGIN{FS="/"} {print $1}'
```
```
alias myipv6='ip a s wlo1 | mawk '\''$0 ~ /inet6 .* scope global dynamic/{print $2}'\'' | mawk '\''BEGIN{FS="/"} {print $1}'\'''
```

---
基本的に、空白を区切り文字として、それ以外の文字を取り出す。

１、 wlo1インターフェースのネットワーク情報を表示
２、標準入力の各行で、正規表現にマッチしている部分を取り出す。
ipv6のグローバルアドレスにマッチする行を取り出して、その２番めのグローバルアドレス部分を標準出力へ。
空白で区切るので、１番目はinet6。３番めはscopeが入っている。
３、区切り文字に’/’を指定して、アドレスとプレフィクスを、さらに分けて、取り出します。

ipv6でプライバシーを優先されている場合、global dynamicではマッチせず、一時的なアドレスしかない場合もあるかもしれません。

---
gawkの場合

```
ip a s wlo1 | awk 'match($0,/inet6 (.*)\/[0-9][0-9] scope global/, a){ print a[1]}'
alias myipv6='ip a s wlo1 | awk '\''match($0,/inet6 (.*)\/[0-9][0-9] scope global/, a){ print a[1]}'\'''
```

----
#### 解説
ipコマンドでアドレスを表示させ、ipv6のglobalなアドレスのみ取り出します。

1. ip address show コマンドでインターフェースwlo1（wifiインターフェース）を表示しています。
上記では省略形を使っています。ip a s \<interface\>

2. gawkを使って、正規表現で、アドレス部分にマッチする部分のみを取り出します。
matchで、標準入力$0、この場合は、ip a s wlo1 の結果を受けとり、マッチ結果をaに代入します。
一致した部分の中でも、括弧でくくられた部分を取り出すことができます。
今回は括弧でくくった部分が一つなので、1番目ということで、 a[1] をprintして、表示しています。
マッチしたn番目の要素を取り出したい、といった時に使えます。

---
#### 補足
正規表現の
/inet6 (.*)\/[0-9][0-9] scope global/ 
は、ipv6でglobalアドレスを示す行を取り出すために指定した正規表現です。

私の環境ではprefixが64ですので、
/inet6 (.*)\/64 scope global/
でも問題ありません。2桁の数字を想定しています。

aliasに指定する場合の書式は、クォーテーションがちょっとわかりにくいかもしれません。
