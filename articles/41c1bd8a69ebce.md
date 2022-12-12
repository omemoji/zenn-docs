---
title: "Nuxt3+Tailwind CSSで静的なギャラリー付きポートフォリオを作成する"
emoji: "🔥"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nuxtjs", "tailwindcss"]
published: false
---

# この記事について

https://adventar.org/calendars/8236
この記事は [WORDIAN Advent Calendar 2022](https://adventar.org/calendars/8236) の 19 日目の記事である。

Nuxt3 と Tailwind CSS を用いて「創作物紹介」というギャラリー付きポートフォリオを作成し、GitHub Pages に独自ドメインでデプロイした。
その過程をこの記事で述べる。

# ポートフォリオ「創作物紹介」について

<!-- [![](https://user-images.githubusercontent.com/68148226/206378627-436399d5-f540-4a6b-adda-d8a68d9a0da0.png)](https://omemoji.com) -->

## 概要

https://omemoji.com

- サイト名: 創作物紹介
- リポジトリ: https://github.com/omemoji/omemoji.com

## 機能

このポートフォリオは以下の機能を持つ。

- 自分のプロフィール、興味のあることや出来ることを伝える
- これまでに作った作品(主に絵)をギャラリーとして公開する
- Markdown で書いた記事を公開する

## 技術構成

Node.js と npm のバージョンは以下の通り。`sudo n latest`でインストールした(2022/12/12 時点)。

```
v19.2.0 (with npm 8.19.3)
```

package.json は以下の通り。

```
  "devDependencies": {
    "@nuxt/content": "^2.2.2",
    "@nuxt/image-edge": "^1.0.0-27840416.dc1ed65",
    "autoprefixer": "^10.4.13",
    "nuxt": "3.0.0",
    "postcss": "^8.4.19",
    "tailwindcss": "^3.2.4"
  },
  "dependencies": {
    "github-markdown-css": "^5.1.0",
    "vue-gtag": "^2.0.1"
  }
```

# 目的

## 自分の作品・成果物を自由に見てもらう

私は絵を描いたりものをつくったりするのが好きで、作品を度々以下のような画像共有 SNS に投稿している。

- [Instagram](https://www.instagram.com/omemoji/)
- [Twitter](https://twitter.com/omemoji_itf)
- [krita-artists.org](https://krita-artists.org/u/omemoji/activity)

これらはとても良いサービスであり、私も愛用している。しかし、（自分で作ったのではない、大規模な外部サービスが必然的に抱える宿命として）以下の欠点を持っている。

- UI が気に入らなかったり不具合が生じていたりしても直すことが出来ず、ただ対応を待つしかない
- 多くのユーザーやデータを捌いているため仕方ないが、サイトや画像の読み込みに時間がかかる
- 急にサービスを終了したり運営の方針を転換したりして、サービスの利用に支障をきたすかも知れない
- 従業員の生活があるため仕方ないが、興味のない広告がある(krita-artists.org は広告なし)

自分のドメインでホームページを立ち上げてそこに作品や記事を公開すれば、（サイトのホスティングを行っている GitHub を信頼することで）上記の問題点を解決することが出来る。何より自由なのは魅力的である。

## 就職活動に役立てる

ギャラリーや記事などで（今はまだ少ないが）成果物やその作成過程を公開していくことで、企業に対して自分の持っている技術や熱意をアピールしたい。
また、このポートフォリオ自体も Web 技術に関する最低限の熱意を示す成果物になる（と信じたい）。

# 開発にあたって重視したこと（速度・デザインのしやすさを改善）

ポートフォリオの作成に際し、特にサイトの速度とデザインのしやすさを重視した。何故なら、以前のポートフォリオにはそれらが欠けており、Web サイトとしての利便性を損なっていたためである。

実はこのポートフォリオ自体は[omemoji.dev](https://github.com/omemoji/omemoji.dev)として以前から存在しており、Nuxt2+Vuetify で実装していた。
https://github.com/omemoji/omemoji.dev

しかし、このポートフォリオには 3 つの問題点があった。

## パフォーマンスが悪い

ページの読み込みが体感出来るレベルで遅かった。
実際に [PageSpeed Insights](https://pagespeed.web.dev/) で分析してみると、Mobile での Performance スコアが 30 点台という悲惨な点数であった。Desktop においても、60 点台とそこまで良いスコアとは言えなかった。
原因としては、多くの画像を使用していること、Vuetify は設定をきちんと行わないと不要なリソースや処理によってパフォーマンスが低下することの 2 点が考えられる。これらの問題を解決するために、まず画像の最適化は必須であると考えた。加えて、そもそも私のポートフォリオのデザインがシンプルであることから、Vuetify の設定を改善するよりももう少し軽量な CSS フレームワークの方が適しているのではないかとも思った。

## デザインの微調整がしづらい

Vuetify はマテリアルデザインのコンポーネントを多数用意しており、リッチな UI を驚くほど簡単に実装出来る。
しかし、コンポーネントが事前に用意されている手軽さはデザインの微調整のしづらさと表裏一体である。もちろんコンポーネントの形に合うようなレイアウトを心がけるべきなのだが、私の場合は個別の要素ごとにスタイルを調整したいケースが頻発した。

## 開発がしづらい

Nuxt2 のプロジェクト作成は以下のコマンドで行う。

```
npx create-nuxt-app <project-name>
```

しかし、（少なくとも私の環境では）`create-nuxt-app`を実行するためには node のバージョンを 14 まで下げなければならなかった。インストールされるパッケージも非推奨・脆弱性を示す警告がしばしば発生し、（私の知識や調査が不足している可能性が高いものの）Nuxt2 での開発に煩わしさを感じていた。

そんなある日のこと、
https://nuxt.com/v3
Nuxt3 の安定版が 11 月中旬にリリースされた。
そのため、これを機会に Nuxt のバージョンを一新し、ポートフォリオをリニューアルしようと考えた。
また、上記の問題点を踏まえた結果、自由度の高い CSS フレームワークである Tailwind CSS を導入することにした。
https://tailwindcss.com/
かくして私は時を同じくして購入していた新しいドメイン(omemoji.com)も携え、ポートフォリオのリニューアルにとりかかったのである。

# 開発していて感じたこと

ここからは、実際にポートフォリオを作成していく中での実装のポイントや困ったことなどを述べていく。

## 画像最適化