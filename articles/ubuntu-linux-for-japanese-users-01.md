---
title: "Ubuntu for Japanese Users"
emoji: "🇯🇵"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [Linux,Japan,newbie,ubuntu,gnome]
lang: ja-JP
published: true
marp: true
---

# Ubuntu for Japanese Users #1

at 2021-02-12

# はじめの第一歩

[Ubuntu 20.10](https://jp.ubuntu.com/)をインストールしました。
インストーラーは日本語対応しています。[kernel](https://kernel.org)は5.8系。
オフィスアプリは、libreOfficeが最初からインストールされています。

Ubuntuのインストーラーのできは、素晴らしいと思います。

---
# パッケージの更新
インストール直後に一回、更新しておきます。

```
sudo apt update
sudo apt upgrade
```

# 日本語入力のインストール
日本語入力はfcitx-mozcをインストールすればOKです。

```
sudo apt install fcitx-mozc --install-recommends
```

---
# 日本語入力の自動起動
GNOMEの自動起動の設定で、im-launchという設定がありましたが、wayland前提になっています。
GNOMEでログインする際に、右下に出てくるボタンでセッションを選べます。waylandでログインするか、自動起動のコマンドを編集しましょう。
また、なぜかim-launch trueとなってましたが、im-launch fcitxと変更しました。

ibusなどの削除は不要です。そのままでも動きます。

---
# swapfileの修正
起動時にswapfileが読み込めずエラーとなっていました。
swapfileを作り直しましょう。以下の例はサイズを1Gで設定しています。 

```
sudo truncate -s 0 /swapfile
sudo chattr +C /swapfile
sudo btrfs property set /swapfile compression none
sudo fallocate -l 1G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

---
# 起動時のエラー修正
initramfs unpacking failed: Decoding failed というエラーが出ていました。
起動時間短縮のため、カーネルは圧縮されているのですが、形式がlz4だとバグで起動時にエラーが出るようです。起動自体はするので、問題ないのかもしれません。
念の為、昔ながらのgzipまたはlzopに変更しておきます。

次のファイルのCOMPRESS行を変更します。
/etc/initramfs-tools/initramfs.conf 
```
COMPRESS=lzop
    または
COMPRESS=gzip
```

忘れないうちに、カーネルのローダーを再生成します。
```
sudo update-initramfs -u
```
