---
title: "Pythonのイテラブルとは何かを現役エンジニアが解説【初心者向け】"
source: "https://magazine.techacademy.jp/magazine/28147"
author:
  - "[[TechAcademyマガジン]]"
published: 2018-01-25
created: 2025-10-27
description: "初心者向けにPythonのイテラブルとは何かを現役エンジニアが解説しています。イテラブルとは繰り返し可能なオブジェクトのことでリストやタプルやrange関数で作成したオブジェクトのことです。for文でイテラブルなオブジェクトを繰り返し処理してみましょう。"
tags:
  - "clippings"
  - "python"
---
初心者向けにPythonのイテラブルとは何かを現役エンジニアが解説しています。イテラブルとは繰り返し可能なオブジェクトのことでリストやタプルやrange関数で作成したオブジェクトのことです。for文でイテラブルなオブジェクトを繰り返し処理してみましょう。

テックアカデミーマガジンは [受講者数No.1のプログラミングスクール「テックアカデミー」](https://techacademy.jp/) が運営。初心者向けにプロが解説した記事を公開中。現役エンジニアの方は [こちら](https://techacademy.jp/magazine/engineer) をご覧ください。 ※ アンケートモニター提供元：GMOリサーチ株式会社　調査期間：2021年8月12日～8月16日　 調査対象：2020年8月以降にプログラミングスクールを受講した18～80歳の男女1,000名　 調査手法：インターネット調査

Pythonのイテラブルとは何かを、TechAcademyのメンター（現役エンジニア）が実際のコードを使用して、初心者向けに解説します。

Pythonについてそもそもよく分からないという方は、 [Pythonとは](https://techacademy.jp/magazine/15507) 何なのか解説した 記事を読むとさらに理解が深まるでしょう。

なお本記事は、TechAcademyのオンラインブートキャンプ、 [Python講座](https://techacademy.jp/python-special) の内容をもとに紹介しています。

![](https://techacademy-magazine-cdn.techacademy.jp/magazine/wp-content/uploads/2018/02/20163020-2-400x400-e1517449291396.png)

田島悠介

今回は、Pythonに関する内容だね！

![](https://techacademy-magazine-cdn.techacademy.jp/magazine/wp-content/uploads/2018/02/20160620-400x400-e1517449269611.png)

大石ゆかり

どういう内容でしょうか？

![](https://techacademy-magazine-cdn.techacademy.jp/magazine/wp-content/uploads/2018/02/20163020-2-400x400-e1517449291396.png)

田島悠介

Pythonのイテラブルとは何か詳しく説明していくね！

![](https://techacademy-magazine-cdn.techacademy.jp/magazine/wp-content/uploads/2018/02/20160620-400x400-e1517449269611.png)

大石ゆかり

お願いします！

## イテラブルとは

イテラブルとは、一言で言うと「繰り返し可能なオブジェクト」のことです。

for文において、文字列や数字を繰り返すことが可能であり、「for i in A:のAの部分に用いることができるもの」といえます。

例えばリスト、タプルやrange関数で生成したオブジェクトなどがイテラブルに該当します。

## イテレータとは

イテレータとは、「イテラブルオブジェクトを使用した連続データ」のことです。

イテレータは以下のようにiter()という関数を用いて作成することができます。

iter()の引数にはイテラブルを与えてあげる必要があり、イテラブルがなければイテレータを作成できません。

では、実際にイテレータを作成してみましょう。

```
>>>list_a = [1,2,3,4,5]
>>>iter_a = iter(list_a)
```

これでiter\_aという変数にイテレータを格納することができました。

イテレータはnext()という関数を用いて要素を順に取り出すことが可能です。

しかし、リストなどとは違って要素を取り出すごとに、取り出された要素は消滅していくことに注意しましょう。

これがイテレータの最大の特徴です。

実際にコードを書いて確認してみましょう。

```
>>>print(next(iter_a))
1
>>>print(next(iter_a))
2
>>>print(next(iter_a))
3
>>>print(next(iter_a))
4
>>>print(next(iter_a))
5
>>>print(next(iter_a))
StopIteration  Traceback (most recent call last)
<ipython-input-14-cbbee29b6a81> in <module>
```

このコードではnext()を呼び出すごとに新たな要素が取得されているのが確認できました。

また、6度目のnext()の実行でStopIterationというエラーになります。

これは、イテレータの中身が空の状態で要素を取り出そうとしたために起きたエラーです。

[\[PR\] 未経験からWebエンジニアを目指す方法とは](https://techacademy.jp/lp/branded)

## for文でイテラブルなオブジェクトを処理してみよう

ここでは、for文に対するイテラブルなオブジェクトを用いてみましょう。

```
for i in range(9):
    print(i)
```

上のコードは普段から何気なく使うfor文です。

しかし、for文では「StopIterationが起きたらfor文から抜ける」といった処理が内部的に行われています。

つまり、for文にも内部的にイテレータが使われているということです。

イテレータを用いることのメリットについての詳細には、ここではふれません。

しかし、for文の機能を作るためには必要な仕組みでだといえます。

## まとめ

この記事では、イテラブルやイテレータについて解説しました。

再度まとめると、イテラブルとは「for i in AのAの部分に用いることができるオブジェクト」、イテレータとは、「イテラブルオブジェクトを使用した連続データ」のことです。

ややこしい部分ではあるものの、これからPythonを勉強するうえで必要な部分もあるため、知識として知っておきましょう。

## 執筆してくれたメンター

| **柴山真沙希（しばやままさき）**  大手IT企業などでエンジニアとして2年ほど勤務した後、個人事業主としてプログラミングスクール「エンペサール」を経営。  子供から大人まで幅広い層を対象にプログラミングを教えている。  得意言語はPython, HTML, CSSで、機械学習やデータ分析、スクレイピングなどが得意。  サッカー観戦や読書が趣味である。 |
| --- |

![](https://techacademy-magazine-cdn.techacademy.jp/magazine/wp-content/uploads/2018/02/20160620-400x400-e1517449269611.png)

大石ゆかり

Pythonのイテラブルがどんなものなのか分かったので良かったです！

![](https://techacademy-magazine-cdn.techacademy.jp/magazine/wp-content/uploads/2018/02/20163020-2-400x400-e1517449291396.png)

田島悠介

ゆかりちゃん、これからも分からないことがあったら質問してね！

![](https://techacademy-magazine-cdn.techacademy.jp/magazine/wp-content/uploads/2018/02/20160620-400x400-e1517449269611.png)

大石ゆかり

分かりました。ありがとうございます！

TechAcademyでは、初心者でも、Pythonを使った人工知能（AI）や機械学習の基礎を習得できる、 [オンラインブートキャンプ](https://techacademy.jp/python-special) を開催しています。

また、現役エンジニアから学べる [無料体験](https://techacademy.jp/lp-htmlcss-trial-s) も実施しているので、参加してみてください。

※ 経済産業省の行うリスキリングを通じたキャリアアップ支援事業の補助金対象コースです。条件を満たすことで支払った受講料の最大70％がキャッシュバックされます。詳しくは [こちら](https://techacademy.jp/benefits/reskilling) のページをご確認ください。