---
title: "bootable-usb-solution-ventoy"
emoji: "🇯🇵"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [bootable,iso,usb,grub]
lang: ja-JP
published: true
marp: true
---
# A New Bootable USB Solution
##### ブータブルなisoイメージをコピーするだけで、簡単にUSBメディアから起動できます。

[Ventoy](https://www.ventoy.net/en/index.html)のおかげで、レスキューもOSの試用も、安心して試せます。
こんな安心できるもの、どうやって作ってるの！？
万一の備えに、あなたもお一ついかがですか？

USBディスクに、セキュアブート対応でGPTパーティションのventoyをインストールしておきました。
```
sudo Ventoy2Disk.sh -i -s -g /dev/sdX
```
USBディスクの中味は消えますよ！

---
ventoyが対応しているファイルシステムならisoイメージをコピーする場所は、再フォーマットできます。（特にこだわりがなければ、デフォルトでいいと思います。）

isoファイルをコピーするだけで、いろんなブート可能なイメージファイルを試せるなんて、素敵ですね。
公式サイトみたら、これだけ[テスト済み](https://www.ventoy.net/en/isolist.html)なんだというので驚きますし、安心できます。

---
実際、使ってみたら、便利なことこのうえない。
USBブートさえできてくれれば、レスキューできる。
これがあれば、安心安心。

誰か作ってくれないかと思っていたら、既に完成といえるものが、できていたんだもの。
ありがとう。ありがとう。

ほんとに、足を向けて寝れませんよ。ありがたやー。
