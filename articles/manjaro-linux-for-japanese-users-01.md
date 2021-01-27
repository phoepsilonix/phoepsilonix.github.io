---
title: "Manjaro Linux for Japanese Users"
emoji: "🇯🇵"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [Linux,Japan,newbie,Manjaro,arch]
published: false
---
# First step for Manjaro Linux for Japanese Users

at 2021-01-27
##### はじめの第一歩。これだけすれば、使えます。

Manjaro Linux Budgieをインストールしました。
コミュニティエディションのBudgieのインストーラーには日本語フォントが入っていませんでした。その場合、ネットに接続していれば、フォントのインストールで対応可能です。下記、１−３を行うと良いと思います。

インストール後に行ったほうが良いことをまとめます。

#### 1.パッケージの更新元ミラーサーバーを日本にする。
Make update servers geographically close to each other.
```
pacman-mirrors --geoip
または
pacman-mirrors -c Japan #country
```

#### 2.パッケージデータベースの更新
Update server database.
```
sudo pacman -Sy
あるいは
sudo pacman -Syy
```

#### 3.日本語フォントのインストール
Installing Japanese Fonts
```
sudo pacman -S ipa-fonts noto-fonts-cjk
```

#### 4.日本語入力をインストール
Install Japanese input fcitx-mozc
```
sudo pacman -S fcitx-mozc fcitx-configtool fcitx-table-other fcitx-table-extra
```

#### 5.絵文字フォントのインストール
I also love emoji.
```
sudo pacman -S noto-fonts-emoji unicode-emoji nodejs-emojione
```

#### 6.タッチパネルでコンテキストメニューの開き方を、右側のシングルタップにする。
Adjusts how the context menu is opened on the touch screen.
[Right click wont work on 18.04](https://discourse.ubuntubudgie.org/t/right-clickwont-work-on-18-04/279/2) より

```
gsettings set org.gnome.desktop.peripherals.touchpad click-method areas
```
２本指でのタップに戻す場合
```
gsettings set org.gnome.desktop.peripherals.touchpad click-method fingers
```
