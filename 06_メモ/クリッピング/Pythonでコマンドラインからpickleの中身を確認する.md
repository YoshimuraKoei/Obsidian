---
title: "Pythonでコマンドラインからpickleの中身を確認する"
source: "https://qiita.com/yagays/items/3dd084469a7444714695"
author:
  - "[[yagays]]"
published: 2017-03-21
created: 2025-10-27
description: "tl;dr python -m picke hoge.pklでpickleの中身を直に見ることができることを初めて知った。これまで.pklにしたいけどlessできないしなと悩んでたのが馬鹿みたいだ…… https://t.co/QUXqS8WyLh— やぐ (@yag_..."
tags:
  - "clippings"
  - "python"
  - "pickle"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=3999280)

この記事は最終更新日から5年以上が経過しています。

### tl;dr

> python -m picke hoge.pklでpickleの中身を直に見ることができることを初めて知った。これまで.pklにしたいけどlessできないしなと悩んでたのが馬鹿みたいだ……　 [https://t.co/QUXqS8WyLh](https://t.co/QUXqS8WyLh)
> 
> — やぐ (@yag\_ays) [February 9, 2017](https://twitter.com/yag_ays/status/829566794918555648)

### 課題

Pythonで様々なオブジェクトをpickleという形式でシリアライズすることができます。一方で、jsonファイルやテキストファイルと違いバイナリファイルとなるため、catやlessで容易に確認することができず、いちいちpythonのインタプリタでwith句を書いて中身を取り出す必要があると思っていました。

```python
# 書き込み
with open("hoge.pkl", mode="wb") as f:
    pickle.dump({"hoge":"fuga"}, f)

# 読み込み
with open("hoge.pkl", mode="rb") as f:
    hoge = pickle.load(f)
```

ちなみに、上記のhoge.pklをcatで表示すると以下のようになります。

```sh
$ cat hoge.pkl
?}qXhogeqXfugaqs.%
```

### 解決法：シェルから直にpickleの中身を確認する

しかし、pythonのコマンドで直にpickleの中身を確認することができる方法がありました。それが `python -m pickle` というコマンドです。 `-m` というオプションはPythonの標準モジュールを実行してくれるため、pickleを指定するとその読み込み結果を表示してくれます。 `python -m SimpleHTTPServer` や `python -m http.server` でWebサーバが立ち上がるのと同じ仕組みですね。

上のpythonコードで作成したhoge.pklというファイルがあった場合に、以下のようにして中身を表示させることができます。

```sh
$ python -m pickle hoge.pkl
{'hoge': 'fuga'}
```

また、オブジェクトの場合は以下のように表示されます。

```sh
$ python -m pickle dictionary.pkl
<gensim.corpora.dictionary.Dictionary object at 0x104e7ac51>
```

### シェルのエイリアスに登録する

これだけでも個人的には大発見でpickleを積極的に利用できる理由になるほど便利なのですが、いちいち `python -m pickle` とタイプするのは大変なので、シェルの機能でaliasを設定してみます。

bash/zshの場合、以下のようになります。pickleはPythonのバージョンで非互換なので、python2/python3を明示的に指定したい場合は、絶対パスでpythonを指定しておいた方が良いでしょう。 

```sh
alias pcat='python -m pickle'
```

こうすることによって、 `$ pcat hoge.pkl` というコマンドで簡単にpickleファイルの中身を確認することができます。

### 参考

- [Is there a way to view cPickle or Pickle file contents without loading Python in Windows? - Stack Overflow](http://stackoverflow.com/questions/13568790/is-there-a-way-to-view-cpickle-or-pickle-file-contents-without-loading-python-in)

[0](https://qiita.com/yagays/items/#comments)

コメント一覧へ移動

X（Twitter）でシェアする

Facebookでシェアする

はてなブックマークに追加する

[![yagays](https://qiita-user-profile-images.imgix.net/https%3A%2F%2Fs3-ap-northeast-1.amazonaws.com%2Fqiita-image-store%2F0%2F4452%2F9bcd1c85723864c4f64393d6fab2587a3a3f39f9%2Fx_large.png%3F1639529208?ixlib=rb-4.0.0&auto=compress%2Cformat&lossless=0&w=128&s=7b4778de34a7af9cfea5411ef3aa6f3b)](https://qiita.com/yagays)

[

## @yagays

](https://qiita.com/yagays)

[Web](https://yag-ays.github.io/) [RSS](https://qiita.com/yagays/feed)

## Qiita Conference 2025 Autumn 11月5日(水)~7日(金)開催！

![](https://cdn.qiita.com/assets/public/official_campaigns/qiita_conference_2025_autumn/image-conference_2025_autumn_ogp-d35d2500cd0420d93f2ed69a8d162d74.png)

Qiita Conferenceは、AI時代のエンジニアに贈るQiita最大規模のテックカンファレンスです！

基調講演ゲスト(敬称略)

piacere、牛尾 剛、Esteban Suarez、和田 卓人、seya、ミノ駆動、市谷 聡啓、からあげ、岩瀬 義昌、まつもとゆきひろ、みのるん and more…

[イベント詳細を見る](https://qiita.com/official-campaigns/conference/2025-autumn)

[97](https://qiita.com/yagays/items/3dd084469a7444714695/likers)

いいねしたユーザー一覧へ移動

67