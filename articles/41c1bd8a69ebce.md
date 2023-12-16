---
title: "Nuxt3+Tailwind CSSでギャラリー付きポートフォリオを作成する"
emoji: "🍃"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["nuxtjs", "tailwindcss", "nuxt3", "vue", "個人開発"]
published: true
---

# この記事について

https://adventar.org/calendars/8236
この記事は [WORDIAN Advent Calendar 2022](https://adventar.org/calendars/8236) の 19 日目の記事である。

Nuxt3 と Tailwind CSS を用いて「創作物紹介」という静的なギャラリー付きポートフォリオを作成し、GitHub Pages に独自ドメインでデプロイした。
その過程をこの記事で述べる。

# ポートフォリオ「創作物紹介」について

![](https://user-images.githubusercontent.com/68148226/206378627-436399d5-f540-4a6b-adda-d8a68d9a0da0.png)

## 概要

### 名称: 創作物紹介

https://omemoji.com

### リポジトリ

(2023-12-19 追記：以下は Next.js でリニューアルする前のコミットである)

https://github.com/omemoji/omemoji.com/tree/f978bd12d1010264e44d4e45de18a786d047af5d

## 機能

このポートフォリオは以下の機能・特徴を持つ:

- 自分のプロフィール、来歴、興味のあることや技能を伝えることが出来る
- これまでに作った作品(主に絵)をギャラリーとして公開出来る
- Markdown で書いた記事を公開出来る
- 静的ウェブサイトであるため、ページを高速で表示出来る

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

# ポートフォリオ作成の目的について

## 自分の作品・成果物を自由に見てもらう

私は絵を描いたりものをつくったりするのが好きで、作品を度々以下のような画像共有 SNS に投稿している:

- [Instagram](https://www.instagram.com/omemoji/)
- [Twitter](https://twitter.com/omemoji_itf)
- [krita-artists.org](https://krita-artists.org/u/omemoji/activity)

これらはとても良いサービスであり、私も愛用している。しかし、（自分で作ったのではない、大規模な外部サービスが必然的に抱える宿命として）以下の欠点を持っている:

- UI が気に入らなかったり不具合が生じていたりしても直すことが出来ず、ただ対応を待つしかない
- 多くのユーザーやデータを捌いているため仕方ないが、サイトや画像の読み込みに長い時間がかかることがある
- SNS を管理している企業や団体が急にサービスを終了したり運営方針を転換したりして、サービスの利用に支障をきたすかも知れない
- ユーザーが興味のない広告がある(krita-artists.org は広告なし)

自分のドメインでホームページを立ち上げてそこに作品や記事を公開すれば、上記の問題点を解決することが出来る。UI も取り扱うコンテンツも、全てを自分で決めることが出来るのだ。

## 就職活動に役立てる

ギャラリーや記事などで（今はまだ少ないが）成果物やその作成過程を公開していくことで、企業に対して自分の持っている技術や熱意をアピールしたい。
また、このポートフォリオ自体も Web 技術に関する最低限の熱意を示す成果物になる（と信じたい）。

## 技術を高める

私は情報技術に関してあまり詳しいわけではなく、JavaScript や周辺のフレームワークに関してもまだまだ学習中である。そのため、目に見える形で成果物を作成することを通して、Web engineering の知識を深めたいと考えた。
知識の少ない状態から何とか Web サイトを形にしたという感じなので、実装にまずい部分があったり、モジュールやフレームワークの強みを活かせていなかったりするかも知れない。そういった点は今後の継続的な開発で改善していきたい。

# Nuxt3 と Tailwind CSS について

## Nuxt3

https://nuxt.com/

Nuxt.js は Vue.js をベースとした Web アプリケーションフレームワークである。Nuxt.js ではルーティング・データ管理を自動で行ってくれる、meta タグ管理を簡単に出来るといった特徴があり、Vue.js 単体と比べ様々な面で開発を効率化することが出来る。
また、多数の便利なモジュールに対応しており、手軽に様々な機能を実装することが出来る。

Vue.js 単体では不可能だった SSR に対応していることも特筆すべき点である。ただ、「創作物紹介」では動的にコンテンツを表示する必要はないので、SSG でページ生成を行っている。

Nuxt2 からの変更点（一部）は以下。

- 静的ページ生成やレンダリングの高速化
- TypeScript にデフォルトで対応
- `~/plugins`に置かれた plugin ファイルを自動でインポートする
- `/static`ディレクトリの名称が`/public`に変更

```
npx nuxi init <project-name>
```

でインストール出来る。

## Tailwind CSS

https://tailwindcss.com/

Tailwind CSS は CSS フレームワークの一種であり、事前に用意されたクラスを要素に追加することでスタイルを記述していく。カスタマイズ性が高い、軽量である、手軽にアニメーションやレスポンシブデザインを含めたスタイリングが出来るといったメリットがある。
一方で、インライン記述に近い形態をとるためコードが煩雑になりかねないというデメリットも持っている。ただ、 Vue.js や React などのフレームワーク を使えばコンポーネント単位で管理することが出来るため、この欠点はある程度解消する。

```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init
```

でインストール出来る。

# 開発のポイント

ここからは、実際にポートフォリオを作成していく中での実装のポイントを述べていく。

## デザイン

デザインについては、以下の観点を重視した:

- シンプルであること
- チープな印象を与えないこと
- レスポンシブデザインに対応すること
- 情報やサイトの構造が見やすいこと
- (様々な色を使っている作品群と色彩が衝突しないように)モノトーンに近いカラーリングにすること

加えて、既製のコンポーネントを用意しているタイプの UI フレームワークでは自分の求めるデザインに微妙に合致していないことが多かった(これが Tailwind CSS を選択した理由でもある)ので、他の Web サイトを参考に自分で UI をデザインした。

### デザインのこだわり

- ヘッダーがすりガラスのように背景をぼかして透過している
  ![screen24](https://user-images.githubusercontent.com/68148226/208279097-c6b22da1-6c9e-440c-991a-6094f3a42f49.png)
- Instagram や Pixiv を参考にした、正方形を基調としたギャラリー
  ![image](https://user-images.githubusercontent.com/68148226/208282966-3142107e-732b-4f9a-bd17-84ad76bdd656.png)

## 作品ページとギャラリーの実装

作品や成果物のページ、及びそれらのサムネイル・リンクをきれいに一覧して表示するギャラリーは、このポートフォリオの最も重要な部分である。しかし、多くの画像を扱うギャラリーを実装し、しかも効率的に運用するには、

- 作品ごとに自動でサムネイル一覧やページを生成すること
- サムネイル用、アイキャッチ用…という風に何枚も画像を用意することなく、自動で用途にあった画像最適化を行ってくれること

という 2 つの課題をクリアする必要がある。

### 自動でサムネイル一覧やページを生成する

#### サムネイル一覧

このポートフォリオでは、作品の画像やそれに紐づく一連のデータを json で管理している。例えば以下のように:

```json:/assets/json/artworks.json
[
  {
    "id": "zugadanten2022",
    "image": "/images/artworks/zugadanten2022.jpg",
    "title": "図画団展2022ポスター",
    "tool": "Krita",
    "href": "https://www.instagram.com/p/CkSHtqIBGFx/",
    "caption": "2022年度図画団展のポスター",
    "aspect_h": 1.414,
    "aspect_w": 1
  },
  {
    "id": "rainbow_dragon",
    "image": "/images/artworks/rainbow_dragon.png",
    "title": "虹龍",
    "tool": "Krita",
    "href": "https://www.instagram.com/p/Cb1wksDvX-i/",
    "caption": "極彩色の龍",
    "aspect_h": 9,
    "aspect_w": 16
  },
  //続く
]
```

これを読み込んで`v-for`で回せば、全作品のサムネイルを一覧したギャラリーが出来る。

```vue:/components/gallery.vue
<template>
  <div class="card card-shadow bg-white m-3 p-3">
    <h2 class="text-center mb-3">Gallery</h2>

    <div
      class="aspect-square overflow-y-auto over-contain card"
      style="font-size: 0"
    >
      <div
        v-for="(artwork, i) in artworks"
        :key="i"
        class="w-1/3 inline-block mb-0 border border-white"
      >
        <nuxt-link :to="'/artworks/' + artwork.id" :title="artwork.title">
          <nuxt-img
            :alt="artwork.title"
            :src="artwork.image"
            provider="ipx_fixed"
            width="240"
            height="240"
            quality="25"
            loading="lazy"
            class="object-cover aspect-square w-full"
          />
        </nuxt-link>
      </div>
    </div>
  </div>
</template>
<script>
import artworks from "@/assets/json/artworks.json";
export default {
  data() {
    return {
      artworks: artworks,
    };
  },
};
</script>
```

#### ページ生成

また、作品ページの自動生成では動的ルーティング機能を用いる。以下を参考にした。
https://nuxt.com/docs/guide/directory-structure/pages/#dynamic-routes
まず、例えば`/artworks`ディレクトリ内に自動でページを作成したい場合

```
-| pages/
---| index.vue
---| artworks/
-----| [id].vue
```

とすればよい。ここで`id`にあたる文字列は何でもいいが、後で参照するので分かりやすいものにしておくこと。
次に、先程の json ファイルを`JSON.parse(JSON.stringify())`を用いて読み込み、
`artwork.id === this.$route.params.id;`を満たす作品ページのみが生成されるようにする。
(ここで、`this.$route.params.id`は`[id].vue`のファイル名を示している)
参考までに、`[id].vue`の全体は以下のようになる。

https://github.com/omemoji/omemoji.com/blob/main/pages/Artworks/%5Bid%5D.vue

相当に力づくの方法だと思うので、この手法は今後修正するかもしれない。

### 画像最適化

画像最適化は「Nuxt Image」を用いて行った。
https://v1.image.nuxtjs.org/

```
npm install @nuxt/image-edge
```

でインストール出来る。

`<nuxt-img>`タグは従来の`<img>`タグを置き換えるものであり、複数のオプションによって様々な形で画像を最適化することが出来る。例えば、以下のように書く:

```
<nuxt-img>
  src="/title.png"
  width="720"
  height="450"
</nuxt-img>
```

この場合、`width`と`height`に設定されている通りに画像サイズが設定された後、それに合うように拡縮・トリミングされた画像が生成される。即ち、高解像度の画像を 1 枚入れておけば、`nuxt-img`によっていくらでも好きなサイズ・解像度の画像を自動で生成出来るということである。ギャラリーを実装する上でも、例えばサムネイル用に自前でトリミング・圧縮した画像を用意する必要がなくなり、開発を大幅に効率化することが出来た。
他にも、容量を圧縮する`quality`、遅延読み込みを可能にする`loading`、画像の形式を変換する`format`などの様々なオプションが存在する。

:::message
非常に便利なこのパッケージであるが、2022/12/12 時点で「特定の組み合わせで複数のオプションをつけると`nuxt run generate`で画像が生成されない」というバグがある。バグ修正の PR 自体は既に行われているようだ。
https://github.com/nuxt/image/pull/673
:::

## Markdown から記事を生成する

個人サイトである程度長い文章を書いていく際、HTML と CSS を使って文章やレイアウトを記述するのは骨が折れる。出来ることなら、より簡単に書けてレイアウトも勝手に決めてくれる Markdown で記述したい。そうした希望を叶えてくれるのが、Nuxt Content パッケージである。
https://content.nuxtjs.org/

```
npm install --save-dev @nuxt/content
```

でインストール出来る。また、これだけだとスタイリングがされないため、例えば GitHub 風のスタイルもインストールしておく。

```
npm install github-markdown-css
```

本体の Markdown は`/content`以下に配置する。例えば、ファイル構造は以下のようになる。

```
-| pages/
---| about.vue
---| articles/
-----| [...slug].vue
-----| index.vue
-| contents/
---| about.md
---| articles/
-----| hello.md
-----| goodbye.md
```

なお、なぜかディレクトリ名を大文字にしたとき上手くいかないことがあったので、ディレクトリやファイルは小文字で命名した方が良いと思う。

ここで、実際に Markdown ファイルを vue ファイルの中で読み込んでみる。
例えば`about.vue`を`about.md`と対応させるには、

```vue:/pages/about.vue
<template>
  <div class="card card-shadow p-6 m-3">
    <ContentDoc path="/about" />
  </div>
</template>
```

と記述すれば良い。ここでは、`<ContentDoc />`コンポーネントによって該当する path の Markdown ファイルが描画されている。

## GitHub Pages での自動デプロイ

GitHub Actions を用いる。ワークフローは以下。
https://github.com/omemoji/omemoji.com/blob/main/.github/workflows/gh-pages.yml
`./dist`以下のファイルを`gh-pages`ブランチに生成し、そのブランチ上でデプロイを行っている。

# 振り返りと課題・今後の展望

## 振り返り

Nuxt.js や Tailwind CSS のような手軽で軽量なフレームワークと優秀なモジュールのおかげで、作品ページ・ギャラリーの自動生成、Markdown による記事作成など複数の機能を持ったポートフォリオを実装することが出来た。以降は必要に応じて画像や記事を新しく追加すれば自動的に作品ページ・記事を生成してくれるため、保守がそこまで難しくないのも良い。
また、画像最適化が功を奏したのか、ギャラリーなどで多くの画像を使う割には PageSpeed Insights のスコアも良好であった。以下に示すように Mobile でも 90 点を超えており、少なくとも読み込みの遅さが原因でサイトの訪問者がしびれを切らすことはないはずである。
![screen23](https://user-images.githubusercontent.com/68148226/208253368-03eb6985-f548-44f5-b64e-17b78e776744.png)
![screen22](https://user-images.githubusercontent.com/68148226/208253126-fa6cce30-d485-41d7-b9ba-516c6dd50ea7.png)

## 課題・今後の展望

TypeScript、もっと言えば JavaScript もきちんと理解していないために Nuxt の機能についても理解が難しくなっていることを痛感したので、まずはそれらの基礎的な勉強をしていきたい。
その上で、現在かなり雑にやっている部分(json で動的ルーティングを管理しているという実装手法や Nuxt Content に対する理解など)を見直そうと思う。
また、

- サイト内の記事の中に外部リンクをブログカードとして埋め込む
- 作品ページで画像をクリックすると全画面に拡大する(これは簡単にできるモジュールがありそう)
- ダークモード
- 作品を Twitter でシェアするボタン
- 記事や作品をタグで整理する

などの機能を今後実装してみたい。

# おまけ

## Nuxt Image のバグについて

画像最適化の項で述べた通り、`nuxt-img`タグには複数のオプションを用いると特定条件下で SSG が出来ないというバグがある。原因としては、URL にカンマ(,)が混じることが考えられる。
例えば、以下のようにして最適化された画像について考えてみる。

```vue:
<nuxt-img
  :alt="artwork.title"
  :src="artwork.image"
  width="240"
  height="240"
  quality="25"
  loading="lazy"
  class="object-cover aspect-square w-full"
/>
```

この画像が最適化されたとき、最適化後の画像のリンクは例えば以下のようになる(これは開発環境では上手く働くが、静的ファイルを生成した際にはリンクがつながらず動作しない)。
`https://omemoji.com/_ipx/q_25,s_240x240/images/artworks/zugadanten2022.jpg`

上記の画像では`width`(w)と`height`(h)のみを設定する分には問題ないが、`quality`(q)オプションを指定するとバグが発生する。そのため、複数のオプションを区切る際にカンマで区切ってしまうことがその「特定条件」にあたると推測出来る。

以上を考えると、カンマ以外の何らかの文字でオプションを区切れば問題は解決すると考えられる。画像最適化のオプションの振る舞いは provider によって決定されるため、デフォルトの provider である ipx を修正したものを custom provider として`/providers/custom`ディレクトリに配置すれば良い。修正した`ipx_fixed.ts`は以下。

https://github.com/omemoji/omemoji.com/blob/main/providers/custom/ipx_fixed.ts

これはデフォルトの ipx の設定ファイルと殆ど変わらないが、`joinWith`の部分に`,`ではなく`&`が指定されている点が異なる。私はこの`ipx_fixed`を画像の provider に指定することで、問題を解決することが出来た。

なお、上でも触れたこの PR では区切り文字を`_`にすることが提案されていたが、私の環境では上手くいかなかった。
https://github.com/nuxt/image/pull/673
