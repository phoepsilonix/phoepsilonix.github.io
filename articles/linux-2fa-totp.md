---
title: "２要素認証で安心ログイン"
emoji: "🇯🇵"
type: " tech" # tech: 技術記事 / idea: アイデア
topics: [2fa,totp,pam,ssh]
lang: ja-JP
published: true
marp: true
---
# ２要素認証の導入
TOTPとパスワードの２要素認証を導入します。
libpam-google-authenticatorモジュールを使用します。
対象は、リモートログイン全般とデスクトップ画面のログイン画面です。
２要素認証(OTP + password)を必須にします。

Authyが、LinuxとAndroid両方で動作し設定を共有できて、扱いやすかったです。

ここではパスワードとワンタイムパスワードの２要素認証について、書いています。
sshで公開鍵認証（パスワードなし）とワンタイムパスワードの組み合わせによる、２要素認証を行う場合は他を参考にしてください。

---
以下の手順は、sshでのログインを __２要素認証必須__ にします。
端末のパスワードと(タイムベースの)ワンタイムパスワード、２つを共に入力して認証ができない場合は、sshでのログインは不可能になります。
リモートでの作業が不可能になることもありえます。
慎重に、実行してください。思い込み、きめつけは、やめましょう。

---
# 認証用アプリの用意
OTP用のアプリを用意します。
Google認証システム、Authy、Duo mobileなどがあります。
LinuxのDesktop版としては、Authyがsnapでbetaリリースされています。スマホのAuthyと設定を共有するので楽でした。

```
snap install --edge authy
```

arch系LinuxでのCUIクライアントは、oath-toolkit(oathtool)も利用できます。
```
sudo pacman -S oath-toolkit
oathtool -b --totp  'secret key'
```

---
# 認証モジュールにgoogle authenticatorを利用する
```
sudo pacman -S libpam-google-authenticator qrencode
```
qrencodeは、コンソール画面にQRコードを出力してくれるパッケージです。
秘密鍵を生成する際に、コンソール画面にQRコードが出力されます。

アプリから、カメラで読み込んで登録できるので便利です。

---
# system-remote-loginを２要素認証にする
認証手続き(PAM)でgoogle authenticatorを利用するように変更します。
もしリモート先の設定を変更する場合には、十分、注意してください。

/etc/pam.d/system-remote-login の先頭に一行追加します。
先頭に追加することで、TOTPによる認証が先に行われます。
```/etc/pam.d/system-remote-login
auth        required    pam_google_authenticator.so    nullok echo_verification_code no_increment_hotp       #2FA
auth        include     system-login
-auth       optional    pam_gnome_keyring.so
account     include     system-login
password    include     system-login
session     include     system-login
-session    optional    pam_gnome_keyring.so auto_start
```

sshサーバーの設定も変更しておきます。
/etc/ssh/sshd_config チャレンジレスポンス認証を有効(yes)にします。
```/etc/ssh/sshd_config
ChallengeResponseAuthentication yes
```
---
# lightdmを２要素認証にする
/etc/pam.d/lightdm の先頭に一行追加します。
```/etc/pam.d/lightdm
auth        required    pam_google_authenticator.so    nullok echo_verification_code no_increment_hotp       #2FA
auth        include     system-login
-auth       optional    pam_gnome_keyring.so
account     include     system-login
password    include     system-login
session     include     system-login
-session    optional    pam_gnome_keyring.so auto_start
```

---
# 秘密鍵の生成
```
google-authenticator
```
最初の質問に回答すると秘密鍵が更新されます。
このコマンドを実行したら、最後まで一通りやりましょう。

---
# 認証クライアントに秘密鍵を登録
QRコードをスキャンするか、入力して登録してください。
表示された数字を入力することで、タイムベースのOTP認証が問題なく行われているか確認します。
いくつかの質問に答えて、秘密鍵の設定は終了です。

うまくいかない場合には、見直してやり直します。
ArchWikiでは、時間に余裕をもたせるかどうかの質問にnとなっていますが、yにするのもいいでしょう。

---
# 接続テスト
sshでの接続テストを行います。
他の管理者アカウントでも２要素認証でログインできるように、アカウントを設定しておいた方が良いかもしれません。
万一に備えた、復旧手順を明確にすることは、とても大事です。

確認してから、sshサーバー再起動
```
sudo systemctl restart sshd
```

設定が正常なら、ログイン先が、リブートしたとしてもログインできるでしょう。

---
# デスクトップのログインテスト。
ログアウトして、セッションを終了します。
最初に、TOTPを尋ねられます。その次にパスワードです。
両方、認証されたら、ログインできます。


##### 参考情報
[Google Authenticator - ArchWiki](https://wiki.archlinux.jp/index.php/Google_Authenticator)

[Use oathtool Linux command line for 2 step verification (2FA) - nixCraft](https://www.cyberciti.biz/faq/use-oathtool-linux-command-line-for-2-step-verification-2fa/)
