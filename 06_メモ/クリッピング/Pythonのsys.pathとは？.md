---
title: "Pythonのsys.pathとは？"
source: "https://qiita.com/mei2678/items/53da7818e03722b451b2"
author:
  - "[[mei2678]]"
published: 2025-02-11
created: 2025-10-27
description: "はじめに 開発をしているとよく目にするsys.pathについて、きちんと理解できていなかったので学習メモです。 sys.pathとは Pythonはimport文を使ってモジュールを読み込みます。 その際、Pythonは「どのフォルダの中を探せばいいのか？」をリストで管..."
tags:
  - "clippings"
  - "python"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=3999280)

[@mei2678 (mei)](https://qiita.com/mei2678)

投稿日

## はじめに

開発をしているとよく目にする `sys.path` について、きちんと理解できていなかったので学習メモです。

## sys.pathとは

Pythonは `import` 文を使ってモジュールを読み込みます。  
その際、Pythonは「どのフォルダの中を探せばいいのか？」をリストで管理しています。  
このリストが `sys.path` です。

```python
import sys
print(sys.path)
```

実行結果の例

```bash
['/Users/user/project',
 '/Users/user/.pyenv/versions/3.10/lib/python3.10',
 '/Users/user/.pyenv/versions/3.10/lib/python3.10/site-packages']
```

このようにPythonはsys.pathに書かれたフォルダの中からパッケージを探します。

## sys.pathが作成・追記されるタイミング

### 1\. Pythonの起動時

- `python` コマンドで対話型シェルを起動時
- Pythonファイルを実行時
- `python -m module` でモジュールを実行時

pythonを起動すると自動的に `sys.path` が作成され、以下のような情報を元にリストが作成されます。

- スクリプトのあるディレクトリ(現在のディレクトリ)
- 標準ライブラリのディレクトリ(例: /usr/lib/python3.10)
- `site-packages` のディレクトリ(インストールされたパッケージの保存場所)
- (設定されている場合のみ) `PYTHONPATH` 環境変数に指定されたディレクトリ

### 2\. PYTHONPATH環境変数が設定されたとき

`PYTHONPATH` 環境変数を設定すると、そのディレクトリも `sys.path` に追加されます。

### 3\. sys.path.append()で手動追加したとき

Pythonのスクリプト内で `sys.path.appen()` を使うと、指定したディレクトリが `sys.path` に追加されます。　

```python
import sys
sys.path.append("/path/to/custom/module")
```

## sys.pathを確認するといいケース

### 1\. NotModuleFoundエラーが発生したとき

```python
import mymodule #ModuleNotFoundError
```

このようなエラーが発生したときは、 `sys.path` を確認し、対象モジュールのディレクトリが `sys.path` に含まれているかチェックしましょう。

```python
import sys
print(sys.path)
```

### 2\. カスタムモジュールを正しくインポートできないとき

プロジェクトの独自モジュールをインポートしようとしたとき、 `sys.path` にそのディレクトリが含まれているか確認します。

### 3\. パッケージのインストール後にimportできないとき

`pip install` したはずのパッケージが `import` できない場合、 `sys.path` に `site-packages` のディレクトリが含まれているかを確認します。

```python
import sys
print([p for p in sys.path if "site-packages" in p])
```

[0](https://qiita.com/mei2678/items/#comments)

コメント一覧へ移動

X（Twitter）でシェアする

Facebookでシェアする

はてなブックマークに追加する

[![mei2678](https://qiita-user-profile-images.imgix.net/https%3A%2F%2Favatars.githubusercontent.com%2Fu%2F124407903%3Fv%3D4?ixlib=rb-4.0.0&auto=compress%2Cformat&lossless=0&w=128&s=9ae630407249a27c1bd195a5617e3d12)](https://qiita.com/mei2678)

[

## @mei2678(mei)

](https://qiita.com/mei2678)

[RSS](https://qiita.com/mei2678/feed)

## Qiita Conference 2025 Autumn 11月5日(水)~7日(金)開催！

![](https://cdn.qiita.com/assets/public/official_campaigns/qiita_conference_2025_autumn/image-conference_2025_autumn_ogp-d35d2500cd0420d93f2ed69a8d162d74.png)

Qiita Conferenceは、AI時代のエンジニアに贈るQiita最大規模のテックカンファレンスです！

基調講演ゲスト(敬称略)

piacere、牛尾 剛、Esteban Suarez、和田 卓人、seya、ミノ駆動、市谷 聡啓、からあげ、岩瀬 義昌、まつもとゆきひろ、みのるん and more…

[イベント詳細を見る](https://qiita.com/official-campaigns/conference/2025-autumn)

[4](https://qiita.com/mei2678/items/53da7818e03722b451b2/likers)

いいねしたユーザー一覧へ移動

2