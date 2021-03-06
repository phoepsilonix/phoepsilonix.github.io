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

# 日本語入力の設定

fcitxのインストール
```
sudo apt install fcitx-mozc --install-recommends fcitx-tools fcitx-m17n
```

設定画面で、地域と言語を選び＋ボタンを押して、日本語(mozc)を追加します。
その次に、インストールされている言語の管理を押して、キーボード入力に使うIMシステムをfcitxにします。

fcitxの起動
```
fcitx -r
```
fcitxが起動されたら、設定の入力ソースの確認を行います。
mozcが加わっていることを確認できたら、
入力ソースの設定を行います。

```
im-config
```
で、はいを選びながら、選択画面が出てきます。
fcitxを選んで、はい、で終了させます。

最後に、fcitxの自動起動を設定します。
自動起動するアプリケーションが、アクティビティのすべてのアプリの中にあるので、立ち上げます。その中に、im-launchという項目があるので、編集します。
im-launch trueとなっている部分をim-launch fcitxと書き換えましょう。

最後に、いろいろなアプリケーションで日本語入力ができるように、dot file２つも編集しておきます。

.xprofile
```.xprofile
export LANG="ja_JP.UTF-8"
export XMODIFIERS="@im=fcitx"
export XMODIFIER="@im=fcitx"
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export DefaultIMModule=fcitx
```

.pam_environment
```.pam_environment
GTK_IM_MODULE DEFAULT=fcitx
QT_IM_MODULE  DEFAULT=fcitx
XMODIFIERS    DEFAULT=\@im=fcitx
```

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
