---
title: "Manjaro Linux for Japanese Users: はじめの第一歩"
emoji: "🇯🇵"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [Linux,Japan,newbie,Manjaro,arch]
published: true
---
# First step for Manjaro Linux for Japanese Users

at 2021-01-27
##### はじめの第一歩。これだけすれば、使えます。

Manjaro Linux Budgieをインストールしました。
コミュニティエディションのBudgieのインストーラーには日本語フォントが入っていませんでした。その場合、ネットに接続していれば、フォントのインストールで対応可能です。インストーラーを起動する前に、下記の１〜３を行うと日本語表示がされると思います。他のエディションでも、基本的に同じです。

### インストール後に最初にすること。
#### 1. パッケージの更新元ミラーサーバーを日本にする。
Make update servers geographically close to each other.
```
sudo pacman-mirrors --geoip
または
sudo pacman-mirrors -c Japan #country
```

#### 2. パッケージデータベースの更新
Update server database.
```
sudo pacman -Sy
あるいは
sudo pacman -Syy
```

#### 3. 日本語フォントのインストール
Installing Japanese Fonts
```
sudo pacman -S ipa-fonts noto-fonts-cjk
```

#### 4. 日本語入力をインストール
Install Japanese input fcitx-mozc
.xprofileという設定ファイルにGUIで使う日本語入力のInputMethodを指定します。
昔の名残で、少し冗長ではありますが、いろんな環境で動くようにという配慮です。
```
sudo pacman -S fcitx-mozc fcitx-configtool fcitx-table-other fcitx-table-extra
cat - << EOF >> ~/.xprofile
export LANG="ja_JP.UTF-8"
export XMODIFIERS="@im=fcitx"
export XMODIFIER="@im=fcitx"
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export DefaultIMModule=fcitx
EOF
```

#### 5. 絵文字フォントのインストール
I also love emoji.
```
sudo pacman -S noto-fonts-emoji unicode-emoji nodejs-emojione
```

#### 6. タッチパネルでコンテキストメニューの開き方を、右側のシングルタップにする。
Adjusts how the context menu is opened on the touch screen.
[Right click wont work on 18.04](https://discourse.ubuntubudgie.org/t/right-clickwont-work-on-18-04/279/2) より
タッチパネルでサブメニュー（コンテキストメニュー）を表示する操作を調整できます。
```
gsettings set org.gnome.desktop.peripherals.touchpad click-method areas
```
２本指でのタップに戻す場合
```
gsettings set org.gnome.desktop.peripherals.touchpad click-method fingers
```
