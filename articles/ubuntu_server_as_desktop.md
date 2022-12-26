---
title: "Ubuntu Server を普段使いしてみよう"
description: "Ubuntu Server 22.04 にGUI環境を入れて普段使い"
date: "2022-12-24"
type: idea
emoji: "💻"
published: true
topics: ["ubuntu", "linux"]
---

# Ubuntu Server を普段使いしてみよう

![](https://omemoji.com/images/content/ubuntu_i3/ubuntu_i3_01.webp)

元記事は以下。
https://omemoji.com/articles/ubuntu_i3

## はじめに

私は Ubuntu **_Server_** を普段使いしている。「サーバ用のものを普段使い出来るのか」と疑問に思うかもしれないが、意外にもデスクトップ用としても使い勝手がよかったのでここに紹介したい。

## インストールスクリプト (一例)

- i3 ベースのデスクトップ環境
- Repository: https://github.com/omemoji/ubuntu-i3
- (optional) dotfiles: https://github.com/omemoji/dotfiles
- :::details Installation
  1.  [Get and install Ubuntu Server 22.04 on your computer]("https://ubuntu.com/download/server")
  2.  ```
      git clone https://github.com/omemoji/ubuntu-i3.git
      cd ubuntu-i3
      sh install.sh
      ```
  :::

- :::details Application

  | Category                | Application          |
  | ----------------------- | -------------------- |
  | Window manager          | i3                   |
  | Launcher                | i3-dmenu-desktop     |
  | Input method (Japanese) | Fcitx5 and Mozc      |
  | Sound                   | PipeWire (AAC codec) |
  | Terminal                | rxvt-unicode         |
  | Browser                 | Google Chrome        |
  | Mailer                  | Thunderbird          |
  | Editor                  | Visual Studio Code   |
  | File manager            | Thunar               |
  | Image viewer            | Ristoretto           |
  | PDF viewer              | Evince               |
  | Media player            | VLC                  |
  | Image editor            | GIMP, Inkscape       |

  | Category     | Application(CUI) |
  | ------------ | ---------------- |
  | Shell        | zsh              |
  | Browser      | w3m              |
  | Editor       | Vim, Neovim      |
  | File Manager | Ranger           |

  :::

## メリット

### 軽量かつ自由度の高い Ubuntu ベースのデスクトップ環境が手に入る

「軽量で自由度の高い」デスクトップ環境といえば、ある程度 Linux に慣れ親しんだ人は「Arch Linux を使えばいいのではないか」と考えるかもしれない。
また、パッケージ管理に apt を使いたいという場合は

- [Debian](https://debian.org) (デスクトップ環境なしでのインストールが可能)
- [Xubuntu](https://xubuntu.org/)、[Lubuntu](https://lubuntu.me) (Ubuntu ベース、軽量デスクトップ環境)
- [Regolith Linux](https://regolith-linux.org/) (Ubuntu ベース、i3 をウィンドウマネージャとして用いる)

も候補として挙げられるだろう。しかしながら、Ubuntu Server をあえて GUI 環境で使うことには以下のメリットがある:

- Arch Linux よりも環境構築が楽
- Arch Linux よりもパッケージが安定している
- Debian よりもパッケージが新しい上、場合によっては ppa で最新のアプリも入れられる
- Ubuntu ベースのディストリビューションよりも入れられるアプリの自由度が高いため、余計なものが最初から入っているということがない
- Regolith Linux よりもロゴがかっこいい (個人差あり)

まとめると、「自分で 1 から欲しいアプリだけを入れたいが、時々最新のアプリも入れつつ安定した環境を作りたい」人に向いていると言える。

### 個性を主張できる

私は coins (筑波大学情報学群情報科学類)に所属しているが、このような geek community では Linux を使っている人は決して珍しくない。そのため、Arch Linux を Thinkpad に入れるだけでは逆に没個性になりがちである。そこで、一見 Linux の中では beginner 向けに見える Ubuntu を使った上で「これ、実は Ubuntu **_Server_** なんですよ」と言うことで、どうせ初心者だろうと高をくくっていた相手に「一味違う」と思わせることが出来るのではないだろうか。

## 使ってみた感想

快適。その一言に尽きる。

私の場合は i3 をウィンドウマネージャとして用いているが、アイドル状態で消費メモリが 1GB に達しないというのがまず素晴らしい。また、

- 大量のタブを開きながら`npm run dev`で開発環境を開く
- Krita (OSS の高機能お絵描きアプリ) で絵を描きつつ動画も見る

というような高負荷の運用をしても、消費メモリが 8GB を超えることは殆どない。動作も素早く、ストレスフリーな生活を送る上で確実に役立ってくれる。

「とりあえず使ってみたい」という場合は、私が作成した上記のインストールスクリプトを仮想環境で試してみるのが良いと思う。
その軽量さ故に、低スペックの仮想環境でも問題なく動作するはずである。

## 注意点

### Tearing

Ubuntu Server は本来サーバ用の OS であるため、動画を見ていると画面にちらつき(Tearing)が生じることがある。これを回避するためには、`/etc/X11`下に以下のファイルを作成する。

#### Intel の場合

:::details ファイル名: 20-intel.conf

内容:

```
Section "Device"

  Identifier "Intel Graphics"

  Driver "intel"

  Option "TearFree" "true"

EndSection
```

:::

#### AMD の場合

:::details ファイル名: 20-amdgpu.conf
内容:

```
Section "Device"

  Identifier "AMD Graphics"

  Driver "amdgpu"

  Option "TearFree" "true"

EndSection
```

:::

### 音が出るようにする

サーバ用なので当然だが、Ubuntu Server からは音が出ない。そのため、自分でサウンドシステムをインストールする必要がある。

Linux 用のサウンドシステムとしては PulseAudio がよく知られているが、私は [PipeWire](https://wiki.archlinux.jp/index.php/PipeWire) をインストールした。
PipeWire は PulseAudio と互換性があり、AAC codec に対応しているなどのメリットがある。

:::details Install PipeWire

```

yes | sudo add-apt-repository ppa:pipewire-debian/pipewire-upstream
yes | sudo add-apt-repository ppa:pipewire-debian/wireplumber-upstream
yes | sudo apt update
yes | sudo apt install gstreamer1.0-pipewire libpipewire-0.3-{0,dev,modules} libspa-0.2-{bluetooth,dev,jack,modules} pipewire{,-{audio-client-libraries,pulse,bin,jack,alsa,v4l2,libcamera,locales,tests}} \
libpipewire-module-x11-bell \
wireplumber{,-doc} gir1.2-wp-0.4 libwireplumber-0.4-{0,dev}
yes | sudo apt install --no-install-recommends pavucontrol pulseaudio-utils

```

:::
