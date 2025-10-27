---
title: "pythonのos.path.joinについてざっくりまとめる"
source: "https://qiita.com/keishi04hrikzira/items/83e323f1c3d119d2bb77"
author:
  - "[[keishi04hrikzira]]"
published: 2021-10-24
created: 2025-10-27
description: "本日のお題 本日のお題はpythonのos.path.joinです。 このコードは、パスを繋げて新しいパスを生成する役割を持っています。 djangoを学習していたところcssファイルの読み込み経路の設定でこのos.path.joinの知識が出てきたので、備忘録として残し..."
tags:
  - "clippings"
  - "python"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=3999280)

この記事は最終更新日から3年以上が経過しています。

### 本日のお題

本日のお題はpythonのos.path.joinです。

このコードは、パスを繋げて新しいパスを生成する役割を持っています。

djangoを学習していたところcssファイルの読み込み経路の設定でこのos.path.joinの知識が出てきたので、備忘録として残しておきます。

### 目次

- os.path.joinの文型
- 注意点１〜リスト型のパスの結合
- 注意点２〜/を含むパスについて

### os.path.joinの文型

os.path.joinの基本構文及び具体的な使用例は以下の通りです。  
また、osモジュールはimportが必要です。

```python
# import
import os

# 基本構文
os.path.join("パス１", "パス2")

# 具体例
path = os.path.join( ("main", "project1/test1")
```

以下の例では、pathという変数に"main/project1/test1というパスが代入されています。

第一引数の下流に第二引数が連結されることに注意してください。

### 注意点１〜リスト型のパスの結合

リスト型のパスを結合する場合には、以下のように記述します。

```python
list = ["main", "project1", "test1"]
path = os.path.join(*list)
```

### 注意点２〜/を含むパスについて

os.path.joinには、"/main"などの先頭に/を含むパスをルートパスとして解釈してしまう、という性質があります。

これはなんとなく分かりますね。  
ターミナルでのパス表記もルートディレクトリは/で表されているので。

/で始まるパスを結合した場合の具体例は以下になります。

```python
path = os.path.join("/main", "/project1", "/test1")

print(path) #結果："/test1"
```

このような場合はreplaceメソッドを用いて/を取り除いてあげることで、意図した通りのパスを取得できるようになります。

```python
list = ["/main", "/project", "/test"]
list = [dir.replace("/", "") for dir in list] # for inを用いて要素の/を全て削除
path = os.path.join(*list)
```

### 終わりに

以上がos.path.joinの使い方になります。  
次回はこれを用いてcssファイル・画像ファイルの配置を指定していきます。

[1](https://qiita.com/keishi04hrikzira/items/#comments)

コメント一覧へ移動

X（Twitter）でシェアする

Facebookでシェアする

はてなブックマークに追加する

この記事は以下の記事からリンクされています

- [djangoのpathlibについてざっくりとまとめる](https://qiita.com/keishi04hrikzira/items/90a1ee4d9a4513e44144)

## Qiita Conference 2025 Autumn 11月5日(水)~7日(金)開催！

![](https://cdn.qiita.com/assets/public/official_campaigns/qiita_conference_2025_autumn/image-conference_2025_autumn_ogp-d35d2500cd0420d93f2ed69a8d162d74.png)

Qiita Conferenceは、AI時代のエンジニアに贈るQiita最大規模のテックカンファレンスです！

基調講演ゲスト(敬称略)

piacere、牛尾 剛、Esteban Suarez、和田 卓人、seya、ミノ駆動、市谷 聡啓、からあげ、岩瀬 義昌、まつもとゆきひろ、みのるん and more…

[イベント詳細を見る](https://qiita.com/official-campaigns/conference/2025-autumn)

[4](https://qiita.com/keishi04hrikzira/items/83e323f1c3d119d2bb77/likers)

いいねしたユーザー一覧へ移動

2