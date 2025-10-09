---
title: "Obsidian で数式を記述する"
source: "https://qiita.com/K-Fluid/items/c318b21448dfcbc2f960"
author:
  - "qiita"
published: 2023-03-05
created: 2025-07-06
description: "1. 背景 Obsidianでは 記法を用いて数式が書ける． しかし詳しく説明した記事が見当たらなかったので備忘録として作成する． MathJax をサポートしている．MathJax とは， などで記述されたコマンドを Web 上で数式に..."
tags:
  - "clippings latex"
---
More than 1 year has passed since last update.

[@K-Fluid](https://qiita.com/K-Fluid)

Last updated at Posted at 2023-03-05

## 1\. 背景

Obsidianでは $LATEX$ 記法を用いて数式が書ける．  
しかし詳しく説明した記事が見当たらなかったので備忘録として作成する．

[MathJax をサポートしている](https://help.obsidian.md/How+to/Format+your+notes#Math) ．MathJax とは， $LATEX$ などで記述されたコマンドを Web 上で数式に整形・表示するオープンソースの JavaScript ライブラリ．MathJax は [AMS-LaTeX 数学コマンドに対応するらしい](https://ja.wikipedia.org/wiki/MathJax) ．

\-> [AMS-LaTeX](http://www.ams.org/arc/resources/amslatex-about.html)

## 2\. 基本的な数式

## 2-1. 文中に数式を埋め込む（インライン数式）

- 数式を `$` で挟む．
- `\(` と `\)` で挟む方法は使えない．

e.g. `$f(x)=x^2$` と記述すると， $f(x)=x2$ となる．

## 2-2. 本文とは別に1行の数式を記述する（ディスプレイ数式）

- 数式を `$$` で挟む．
- $LATEX$ で使うべきでない作法とされているけど，Obsidian では `\[` と `\]` で挟む方法は対応しないみたいなので...

### ディスプレイ数式の例

```latex
$$
f(x)=x^2
$$
```

上記のように記述すると，つぎのようになる．  
$$
f(x)=x2
$$

## 3\. 複雑な数式

## 3-1. 長い数式を途中で改行する

- `\begin{split}` と `\end{split}` で数式を挟む．
- 改行位置に `\\` を入れる．行間をあけたければ `\\\\` を入れる．
- 揃えたい位置に `&` を入れる．必要に応じて `\quad` などで位置調整．

### 途中で改行した数式の例

```latex
$$
\begin{split}
    f(x) = a_9 \, x^9 &+ a_8 \, x^8 + a_7 \, x^7 + a_6 \, x^6 + a_5 \, x^5 \\
    &+ a_4 \, x^4 + a_3 \, x^3 + a_2 \, x^2 + a_1 \, x + a_0
\end{split}
$$
```

上記のように記述すると，つぎのようになる．  
$$
f(x)=a9x9+a8x8+a7x7+a6x6+a5x5+a4x4+a3x3+a2x2+a1x+a0
$$

## 3-2. 改行しながら式変形する

- `\begin{align}` と `\end{align}` で数式を挟む．
- 改行位置に `\\` を入れる．行間をあけたければ `\\\\` を入れる．
- `=` の前（すなわち揃えたい位置）に `&` を入れる．

### 式変形の例

```latex
$$
\begin{align}
    f(x) &= (x+1)^2 \\
    &= x^2 + 2x + 1
\end{align}
$$
```

上記のように記述すると，つぎのようになる．  
$$
f(x)=(x+1)2=x2+2x+1
$$

## 3-3. 連立方程式を立てる

- `\begin{align}` と `\end{align}` で数式を挟む．これをさらに `\left\{` と `\right.` で挟む．
- 改行位置に `\\` を入れる．行間をあけたければ `\\\\` を入れる．
- `=` の前（すなわち揃えたい位置）に `&` を入れる．

### 連立方程式の例

```latex
$$
\left\{
    \begin{align}
        2x + y &= 3 \\\\
        3x - 4y &= 1  
    \end{align}
\right.  
$$
```

上記のように記述すると，つぎのようになる．  
[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2444897/7d97fc57-c617-dd3f-656a-3ec9c1ee04ec.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2444897%2F7d97fc57-c617-dd3f-656a-3ec9c1ee04ec.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=cc542bb4eb9a4e950136fe24f22fb8fa)

## 3-4. 場合分けする

- 場合分けする数式を `\begin{cases}` と `\end{cases}` で挟む．
- 改行位置に `\\` を入れる．行間をあけたければ `\\\\` を入れる．
- `cases` 環境ではインライン数式モードになる．ディスプレイモードにするには `dcases` 環境にする．

### 場合分けの例

```latex
$$
\begin{equation}
    f(x) = 
    \begin{cases}
        e^{-1/x} & (x > 0) \\
        0 & (x \le 0)
    \end{cases}
\end{equation}
$$
```

上記のように記述すると，つぎのようになる．  
$$
f(x)={e−1/x(x>0)0(x≤0)
$$

## 3-5. 拡張（パッケージ）を使う

拡張を利用することで，煩雑な記述を単純化することができる．  
たとえば微分を含む数式を記述する場合を考える．

```latex
$$
\frac{\mathrm{d}y}{\mathrm{d}x}
$$
```

$$
dydx
$$
  
都度 `\mathrm{...}` で `d` を立体に指定するので記述が煩雑になる．

一方で `physics` 拡張を利用すると，簡単な記述でこれを実現できる． `\require{...}` は拡張を呼び出すコマンド．

```latex
$$
\require{physics} \dv{y}{x}
$$
```

ただし，拡張によって既存コマンドが上書きされることがあるので要注意．

\-> [数式をもっと楽に書ける TeX の physics パッケージまとめ](https://qiita.com/HelloRusk/items/ce9f49e9b3fc0344ae23)

## 4\. 数式の微調整

## 4-1. 記号間を調整する

記号をそのまま並べて記述すると記号間が詰まる場合がある．  
`\,` あたりで記号間の距離を調整すると見やすくなる．  
[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2444897/f907bb7d-50b0-f692-c406-6f05014618cb.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2444897%2Ff907bb7d-50b0-f692-c406-6f05014618cb.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fb574940c22eff33dcd784a61f004d36)

## 4-2. 行末に式番号をつける

- Obsidian ではデフォルトで数式番号がつかない．
- 付ける場合は数式の末尾に `\tag{...}` を置く．

### 式番号の例

```latex
$$
f(x)=x^2 \tag{1}
$$
```

上記のように記述すると，つぎのようになる．  
$$
(1)f(x)=x2
$$

## 5\. 化学反応式

`\ce` 環境を指定すれば化学反応式も書ける．  
`$\ce{Ag2O + 4NH3 -> 2[Ag(NH3)2]+ + 2OH-}$` と記述すると，つぎのようになる．  
[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2444897/5f56de37-bf04-e81c-22c4-02f1a9c93733.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F2444897%2F5f56de37-bf04-e81c-22c4-02f1a9c93733.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=62b0ef21af563326fc4fea1229bf01bf)  
\-> [TeXによる化学組版](https://doratex.hatenablog.jp/entry/20131203/1386068127)

## 6\. 参考

- [MathJax公式サイト](https://www.mathjax.org/)
- [MathJax公式ドキュメント](https://docs.mathjax.org/en/latest/)
- [MathJaxを使い始める](https://easy-copy-mathjax.nakaken88.com/introduction/)
- [Easy Copy MathJax](https://easy-copy-mathjax.nakaken88.com/)... MathJax のコマンド集
- [amsmathの数式環境まとめ](https://qiita.com/t_kemmochi/items/a4c390b4967b13f3afb7)

Register as a new user and use Qiita more conveniently

1. You get articles that match your needs
2. You can efficiently read back useful information
3. You can use dark theme
[What you can do with signing up](https://help.qiita.com/ja/articles/qiita-login-user)

[Sign up](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2FK-Fluid%2Fitems%2Fc318b21448dfcbc2f960&realm=qiita) [Login](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2FK-Fluid%2Fitems%2Fc318b21448dfcbc2f960&realm=qiita)