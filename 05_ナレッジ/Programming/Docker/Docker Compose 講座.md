---
title: Docker Compose 講座
AI:
published: 2025-09-06
description:
tags:
  - permanent-note
  - docker
---
**Docker講座**
- https://engineer-ninaritai.com/infraacademy/courses/docker-compose/

---
##### Docker Composeとは？

ここでは、Docker Composeの概要について解説します。

その前に、Dockerについて軽く復習しておきましょう。

###### Dockerがコンテナを起動する流れ(復習)

Dockerは、インフラやシステムをコンテナと呼ばれる仮想環境にまとめる技術のことです。

Dockerイメージから、Dockerコンテナを起動させて、アプリケーションを動かします。
![[Pasted image 20250907004249.png]]

Dockerイメージとは、アプリケーションやサービス、その依存関係、および実行環境を含むファイルのことです。Dockerイメージは、Dockerfileと呼ばれるテキストファイルに基づいて構築されます。

また、Dockerコンテナは、実行可能なイメージのインスタンス(実体)であり、アプリケーションやサービスが実際に動作する環境のことです。

###### Dockerのコンテナを複数起動する場合

ここで、一般的なWebアプリケーションをDocker環境にすると考えてみましょう。

Webアプリケーションを構築するには、さまざまなサーバーが必要です。

例えば、

- Webサーバー
- データベース
- メールサーバー

などが必要です。

Webサーバーはソースコードを動かすために必要です。

データベースは、会員登録情報や決済情報などWebアプリに必要なデータを保存します。

メールサーバーは、会員登録時の通知などのメールを送るために必要です。

![[Pasted image 20250907004343.png]]

**これらのサーバーを、すべてDockerで動かすことを考えてみましょう。**

３つのサーバーをコンテナとして動作させるためには、docker runコマンドを３回も入力する必要があります。

docker runコマンドを何回も入力するのは面倒ですよね！

![[Pasted image 20250907004359.png]]

そこで、使うのがDocker Composeです。

**Docker Composeは、複数のコンテナを同時に起動させたり、管理することができます。**

![[Pasted image 20250907004415.png]]

Docker Composeは、基本的にdocker-compose.ymlファイルに各コンテナの設定を定義します。

docker-compose.ymlファイルを作成したら、docker-composeコマンドを使って、コンテナの作成や起動を行います。

次は、ハンズオンです。

Docker Composeを使ってみましょう!

---
##### Docker Composeのハンズオン

ここからは、Docker Composeのハンズオンを行います。

###### 今回構築する構成

今回は、Webサーバーとデータベースの2つをコンテナで起動させます。

docker-compose.ymlファイルを作成して、2つのコンテナの設定を行います。

![[Pasted image 20250907004513.png]]

###### docker-compose.ymlの作成

Docker Composeは、docker-compose.ymlファイルにコンテナの設定を定義します。

基本的にdocker runコマンドで入力したオプションなどをdocker-compose.ymlファイルに定義します。

