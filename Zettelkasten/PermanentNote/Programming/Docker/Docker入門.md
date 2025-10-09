---
title: Docker入門
AI:
published: 2025-09-04
description:
tags:
  - permanent-note
  - docker
---
**Docker講座**
- https://engineer-ninaritai.com/infraacademy/courses/introduction-docker/

---
##### Dockerの概要

まずは、Dockerの概要について解説します。

Dockerとは、そもそも何なのでしょうか？

###### Dockerとは？

**Dockerとは、コンテナ型の仮想化技術のことです。**

**簡単に言うと、サーバーなどの設定を一つにまとめる技術のことです。**
![[Pasted image 20250904171159.png]]

Dockerを使うことで、簡単にWebサーバーやデータベースなど様々なインフラを構築することができます。

##### 開発現場を想定してみよう

**なぜ、Dockerを使う必要があるのでしょうか？**

Webアプリの開発現場を想像してみましょう。

Webアプリを開発するためには、Webアプリのソースコードや、Webアプリを動作させるWebサーバーなどが必要になります。
![[Pasted image 20250904171219.png]]

これらを開発するときには、自身のPCでソースコードを編集します。

ソースコードを編集したら、その場で確認する必要があり、自身のPCにもWebサーバーなどが必要になります。

しかし、開発者がWindowsやMacなど様々なPCを使用すると、Webサーバーやデータベースなどの設定がバラバラになる場合があります。

バラバラな開発環境で開発した場合、AさんのPCでは動くけど、BさんのPCでは動作しないといった不具合が発生します。
![[Pasted image 20250904171252.png]]

このような不具合をなくすために、インフラをDockerで構築します。

Dockerは、インフラの設定をファイルに記述することで、仮想環境をコマンド1つで作成できます。

すると、開発者全員が同じ環境のインフラで開発を進めることができるのです。
![[Pasted image 20250904171310.png]]

---
##### Dockerの環境準備について

Dockerの環境は、**自分のPCでDockerをインストールするか、ブラウザで動作させる2種類**があります。

Docker学習のために、どちらかの方法でDockerを動かす環境を準備しましょう。

###### Dockerをブラウザで動作させる

