---
title: "【Google謹製】簡単便利なPython引数解析【Abseil】"
source: "https://xvideos.hatenablog.com/entry/argument_parse_with_abseil"
author:
  - "[[xterm256color]]"
published: 2018-06-06
created: 2025-10-27
description: "Pythonプログラムを書くときに、皆さんは引数解析にどのパッケージを使っていますか？ おそらく、ほとんどの人はargparseを使ってるんじゃないでしょうか。 実はGoogleが公開しているabseil（absl-py）というパッケージを使うと、argparseよりずっと簡単・便利に引数解析ができます。この記事ではabseilの使い方を紹介します。"
tags:
  - "clippings"
  - "python"
---
[Python](http://d.hatena.ne.jp/keyword/Python) プログラムを書くときに、皆さんは引数解析にどのパッケージを使っていますか？

おそらく、ほとんどの人はargparseを使ってるんじゃないでしょうか。

実は [Google](http://d.hatena.ne.jp/keyword/Google) が公開している [abseil（absl-py）](https://github.com/abseil/abseil-py) というパッケージを使うと、argparseよりずっと簡単・便利に引数解析ができます。この記事ではabseilの使い方を紹介します。

![f:id:xterm256color:20180606002728j:plain](https://cdn-ak.f.st-hatena.com/images/fotolife/x/xterm256color/20180606/20180606002728.jpg)

f:id:xterm256color:20180606002728j:plain

## 目次

## インストール方法

```
pip install absl-py
```

## サンプルコード

百聞は一見にしかず。ということでabseilのサンプルコードを載せていきます。

```python
# sample.py
from absl import app
from absl import flags

FLAGS = flags.FLAGS

flags.DEFINE_string(
    'foo', 'default value', 'help message of this argument.')

def main(argv):
  print('FLAGS.foo is {}'.format(FLAGS.foo))

if __name__ == '__main__':
  app.run(main)
```

## サンプルコードの実行結果

たとえば、パラメータを与える場合は `sample.py --foo bar` のようにします。

```sh
$ python sample.py --foo bar
FLAGS.foo is bar
```

また、 `sample.py --help` や `sample.py -?`でヘルプメッセージを表示することもできます。

```sh
$ python sample.py --help

       USAGE: sample.py [flags]
flags:

sample.py:
  --foo: help message of this argument.
    (default: 'default value')

Try --helpfull to get a list of all flags.
```

## 簡単な説明

上のサンプルコードを簡単に説明します。

**1\. absl.app, abls.flagsをimportして、FLAGS [インスタンス](http://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) を作る**

```python
from absl import app
from absl import flags

FLAGS = flags.FLAGS
```

**2\. フラグを定義する。今回は文字列型で受け取るフラグを定義しています**

```python
flags.DEFINE_string(
  'foo', 'default value', 'help message of this argument.')
```

**3\. main関数を `absl.app.run(main)` で呼び出す**

```python
if __name__ == '__main__':
  app.run(main)
```

**4\. `FLAGS.xxx` でセットされたパラメータを取得する**

```python
print('FLAGS.foo is {}'.format(FLAGS.foo))
```

どうでしょう？めっちゃ簡単でしょ？

## abseilを使う理由

私がargparseでなくabseilを使う理由は大きいものだと3つ。

**argparseよりシンプルに書くことが出来る。** argparseを使っていると `parser = argparse.ArgumentParser()` とか `parser.parse_args()` やら書かないといけないですが、abseilなら上のサンプルのように簡単にかけるのが嬉しいです

**ヘルプメッセージが親切。** 上のサンプル実行結果を見てもらうと分かりますが、フラグの既定値がきちんとヘルプメッセージにも表示されてますよね。argparseにはこの機能が無いので、abseilを使う一つの理由になっています

**bool型やlist型、 [enum](http://d.hatena.ne.jp/keyword/enum) 型の引数が設定しやすい。** argparseでbool型のパラメータを使おうとすると

```python
parser.add_argument('--feature', dest='feature', action='store_true')
parser.add_argument('--no-feature', dest='feature', action='store_false')
parser.set_defaults(feature=True)
```

みたいな書き方しないといけませんよね。書き方覚えてられないし自分にはこれがすっっっごく面倒でした。

abseilだと `flags.DEFINE_bool()` という関数が用意されているので、bool型のパラメータも簡単に追加することができます。 例えば、

```python
flags.DEFINE_bool('bar', True, 'some message...')
```

とすると `--bar` と `--nobar` というフラグが追加されます。 `--nobar` を実行時引数に与えると、 `bar=False` となる寸法です

他にも `flags.DEFINE_list()` や `flags.DEFINE_emum()` といった関数も用意されていていずれも便利です

## 細かい説明

ここからはabseilの細かい使い方の説明。というか自分用のメモです。読み飛ばしても問題ないです。

### 各種フラグの型

```python
# str型, int型, float型, bool型
flags.DEFINE_string('s', 'default value', 'help message of this argument.')
flags.DEFINE_integer('i', 1, 'help message of this argument.')
flags.DEFINE_float('f', 0.1, 'help message of this argument.')
flags.DEFINE_bool('b', True, 'help message of this argument.')

# list型。\`--hoge x,y\`と指定すると['x', 'y']と解析される
flags.DEFINE_list(
    'hoge', None, 'help message of this argument.')

# enum型。下記の場合はa,b,c以外の値を指定するとエラーが返ってくる
flags.DEFINE_enum(
    'restricted', 'a', ['a', 'b', 'c'], 'help message of this argument.')

# alias。ここでは--barを--fooの別名とみなしている。duplicatedなフラグがあるときに便利そう
flags.DEFINE_bool('foo', True, 'help message of this argument.')
flags.DEFINE_alias('bar', 'foo')
```

### すべての引数を取得する

```python
# dict型ですべてのフラグを取得
FLAGS.flag_values_dict()
```

### フラグの短縮名を設定する

```python
# short_name='x'でフラグの短縮名を設定。-fでも--fugaでも使えるようになる
flags.DEFINE_list(
    'fuga', None, 'help message of this argument.', short_name='f')
```

### フラグ値を上書きする

```python
FLAGS.foo = 'a'
```

### Requiredなフラグ設定

```
# --hogeと--fugaが必須フラグの場合。hoge, fugaの既定値はNoneにしておくこと
if __name__ == '__main__':
  flags.mark_flags_as_required(['hoge', 'fuga'])
  app.run(main)
```

### ファイルからフラグを設定

```sh
# 実行時に --flagfile /path/to/param.txt と指定すると、param.txtから引数を読み込む
# コマンドライン引数を指定した場合はparam.txtに書かれた値よりも優先される。
$ python sample.py --flagfile /path/to/param.txt
```

param.txtには以下のように引数を記述する

```
--hoge=a
--fuga=1
```

## よくある間違い

```
Traceback (most recent call last):
  File "./sample.py", line 43, in <module>
    app.run(main)
  File "/home/n/anaconda2/envs/absl/lib/python2.7/site-packages/absl/app.py", line 274, in run
    _run_main(main, args)
  File "/home/n/anaconda2/envs/absl/lib/python2.7/site-packages/absl/app.py", line 238, in _run_main
    sys.exit(main(argv))
TypeError: main() takes no arguments (1 given)
```

**解決策： `def main():`を `def main(argv):`に直しましょう。**

```
WARNING: Logging before flag parsing goes to stderr.
E0606 00:00:59.346868 139914210911616 _flagvalues.py:487] Trying to access flag --s before flags were parsed.
Traceback (most recent call last):
  File "./sample.py", line 44, in <module>
    main(sys.argv[1:])
  File "./sample.py", line 27, in main
    print('FLAGS.s is {}'.format(FLAGS.s))
  File "/home/n/anaconda2/envs/absl/lib/python2.7/site-packages/absl/flags/_flagvalues.py", line 488, in __getattr__
    raise _exceptions.UnparsedFlagAccessError(error_message)
absl.flags._exceptions.UnparsedFlagAccessError: Trying to access flag --s before flags were parsed.
```

**解決策： `main` 関数を直接呼び出していないですか？ `app.run(main)` の形で呼び出しましょう。**

## まとめ

- argparse代替となる [Google](http://d.hatena.ne.jp/keyword/Google) 謹製のabseil (absl-py) の紹介。
- 私がabseilを使う理由は
	- abseilはargparseよりシンプルに書ける。
	- ヘルプメッセージが親切。
	- bool型のフラグを設定しやすい

[« NVIDIA DALIを使ってみた（DALI単体編）](https://xvideos.hatenablog.com/entry/nvidia_dali_report) [SphinxドキュメントのためのDockerイメー… »](https://xvideos.hatenablog.com/entry/docker_for_sphinx)