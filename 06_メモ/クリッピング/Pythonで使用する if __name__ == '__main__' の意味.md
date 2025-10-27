---
title: "Pythonで使用する if __name__ == '__main__': の意味"
source: "https://qiita.com/soicchi/items/45ac8e2006361126b023"
author:
  - "[[soicchi]]"
published: 2022-04-08
created: 2025-10-27
description: "概要 Pythonを学習していると__name__ == '__main__'という謎の構文が出てきて、 それについて調べたのでアウトプットしたいと思います。 使い方 __name__ == '__main__'は簡単に言うとファイルを読み込んだ際に 関数などの実行を制..."
tags:
  - "clippings"
  - "python"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=3999280)

この記事は最終更新日から3年以上が経過しています。

[@soicchi](https://qiita.com/soicchi)

最終更新日 投稿日 2022年04月08日

## 概要

Pythonを学習していると `__name__ == '__main__'` という謎の構文が出てきて、  
それについて調べたのでアウトプットしたいと思います。

## 使い方

`__name__ == '__main__'` は簡単に言うとファイルを読み込んだ際に  
関数などの実行を制御してくれるものです。  
例えば下記のようなファイルがあるとします。

sample.py

```python
def hello():
  print("Hello!")

hello()
```

`hello()` を別ファイルで使用しようとしたときファイルをimportします。

example.py

```python
from sample import hello

hello()
```

このとき `example.py` ファイルを実行するとどうなるか

```text
python example.py // pyファイルの実行コマンド
// => Hello!
      Hello!
```

`Hello!`が2度表示されてしまいます。  
これはファイルをimportすると読み込み先のファイルに記載されている関数が実行されるためです。  
これを制御するために `if __name__ == '__main__':`を使用します。  
先程の `sample.py` ファイルを下記のように修正します。

sample.py

```python
def hello():
  print('Hello!')

# 追加
if __name__ == '__main__':
  hello()
```

こうすることで `sample.py` をimportした際に `hello()` は実行されなくて済みます。

## 仕組み

`__name__` というのはPythonの特殊な変数で、モジュール名の文字列が格納されます。  
`import hello` のようにモジュールが呼ばれることによって `__name__` に `'hello'` が格納されます。  
しかし `__name__ == '__main__'` が記載されたファイルを直接実行（ `python hello.py` ）すると `__name__` が `'__main__'` という文字列になります。  
つまり `if '__main__' == '__main__'` となり定義元のファイルを実行したときだけ呼び出すということになります。  
なので `example.py` を `python example.py` で実行した場合、 `'hello' == '__main__'` となり実行されない。

[1](https://qiita.com/soicchi/items/#comments)

コメント一覧へ移動

X（Twitter）でシェアする

Facebookでシェアする

はてなブックマークに追加する

## Qiita Conference 2025 Autumn 11月5日(水)~7日(金)開催！

![](https://cdn.qiita.com/assets/public/official_campaigns/qiita_conference_2025_autumn/image-conference_2025_autumn_ogp-d35d2500cd0420d93f2ed69a8d162d74.png)

Qiita Conferenceは、AI時代のエンジニアに贈るQiita最大規模のテックカンファレンスです！

基調講演ゲスト(敬称略)

piacere、牛尾 剛、Esteban Suarez、和田 卓人、seya、ミノ駆動、市谷 聡啓、からあげ、岩瀬 義昌、まつもとゆきひろ、みのるん and more…

[イベント詳細を見る](https://qiita.com/official-campaigns/conference/2025-autumn)

[38](https://qiita.com/soicchi/items/45ac8e2006361126b023/likers)

いいねしたユーザー一覧へ移動

45