Dockerをブラウザで動作させるためには、[シミュレーター](https://terminal.engineer-ninaritai.com/)を起動させましょう。

**シミュレーターを起動させたら、[ + インスタンスを作成]をクリックしましょう。**

![[Pasted image 20250904171355.png]]

インスタンスを起動すると、ターミナルが表示されます。そこで、以下のコマンドを実行しましょう。

```bash
docker -v
```

コマンドを実行すると、dockerのバージョンが表示されます。
![[Pasted image 20250904171406.png]]

これで準備が完了です。

PCにDockerをインストールして動作させたい方は次に解説します。

---
###### PCにDockerをインストールする方法

ここでは、自分のPCで**Dockerを動かすための準備を行います。**

シミュレーターでDockerを動かす方は、[次に進みましょう！](https://engineer-ninaritai.com/infraacademy/courses/introduction-docker/lesson/docker%e3%82%92%e4%bd%bf%e3%81%a3%e3%81%a6%e3%81%bf%e3%82%88%e3%81%86/)

###### 公式ページからダウンロード

**Dockerを自分のPCで動作させるためには、Docker Desktopをインストールします。**

まず、以下のURLからDocker Desktopのサイトへアクセスします。

[https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)  

DockerDesktopのサイトに移動すると、以下のようになっています。
![[Pasted image 20250904171450.png]]

自分の環境に合わせて、Docker Desktopのファイルをダウンロードします。

Macの方の場合は、大きいボタンのintel Chipか左下のApple Chipをクリックします。

Windowsの方の場合は、Windowsをクリックしましょう。

---
##### Dockerを使ってみよう

ここからは、Dockerを実際に使ってみましょう。

前回は、シュミレーターもしくはDockerDesktopを準備してDockerが使えるようになりました。

環境が準備できたら、Dockerを体験してみましょう。

###### WebサーバーをDockerで起動

[シミュレーターはこちら](https://terminal.engineer-ninaritai.com/)

**Dockerを使うことで簡単にWebサーバーが立ち上がります。**

以下のコマンドを入力してください。

```bash
docker run -p 80:80 nginx
```

![[Pasted image 20250904174321.png]]

コマンドを実行したらWebサーバーにアクセスしてみましょう。

**シミュレーターを使っている方**は、OPEN PORTをクリックして、80と入力するとアクセスできます。
![[Pasted image 20250904174331.png]]
![[Pasted image 20250904174341.png]]

自身のPCでDockerを使っている方は、Chromeやsafariなどの**ブラウザで、「http://localhost」にアクセスしてみてください。**

すると、以下のようにnginxのデフォルトのWebページが表示されます。
![[Pasted image 20250904174351.png]]

ターミナルで、**[ctrl] + cキー**を押すとコンテナ起動を終了することができます。

**このようにDockerを使うと、Linuxの設定をせずにWebサーバーを立ち上げることができます。**

Dockerを使わない場合、Linux環境の準備、Webサーバー(Nginx)のインストールなどの設定が必要になります。

Dockerは、煩雑な環境構築の手間を無くしてくれる便利なツールです。

---
##### コンテナを起動するまでの流れ

先ほどは、Dockerを使って、Webサーバーのコンテナを立ち上げました。

ここでは、**コンテナを起動するまでの流れについて**説明します。

Webサーバーのコンテナが立ち上がるまで、以下の2つの流れがあります。

1. **Docker HubからDockerイメージの取得**
2. **Dockerイメージをコンテナとして起動**

さらに詳しく解説します。

###### DockerイメージとDockerHub

**Dockerイメージとは、コンテナの基となるデータのことです。**

例えば、Webサーバーの場合、Webサーバーを動かすために必要なLinuxやNginxのことです。

つまり、**Webサーバーとして動作させるために必要なOSやソフトウェアをまとめたものがDockerイメージとなります。**

ソフトウェアのバージョンや、ソフトウェアを動作させるためのコマンドなどもセットになっています。
![[Pasted image 20250904191027.png]]

**このDockerイメージが保存されている場所がDockerHubです。**

↓Docker Hubはこちら(興味のある方はご覧ください)

[https://hub.docker.com/](https://hub.docker.com/)

Docker Hubには、さまざまなイメージが保存されています。

前回使用したNginxのイメージもあります。
![[Pasted image 20250904192011.png]]

###### Dockerイメージからコンテナを起動するまでの流れ

次に、**取得したDockerイメージを起動することで、Dockerコンテナとしてサーバーの環境が完成します。**
![[Pasted image 20250904192028.png]]

前回入力したdocker runコマンドは、「2.起動」にあたります。

nginxを起動する際は、以下のようにコマンドを実行していました。

```bash
docker run -p 80:80 nginx
```

まず、docker runで指定した、nginxという名前のDockerイメージを取得します。  
ローカルPCに保存されていない場合は、Docker Hubからイメージを取得します。

イメージが取得できたら、イメージを起動させます。  
今回、”-p 80:80″はDockerコンテナとPCのポート80番を繋いでいます。  
ポート番号を繋ぐことで、外部からコンテナへのアクセスが可能になります。

このようにコンテナが起動します。

---
##### Dockerfileを作成してみよう

前回まで、Dockerイメージからコンテナを起動する流れについて説明しました。

そこでは**DockerイメージはDockerHubからダウンロードする方法と、自分で作成する方法**があります。

今回はDockerイメージを作成する方法について解説します。

### Dockerfileとは？

Dockerイメージを作成するためには、**Dockerfileを作成する必要**があります。

**Dockerfileは、どのようなDockerイメージを作成するのかを記述したファイルになります。**

つまり、DockerfileからDockerイメージが作られるのです。
![[Pasted image 20250904192125.png]]
###### Dockerfileを作成する

ここからは、実践です。

**Dockerfileを作成して、Dockerイメージを作成してみましょう。**

今回は、Linux(Ubuntu)にWebサーバーをインストールしたイメージを作成します。

viコマンドを使って、Dockerfileという名前のファイルを編集します。

```bash
vi Dockerfile
```

ファイルには、以下の内容を記述しましょう。

```bash
FROM ubuntu

RUN apt-get update -y && \
apt-get install -y tzdata && \ 
apt-get install -y apache2

EXPOSE 80

CMD ["apachectl", "-D", "FOREGROUND"] 
```

catコマンドでファイルの中身を確認すると、以下のようになっています。
![[Pasted image 20250904192934.png]]

「FROM」では、DockerHubのイメージを指定します。Linuxなど既に準備されているイメージを指定します。

DockerHubからダウンロードしたイメージに、「RUN」をつかって、apache2のパッケージをインストールしています。

EXPOSEを使って、コンテナの80番ポートを開けるようにしています。
![[Pasted image 20250904192945.png]]

Dockerfileの書き方の詳細については、後ほど詳しく解説します。

次に**このDockerfileをビルドしてDockerイメージを作成します。**

---
##### DockerfileをビルドしてDockerイメージを作る

前回はDockerfileを作成しました。

今回はDockerfileをビルドしてDockerイメージを作成します。

###### Dockerfileのビルド

以下のコマンドを実行します。

```bash
docker build --tag web:1.0 .
```

コマンドを実行すると、以下のようにビルドが始まります。
![[Pasted image 20250904193034.png]]

###### docker buildコマンドの書式

docker buildコマンドの書式は以下の通りです。

```bash
docker build [オプション] [Dockerfile のパスまたは URL]
```

docker build は、Dockerfileからイメージをビルドするコマンドです。

–tag または -t オプションを使用して、イメージにタグを付けることができます。

上記の例では、web:1.0 というタグが付けられます。webはイメージの名前で、 1.0はバージョン番号です。

タグは後ほど、コンテナを起動するときに使います。

末尾の”.” は、Dockerfileがあるディレクトリを指します。

つまり、カレントディレクトリにある Dockerfile から web:1.0というタグを持つイメージを作成しています。

![[Pasted image 20250906113730.png]]

次に、ビルドして作成したイメージをコンテナとして起動します。

---
##### Dockerイメージをコンテナとして起動する

###### docker runコマンド

イメージをコンテナとして起動する場合、docker runコマンドを使います。

以下のコマンドを入力します。

```bash
docker run -p 80:80 -d web:1.0
```

コマンドを実行したら、ブラウザでWebサーバーが立ち上がっているのか確認してみましょう。

[OPEN PORT]から”80″と入力してみましょう。

以下のように、Apache2のデフォルトページが表示されます。(nginxのページが表示される方は、画面を更新しましょう。)

![[Pasted image 20250906113846.png]]

これでコンテナ起動しているWebサーバーにアクセスすることができました。

###### コマンドの説明

Dockerイメージをコンテナとして起動させるために、docker runコマンドを使用しました。

このコマンドの書式は以下の通りです。

```bash
docker run [オプション] [イメージ名]
例)docker run -p 80:80 -d web:1.0
```

**“-p”オプション**で、コンテナの80番ポートとローカルの80番ポートの紐づけをしております。

つまり、ローカルの80番ポートにアクセスすると、コンテナの80番ポートにアクセスされます。

**“-d”オプション**は、デタッチドモードでコンテナを起動します。

バックグラウンドで起動するようになります。

コマンドを実行した後も、コンテナが裏で動いている状況です。

**web:1.0**はDockerイメージのタグです。docker buildコマンドのときに付けたタグ名を合わせます。


> [!NOTE] 補足
> -d オプションをつけないとフォアグラウンドモードとなる。
> → サーバーのログがターミナルに出続ける状態。Ctrl + C でコンテナごと終了する。
> -d オプションを付けた場合にログを見る場合は、
> `docker logs <コンテナ名 or ID>` でOK

---
##### 動作しているコンテナの確認

###### docker psコマンド

dockerコンテナが起動しているかどうかは、「**docker ps**」コマンドを使用します。

以下のコマンドを実行してみましょう。

```bash
docker ps
```

実行結果は以下の通りです。
![[Pasted image 20250906115603.png]]

docker psコマンドで、前回起動したコンテナが確認できます。

各項目には、以下のような意味があります。

| 列名           | 説明                                |
| ------------ | --------------------------------- |
| CONTAINER ID | コンテナの一意の識別子                       |
| IMAGE        | コンテナが基づくDockerイメージの名前             |
| COMMAND      | コンテナが実行しているコマンド                   |
| CREATED      | コンテナが作成された日時                      |
| STATUS       | コンテナの状態（実行中、停止中など）                |
| PORTS        | ホストとコンテナの間でポートがマッピングされている場合のポート情報 |
| NAMES        | コンテナに割り当てられた名前                    |

---
##### 動作しているコンテナ内の動作

続いて、動作しているコンテナ内の操作方法です。

###### docker execコマンド

以下のコマンドを実行すると、コンテナ内に入ります。

```bash
docker exec -it [コンテナ名] /bin/bash
```

コンテナ名はdocker psコマンドで確認することができます。

コマンドを入力すると、コンテナ内に入ることができます。
![[Pasted image 20250906115849.png]]

例えば、コンテナ内で以下のコマンドを実行します。

```bash
echo "test server" > /var/www/html/index.html
```

コマンド実行後、再度Apacheのページを確認してみましょう。  
([OPEN PORT]から80と入力)  
すると、”test server”というページが表示されます。

これは、コンテナ内でWebページを編集しています。

コンテナから抜ける際は、[ctrl] + dキーを押すか、”exit”と入力します。

コンテナはイメージから起動されるため、再度コンテナを起動すると、今回の変更は反映されません。

###### docker execコマンドの解説

docker execコマンドは、実行中の Docker コンテナ内でコマンドを実行するためのコマンドです。

このコマンドを使用すると、Docker コンテナ内でシェルを起動したり、特定のコマンドを実行したりできます。

使い方は以下の通りです。

```bash
docker exec [オプション] [コンテナ名] [コマンド]
```

docker execコマンドのオプションは以下の通りです。

|オプション|説明|
|---|---|
|-d, –detach|バックグラウンドでコマンドを実行します。|
|-i, –interactive|対話的なモードでコマンドを実行します。|
|-t, –tty|tty を割り当ててコンテナ内での対話的なセッションを確立します。|
|–user|コマンドを実行するユーザーを指定します。|
|–privileged|特権モードでコマンドを実行します。|
|–env|環境変数を指定します。|
|–workdir|コマンドを実行する作業ディレクトリを指定します。|

Docker コンテナ内に入るには、-iと-tオプションを使用して対話的なモードと tty を割り当てる必要があります。

コマンドは、/bin/bashを指定してシェルを起動させています。

また、lsコマンドなど普通のコマンドも実行できます。

```bash
docker exec [コンテナ名] ls -l /etc
```

コマンドを実行すると、以下のように表示されます。
![[Pasted image 20250906121014.png]]

---
###### 動作しているコンテナのログを確認

続いて、動作しているコンテナのログを確認してみましょう。

###### docker logsコマンド

コンテナのログを確認する場合は、以下のコマンドを実行します。

```bash
docker logs [コンテナ名]
```

docker logsコマンドを使用すると、コンテナ内のログが表示されます。

コンテナ内に入って、ログを探す必要がなくなります。
![[Pasted image 20250906121103.png]]

また、tail -fコマンドのように、ログを出し続けたい場合は、”-f”オプションを使用します。

```bash
docker logs -f [コンテナ名]
```

ログの表示を止めたい場合は、[ctrl] + cキーを押します。

---
##### Dockerイメージをダウンロードする

まず、「docker pull」コマンドについて解説します。

**docker pullコマンドを使用することで、DockerHubにあるイメージをダウンロードすることができます。**

###### docker pullコマンド

コマンドは以下のように使用します。

```bash
docker pull centos
```

コマンドを実行すると、DockerHubからイメージのダウンロードを行います。
![[Pasted image 20250906121307.png]]

###### docker pullコマンドの使い方

docker pullコマンドの書式は以下の通りです。

```bash
docker pull [イメージ名:タグ]
```

タグの部分は、イメージのバージョンを表します。

特定のバージョンのイメージを取得したい場合は、以下のように指定します。

```bash
docker pull centos:8.4.2105
```

バージョンを指定しない場合は、latest(最新版)をダウンロードします。

タグについては、[Docker Hub](https://hub.docker.com/_/centos/tags)でイメージを検索し、tagの部分に記載されています。
![[Pasted image 20250906121405.png]]


---
##### Dockerイメージの一覧

前回は、docker pullコマンドを使って、イメージをダウンロードしました。

ダウンロードしたイメージは、docker imagesコマンドで確認することができます。

###### docker imagesコマンド

docker imagesコマンドは以下のように使用します。

```bash
docker images
```

コマンドを実行すると、以下のように先ほどダウンロードしたイメージが表示されます。
![[Pasted image 20250906121552.png]]

また、イメージ名を指定することも可能です。

```bash
docker images [イメージ名]
例) docker images centos
```

特定のイメージだけ表示させたい場合に使います。
![[Pasted image 20250906121601.png]]

---
##### Dockerイメージへのタグ付け

docker tagコマンドは、Dockerイメージに新しいタグを付けるために使用されます。

###### docker tagコマンドの使い方

docker tagコマンドを使ってみましょう。

centos(:latest)に”new_tag”というタグをつけてみます。

以下のようにコマンドを実行してみましょう。

```bash
docker tag centos centos:new_tag
```

コマンドを実行したら、docker imagesコマンドでイメージの一覧を見てみましょう。

```bash
docker images
```

コマンドを実行すると、new_tagというタグが付いているのがわかります。
![[Pasted image 20250906122005.png]]

docker tagコマンドの書式は以下の通りです。

```bash
docker tag [元のイメージ:タグ] [新しいイメージ名:タグ]
```

例えば、centosというイメージ名を新しく”hogehoge”という名前にすることもできます。

```bash
docker tag centos hogehoge
```

docker tagコマンドはイメージの管理やバージョン管理で使用します。

---
##### コンテナの削除

続いて、コンテナを削除するコマンドです。

コンテナを削除する際は、docker rmコマンドを使用します。

###### コンテナを削除してみよう

コンテナの削除をするために、まずコンテナを起動させます。

```bash
docker run -d -p 80:80 nginx
```

docker psコマンドでコンテナが起動しているか確認しましょう。

```bash
docker ps
```

![[Pasted image 20250906122348.png]]

コンテナの起動が確認できたら、削除をしてみます。

起動中のコンテナは削除できないので一旦停止させます。

[コンテナIDもしくはコンテナ名]はdocker psコマンドのCONTAINER IDかNAMEを指定します。

```bash
docker stop [コンテナIDもしくはコンテナ名]
例)docker stop 199a321f409d
```

停止されたかどうかは、docker psコマンドに-aオプションを付けると確認できます。

```bash
docker ps -a
```

コマンドを実行すると、以下のように表示されます。

STATUSがExitになっていることがわかります。
![[Pasted image 20250906122446.png]]

このコンテナを削除する場合は、docker rmコマンドを使います。

```bash
docker rm [コンテナIDもしくはコンテナ名]
```

再度、確認するとコンテナが削除されていることがわかります。

```bash
docker ps -a
```

また、コンテナ起動中に強制的に削除する場合は、”-f”オプションを使います。

```bash
docke rm -f [コンテナIDもしくはコンテナ名]
```

---
##### イメージの削除

先ほどは、コンテナの削除を行いました。

次はイメージの削除です。イメージの削除はdocker rmiコマンドを使用します。

###### docker rmiコマンド

docker rmiコマンドは以下のように使用します。

```bash
docker rmi [イメージ名]
```

例えば、centosというイメージを削除する場合は、以下のように実行します。

```bash
docker rmi centos
```

このようにイメージの削除を行います。

---
##### Dockerfileの書き方

ここまでは、Dockerのコマンドについて解説しました。

ここからは、Dockerfileの書き方について解説します。

###### Dockerfileの基本的な書き方

Dockerfileとは、Dockerのイメージのもととなるファイルです。

前回のハンズオンでは、以下のようなDockerfileを記述しました。

```bash
FROM ubuntu

RUN apt-get update -y && \
apt-get install -y tzdata && \ 
apt-get install -y apache2

EXPOSE 80

CMD ["apachectl", "-D", "FOREGROUND"] 
```

Dockerfileには、以下のように記述します。

```bash
[命令] [引数]
```

例えば、1行目のFROM ubuntuは、FROMが[命令]で、ubuntuが[引数]です。

また、#を先頭につけることで、コメントにすることが可能です。

###### 上記のDockerfileの解説

では、上記のDockerfileを一行ずつ解説してみます。

```bash
FROM ubuntu
```

FROM ubuntuは、ベースイメージとして公式のUbuntuイメージを使用することを示します。Dockerイメージをビルドする際に、Ubuntuイメージがベースとなります。

```bash
RUN apt-get update -y && \
apt-get install -y tzdata && \ 
apt-get install -y apache2
```

RUNコマンドは、ベースイメージ内でコマンドを実行します。

上記の例では、ubuntuイメージ内に必要なパッケージやツールをインストールします。

&&は、コマンドが成功したら、次のコマンドを実行することを意味しています。

また、 \(バックスラッシュ)は、コマンドが続いているけど改行する場合に使用します。

```bash
EXPOSE 80
```

 EXPOSEは、コンテナ内のポート80を外部に公開することを示します。

```bash
CMD ["apachectl", "-D", "FOREGROUND"] 
```

CMDは、コンテナが起動されたときに実行されるデフォルトのコマンドを指定します。  
ここでは、apachectl -D FOREGROUNDが実行され、Apacheサーバーがバックグラウンドで動作する代わりに、フォアグラウンドで実行されます。

これにより、コンテナが実行されているかどうかを確認しやすくなります。

このような命令を設定してDockerイメージのカスタマイズを行います。

---
##### Dockerfileの命令一覧

Dockerfileに記述できる命令の一覧です。

覚える必要はありませんが、参考程度にお読みください。

| 命令         | 説明                                                                 |
| ---------- | ------------------------------------------------------------------ |
| FROM       | ベースイメージを指定します。                                                     |
| RUN        | コマンドを実行し、新しいイメージレイヤーを作成します。                                        |
| COPY       | ローカルファイルやディレクトリをコンテナ内にコピーします。                                      |
| ADD        | COPYと同様にローカルファイルやディレクトリをコピーしますが、URLからのファイルのダウンロードやtarファイルの解凍も行います。 |
| WORKDIR    | 作業ディレクトリを指定します。                                                    |
| CMD        | コンテナが実行されたときに実行されるデフォルトのコマンドまたは引数を指定します。                           |
| ENTRYPOINT | コンテナが実行されたときに実行されるコマンドを指定します。CMDの引数はENTRYPOINTの後に追加されます。           |
| ENV        | 環境変数を設定します。                                                        |
| EXPOSE     | コンテナが使用するポートをドキュメント化します。実際にホストにポートを公開するには-pフラグを使用します。              |
| VOLUME     | マウントポイントとして使用するディレクトリを指定します。                                       |
| ARG        | ビルド時に指定される変数を定義します。                                                |
| LABEL      | イメージにメタデータを追加します。                                                  |
| USER       | コンテナ内で実行される命令のデフォルトのユーザーを設定します。                                    |

