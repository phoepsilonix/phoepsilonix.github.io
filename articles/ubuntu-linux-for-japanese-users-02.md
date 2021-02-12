---
title: "Ubuntu for Japanese Users #2"
emoji: "🇯🇵"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [Linux,Japan,newbie,ubuntu,gnome]
lang: ja-JP
published: true
marp: true
---
# Ubuntu for Japanese Users #2
at 2021-02-12

# ブラウザとメーラー
[Mozilla](https://www.mozilla.org/ja/)のFirefoxとThunderbirdが標準で入っています。

ブラウザは、[Google Chrome](https://www.google.com/intl/ja_jp/chrome/)、[Microsoft Edge](https://news.mynavi.jp/article/20201124-1512076/)、[Brave Browser](https://brave.com/ja/)、[Vivaldi](https://vivaldi.com/ja/download/)など、問題なく動作します。お好みのものを入れましょう。
今は、ブラウザにブックマークやパスワードの同期機能がついているものが増えています。またアドオンなどでもそういった機能が揃いますね。
アカウントに紐つけておいて、同じ環境をPCやスマホで同期できるのは、大変便利です。

---
# クリップボード

いつもお世話になってるxclipをインストール
```
sudo apt install xclip
```

読み込み先を明示的に指定するalias

```
alias myclip='xclip -selection clipboard'
```
端末で、

```
myclip file
```
などとして、クリップボードに読み込めます。

---
# カーネルを新しいものにしたい場合
Ubuntu 20.10では5.8系がインストールされていますが、もし、より新しいカーネルを試したい場合、5.10系までなら、mainlineというGUIのあるアプリもあります。
ただ、めったに使うことはありませんので、手動でも良いかと思います。

また正式リリース前の5.11系を試したい場合には、手動でパッケージをダウンロードするか、時間はかかりますが、自分でビルドすることになるかと思います。

---
[カーネル更新用スクリプト](https://github.com/pimlie/ubuntu-mainline-kernel.sh)

[mainline(GUI)](https://ubuntuhandbook.org/index.php/2020/08/mainline-install-latest-kernel-ubuntu-linux-mint/)

[カーネル更新、コマンドライン](https://sypalo.com/how-to-upgrade-ubuntu)

[カーネルの更新の概略](https://itsfoss.com/upgrade-linux-kernel-ubuntu/)

[カーネル更新ツールの紹介](https://www.linuxuprising.com/2018/10/2-utilities-to-install-latest-kernel-in.html)

mainlineアプリはGUIがありますが、古いバージョンも含めて、カーネルの一覧を取得するので、そこに時間がかかります。