[シミュレーター](https://terminal.engineer-ninaritai.com/)を起動して、以下のコマンドを実行しましょう。

```bash
vi docker-compose.yml
```

ファイルには、以下の内容を記述します。

```bash
version: '3'

services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    depends_on:
      - db

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: mydatabase
      MYSQL_USER: myuser
      MYSQL_PASSWORD: mypassword
    ports:
      - "3306:3306"
```

ファイルの内容については、後ほど詳しく説明します。

###### コンテナの起動

**docker-compose.ymlファイルの記述が完了したら、コンテナを起動してみましょう。**

以下のコマンドを実行します。

```bash
docker-compose up -d
```

コマンドを実行後、30秒程度待ちましょう。Dockerイメージのダウンロードやビルドが走ります。

コマンドの実行が完了したら、コンテナの確認をしてみましょう。

```bash
docker ps
```

コマンドを実行すると、以下のように2つのコンテナが起動していることが分かります。
![[Pasted image 20250907004616.png]]

###### コンテナの停止

Docker Composeでコンテナを停止(コンテナの削除)する際は、以下のコマンドを実行します。

```bash
docker-compose down
```

このように、Docker Composeでは、ymlファイルを作成して、upコマンドで起動、downコマンドで停止をします。

次に、docker-compose.ymlファイルについて詳しく解説します。

---
##### docker-compose.ymlファイルとは？

前回は、Docker Composeのハンズオンを行いました。

ここでは、docker-compose.ymlファイルについて詳しく解説します。

###### docker-compose.ymlファイルとは？

docker-compose.ymlファイルとは、Docker Composeによって使用される構成ファイルのことです。

このファイルには、**複数のDockerコンテナを定義し、それらのコンテナの設定を管理するための情報が含まれます。**

このファイルは、YAML(ヤムル)形式で記述されます。拡張子のymlは、YAML形式で記述していることを表しています。

###### docker-compose.ymlファイルの中身

docker-compose.ymlファイルでは、主に以下の内容が記述されます。

- バージョン・・・docker-composeで使用するバージョンを定義
- **サービスの定義・・・各コンテナの設定**
- **環境変数・・・サービスが使用する環境変数の設定**
- **ボリュームの定義・・・コンテナのデータ永続化や共有のためのボリュームの定義**
- **ネットワークの設定・・・ コンテナ間の通信や外部からのアクセスを制御するためのネットワークの設定**

例えば、ハンズオンで記述したdocker-compose.ymlファイルでは、サービスの定義や環境変数が記述されています。

![[Pasted image 20250907004806.png]]

各設定や記述の仕方については、後ほど詳しく解説します！

---
##### YAMLとは？

docker-compose.ymlファイルは、YAML形式で記述されます。

ここでは、YAMLについて詳しく解説します。

###### YAMLとは？

YAML（YAML Ain’t Markup Language）は、人間にも機械にも読みやすいデータ形式の一つです。

YAMLは主に設定ファイルやデータの構造化に使用されます。

YAMLは特定のプログラミング言語に依存しないため、Python、Ruby、JavaScriptなど、さまざまなプログラミング言語で使用できます。

そのため、Docker Composeだけでなく、さまざまな場面でyml形式の記述をすることがあります。

###### YAMLの記述方法

YAMLの記述方法は、キーと値のペア、インデント(空白)、リストなどの概念を使ってデータを表現します。

**1.キーと値**

キーと値はコロン（:）で区切り、インデントします※。値は必要に応じてダブルクオート（””）またはシングルクオート（”）で囲むことができます。

※インデントとは、文章の行頭に空白を挿入して、先頭の文字を右に押しやることです。

```bash
key1: value1
key2: 'value2' 
key3: "value3"
```

**2.インデント**

YAMLでは、インデント(空白)を使用してデータの階層構造を表現します。

スペース2つまたはスペース4つが一般的に使用されます。タブ文字は使用できません。

```bash
key1:
  key2:value
    key3:value
```

**3.リスト**

リストはハイフン（-）で表現され、リストの要素はインデントされます。

```bash
key:
  - item1
  - item2
  - item3
```

**4.複数行の文字列**

複数行の文字列は、パイプ記号（|）または大なり記号（>）を使用して表現されます。

パイプ記号は改行を保持し、大なり記号は改行をスペースに変換します。”This is a ~”の部分が値の部分です。

```bash
key: |
  This is a
  multiline string

key: >
  This is a
  multiline string with
  line breaks converted to spaces
```

このようなYAML形式に沿って、docker-compose.ymlファイルを記述します。

---
##### バージョンの定義

ここからは、docker-compose.ymlファイルの書き方について解説します。

まずは、バージョンの定義からです。

###### バージョンの定義の方法

**docker-compose.ymlファイルの先頭には、バージョンの定義をする必要があります。**

以下のように定義します。

```bash
version: "[バージョン]"
例) version: "3"
例) version: "2.4"
```

現在、Docker Composeでは、バージョンの1~3があります。

バージョン1は非推奨です。**そのため、バージョンは2以上を使用するようにしましょう。**

基本的にバージョン3や最新版を指定しておけば大丈夫です。

どのようなバージョンがあるかについては、公式のドキュメントを確認してください。

[https://docs.docker.jp/compose/compose-file/compose-versioning.html](https://docs.docker.jp/compose/compose-file/compose-versioning.html)

---
##### サービスの定義

サービスは、Dockerコンテナのグループを定義し、それぞれがアプリケーションやインフラとして動作します。

具体的には、アプリケーションを動かすためのWebサーバーやデータベースのコンテナの単位がサービスです。

###### サービスの設定方法

サービスは、docker-compose.ymlのservices:セクションで記述します。

```bash
version: "3"

services:
  [サービス名1]:
     [サービス1の設定]:

  
  [サービス名2]:
     [サービス2の設定]:
```

サービス名は任意の文字列を指定します。

例えば、Webサーバーであれば、webやapp、データベースであれば、dbやmysqlなど分かりやすい名前を指定します。

```bash
version: "3"

services:
  web:
     [webの設定]:

  
  db:
     [dbの設定]:
```

###### サービスの各設定

サービスには、コンテナを動作させるための設定が必要です。

例えば、どのようなイメージをどのようなポートで公開するといった内容です。

これは、docker runコマンドで指定した内容と同じ内容をdocker-compose.ymlでも記述します。

よく使う設定値について解説します。

###### **Dockerイメージの指定**

イメージの指定では、サービスがどのようなDockerイメージを使用するのかを指定します。

例えば、webのサービスでは、nginxのDockerイメージを使用します。

その場合の設定は、以下のようになります。

```bash
version: "3"

services:
  web:
   image: nginx:latest
```

image: ~と指定した場合、Docker Hubから指定したDockerイメージをダウンロードします。

**自分で作成したDockerイメージを使用したい場合は、Dockerfileを作成して以下のように指定します。**
```bash
version: "3"

services:
  web:
　　build:
　　  context:./path/# Dockerfileのあるディレクトリのパスを指定します。
　　  dockerfile:Dockerfile# 使用するDockerfileのファイル名を指定します。
```

上記のように指定すると、./path/Dockerfileをビルドして作成されたイメージを使用します。

**image:かbuild:のどちらかを指定してDockerイメージを指定しましょう。**

###### **ポートの指定**

**ポートの指定は、自身のPCのポートとコンテナのポートのマッピングを定義します。**

このポートのマッピングにより、ホストマシン上のポートとコンテナ内のアプリケーションが通信できるようになります。

設定は、サービス内でports:と指定します。

ポートの指定は、“**自身のPCのポート:コンテナのポート**“と指定します。

( – はYAMLのリスト形式の記述方法です。)

```bash
version: "3"

services:
  web:
   image: nginx:latest
   ports:
    - "80:80"
```

複数のポートを指定することも可能です。

```bash
version: "3"

services:
   web:
     image: nginx:latest
     ports:
      - "80:80"
      - "443:443"
```

###### **ボリュームの設定**

ボリュームの設定は、自身のPC上のディレクトリやファイルをコンテナ内の特定の場所にマウントする場合に使用します。

これにより、データの永続化やファイルの共有が可能になります。

例えば、アプリケーションのソースコードを編集する場合、自身のPCとコンテナのファイルを共有しておくことで、リアルタイムで変更を確認することができます。

![[Pasted image 20250907005512.png]]

ボリュームの設定は、以下のように行います。

自身のPCの/home/userディレクトリをコンテナ内の/var/www/htmlにマウントしています。  
指定の方法volumes:をキーとして、[自身のPCのディレクトリのパス]:[コンテナのディレクトリのパス]と指定します。

```bash
version: "3"

services:
  web:
   image: nginx:latest
   ports:
    - "80:80"
　 volumes:
    - /home/user:/var/www/html
```

volumesでは、ソースコードや設定ファイルなどを共有する際によく使うので覚えておきましょう。

###### 依存するサービスの指定

depends_onキーを使用して、サービス間の依存関係を定義することができます。

依存するサービスが起動するまで、指定したサービスの起動を遅延させることができます。

例えば、Webアプリケーションの場合、Webサーバーを起動する前にデータベースを起動させる必要があります。このような場合に、depends_onキーを指定します。

設定は、以下の通りです。

```bash
version: "3" 

services:
  web: 
   image: nginx:latest
   depends_on:
     - db  #←サービス名を指定

  db:
   image: mysql:5.7
```

上記の例では、webのサービスを起動する前に、dbのサービスを起動させます。

---
##### docker-compose コマンド

ここまで、ハンズオンやdocker-compose.ymlファイルの書き方について解説しました。

ここからは、docker-composeコマンドについて解説します。

###### docker-composeコマンドの使い方

docker-composeコマンドは、サブコマンドと組み合わせて使用します。

```bash
docker-compose [サブコマンド] 
```

ハンズオンでは、docker-compose upなどを使用しました。

docker-composeコマンドを使用すると、docker-compose.ymlファイルに記述しているコンテナのすべてを一度に操作することができます。

特定のコンテナを操作したい場合は、サービス名を指定します。

```bash
docker-compose [サブコマンド] [サービス名]

例) docker-compose up -d web
```

###### サブコマンドの一覧

docker-composeのサブコマンドには、以下のようなものがあります。

|コマンド|説明|
|---|---|
|docker-compose up|コンテナを起動します。|
|docker-compose down|コンテナを停止し、削除します。|
|docker-compose build|コンテナイメージをビルドします。|
|docker-compose start|停止されたコンテナを再起動します。|
|docker-compose stop|実行中のコンテナを停止しますが、コンテナを削除しません。|
|docker-compose restart|実行中のコンテナを再起動します。|
|docker-compose ps|実行中のコンテナの一覧を表示します。|
|docker-compose logs|コンテナのログを表示します。|
|docker-compose exec|実行中のコンテナに対してコマンドを実行します。|

基本的には、dockerコマンドと同じようなものがあります。

すべてを覚える必要はありませんが、どのようなものがあるのかだけ知っておきましょう。