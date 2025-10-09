---
title: "Git LFS をちょっと詳しく"
source: "https://qiita.com/ikmski/items/5cc8b8832336b8d85429"
author:
  - "qiita"
published: 2018-12-20
created: 2025-05-23
description: "Git LFS の機能が実際にどういう感じで動作しているかを、基本的な Git の手順に沿って少しだけ詳しく調べてみました。なお、ロック機能については検証していません orz（今後に期待）TL…"
tags:
  - "clippings"
---
More than 5 years have passed since last update.

Git LFS の機能が実際にどういう感じで動作しているかを、基本的な Git の手順に沿って少しだけ詳しく調べてみました。

なお、ロック機能については検証していません orz  
（今後に期待）

## TL;DR

- ワークツリーの情報をリポジトリ（`.git/` ）に格納するタイミング（clean filter）で対象のファイルがメタ情報（ポインタ）に置き換えられ、ファイルの実体（オブジェクト）は `.git/lfs/` 以下に格納される
- push の直前に LFS API を通してオブジェクトがサーバーにアップロードされる
- リポジトリ（`.git/` ）からワークツリーに展開するタイミング（smudge filter）でメタ情報から実体ファイルに置き換えられる
- LFS オブジェクトが `.git/lfs/` 以下にない場合は LFS API を通してサーバーからダウンロードされる
- コミット時、マージ時、チェックアウト時にそれぞれロック状態をチェックし、その後の処理をどうするかが判断される

## Git LFS とは

Git はファイルの変更差分ではなく、その時点のスナップショットをとることでバージョンの管理を行うため、動画やグラフィックス用の大きなファイルを扱うには不向きです。

そこで GitHub が中心となって大きなファイルを扱うための拡張機能を作成しました。  
Git LFS は、それらのファイルのメタ情報だけを Git で管理し、ファイルの実体はリモートサーバーで一元管理する仕組みを提供しています。

ゲーム開発などの現場では、ソースコードは Git で管理し、アセットデータは Subversion で管理する、など、複数のバージョン管理システムを利用していることもあると思います。  
このような場合、ソースコードとアセットで管理が別になることで、手間が増えたり管理が煩雑になったりすることもあります。

Git LFS を使うことで、プロジェクトのバージョン管理を Git に統合でき、ソースコードもアセットデータも含めたシステム全体を、例えば GitHub を中心としたエコシステムのワークフローに乗せることができるかもしれません。

## クライアントの動作

## Git LFS の初期化

Git と Git LFS はインストール済みとします。  
以下のバージョンで確認しました。

```bash
$ git version
git version 2.20.0

$ git lfs version
git-lfs/2.6.1 (GitHub; darwin amd64; go 1.11.2)
```

まずはじめに Git のローカルリポジトリを作成します。

```bash
$ git init
Initialized empty Git repository in /path/to/git/repo/.git/
```

この時点でのリポジトリの構成を見てみます。

```bash
$ tree -a
.
└── .git
    ├── HEAD
    ├── config
    ├── description
    ├── hooks
    │   ├── applypatch-msg.sample
    │   ├── commit-msg.sample
    │   ├── fsmonitor-watchman.sample
    │   ├── post-update.sample
    │   ├── pre-applypatch.sample
    │   ├── pre-commit.sample
    │   ├── pre-push.sample
    │   ├── pre-rebase.sample
    │   ├── pre-receive.sample
    │   ├── prepare-commit-msg.sample
    │   └── update.sample
    ├── info
    │   └── exclude
    ├── objects
    │   ├── info
    │   └── pack
    └── refs
        ├── heads
        └── tags
```

ほとんどが空のディレクトリで、 `hooks/` にいくつかのサンプルスクリプトがあるだけです。

次に、LFS を初期化します。  
初期化するには `git lfs install` コマンドを実行します。

```bash
$ git lfs install
Updated git hooks.
Git LFS initialized.
```

では、リポジトリがどう変わったかを見てみます。

```bash
$ tree -a
.
└── .git
    ├── HEAD
    ├── config
    ├── description
    ├── hooks
    │   ├── applypatch-msg.sample
    │   ├── commit-msg.sample
    │   ├── fsmonitor-watchman.sample
    │   ├── post-checkout                // <- 追加された
    │   ├── post-commit                  // <- 追加された
    │   ├── post-merge                   // <- 追加された
    │   ├── post-update.sample
    │   ├── pre-applypatch.sample
    │   ├── pre-commit.sample
    │   ├── pre-push                     // <- 追加された
    │   ├── pre-push.sample
    │   ├── pre-rebase.sample
    │   ├── pre-receive.sample
    │   ├── prepare-commit-msg.sample
    │   └── update.sample
    ├── info
    │   └── exclude
    ├── objects
    │   ├── info
    │   └── pack
    └── refs
        ├── heads
        └── tags
```

以下のフックスクリプト [^1] が追加されました。

- post-checkout
- post-commit
- post-merge
- pre-push

試しに `post-checkout` の中身をチェックしてみます。

```bash
$ cat .git/hooks/post-checkout
# !/bin/sh
command -v git-lfs >/dev/null 2>&1 || { echo >&2 "\nThis repository is configured for Git LFS but 'git-lfs' was not found on your path. If you no longer wish to use Git LFS, remove this hook by deleting .git/hooks/post-checkout.\n"; exit 2; }
git lfs post-checkout "$@"
```

LFS コマンドのチェックをして、コマンドがあれば `git lfs post-checkout` コマンドを実行するようになっています。  
他の３つのスクリプトに関しても同様に、対応する `git lfs` コマンドを実行するスクリプトになっています。

また、ここで `git config` を確認すると、以下のフィルタが追加されていることがわかります。  
これは、ユーザの `.gitconfig` に追記されています。

```bash
$ git config -l
~
filter.lfs.clean=git-lfs clean -- %f
filter.lfs.smudge=git-lfs smudge -- %f
filter.lfs.process=git-lfs filter-process
filter.lfs.required=true
~

// "%f" は実行時に対象のファイルパスに置き換えられる
```

つまり、

- `git add` 時に clean フィルタで `git lfs clean` が実行される
- `git checkout` 時に smudge フィルタで `git lfs smudge` が実行される
- `git checkout` の直後に post-checkout フックで `git lfs post-checkout` が実行される
- `git commit` の直後に post-commit フックで `git lfs post-commit` が実行される
- `git push` の直前に pre-push フックで `git lfs pre-push` が実行される
- `git merge` の直後に post-merge フックで `git lfs post-merge` が実行される

ことになります。

最後に、実際に画像ファイルを LFS で管理する下準備として、ファイルの拡張子を登録します。

```bash
$ git lfs track "*.jpg"
Tracking "*.jpg"
```

`.gitattributes` ファイルが作成され、以下のような属性 [^2] が追加されます。

```bash
$ cat .gitattributes
*.jpg filter=lfs diff=lfs merge=lfs -text
```

これで LFS を使う準備が整いました。

## ファイルを追加する

画像ファイルを追加してみます。

```bash
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

    sample01.jpg

nothing added to commit but untracked files present (use "git add" to track)
```

`sample01.jpg` という画像ファイルをワークツリーに追加しました。  
この時点ではリポジトリ（`.git/` ）に変更はありません。

`git add` を実行して追加したファイルをステージングにあげてみます。

```bash
$ git add sample01.jpg
```

見た目は何も変わりません。  
リポジトリの状態を見てみます。（一部省略して表示しています）

```bash
$ tree -a
.
├── .git
│   ├── COMMIT_EDITMSG
│   ├── lfs
│   │   ├── objects
│   │   │   └── 44
│   │   │       └── a8
│   │   │           └── 44a8486abdf0330a3fe6b586e407506e738fe075fe0b5dfc43961e358fda7206
│   │   └── tmp
│   ├── logs
│   │   ├── HEAD
│   │   └── refs
│   │       └── heads
│   │           └── master
│   ├── objects
│   │   ├── 89
│   │       └── 2dbc51ea44a89f4619397b3d0ce8d939504901
│   └── refs
├── .gitattributes
├── README.md
└── sample01.jpg
```

`.git/lfs/` ディレクトリが作成され、その中にオブジェクトが格納されています。  
このファイルが画像ファイルの実体になります。

また、`.git/objects/` 以下に新しく作成されたファイル（89/2dbc...）を見ると先程の `.git/lfs/` 以下のオブジェクトファイルのハッシュ値が格納されていることがわかります。

```bash
$ git cat-file -p 892dbc
version https://git-lfs.github.com/spec/v1
oid sha256:44a8486abdf0330a3fe6b586e407506e738fe075fe0b5dfc43961e358fda7206
size 2684586
```

つまり、clean フィルタで実行された `git lfs clean` によって、ファイルの実体が `.git/lfs/` 以下に格納され、Git で管理されるファイルにはそのオブジェクトハッシュとファイルサイズの情報に置き換えられたことになります。

では、そのままコミットしてみます。

```bash
$ git commit -m "add sample file"
[master 67712b1] add sample file
 1 file changed, 3 insertions(+)
 create mode 100644 sample01.jpg
```

ここで `post-commit` はファイルのロック状態をチェックしています。今はロックの設定をおこなっていないので、そのままスルーしてコミットが完了しました。

## プッシュする

```bash
$ git push -u origin master
origin git@github.com:ikmski/git-lfs-test.git
Uploading LFS objects: 100% (1/1), 2.7 MB | 255 KB/s, done
Enumerating objects: 9, done.
Counting objects: 100% (9/9), done.
Delta compression using up to 4 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (9/9), 821 bytes | 821.00 KiB/s, done.
Total 9 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), done.
To github.com:ikmski/git-lfs-test.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

`pre-push` フックでリモートサーバーに Git LFS API を使ってオブジェクトをアップロードされました。

Git LFS API については後述します。

## リモートから取得する

新しいディレクトリを作成、Git を初期化し、先程の GitHub リポジトリをリモートに登録します。

```bash
$ git init
$ git lfs install
$ git remote add origin git@github.com:ikmski/git-lfs-test.git

$ git fetch
remote: Enumerating objects: 9, done.
remote: Counting objects: 100% (9/9), done.
remote: Compressing objects: 100% (5/5), done.
Unpacking objects: 100% (9/9), done.
remote: Total 9 (delta 1), reused 9 (delta 1), pack-reused 0
From github.com:ikmski/git-lfs-test
 * [new branch]      master     -> origin/master
```

リポジトリ内を確認してみます。

```bash
$ tree -a
.
└── .git
    ├── FETCH_HEAD
    ├── HEAD
    ├── config
    ├── description
    ├── hooks
    ├── info
    │   └── exclude
    ├── logs
    │   └── refs
    │       └── remotes
    │           └── origin
    │               └── master
    ├── objects
    └── refs
        ├── heads
        ├── remotes
        │   └── origin
        │       └── master
        └── tags
```

この時点では LFS で管理しているファイルは同期されていません。

では `origin/master` をマージしてみます。

```bash
$ git merge origin/master
```

標準出力には特に何も表示されませんでしたが、 `tcpdump` などで見てみると、

1. smudge フィルタにより `git lfs smudge` が実行される
2. 画像ファイルの実体がダウンロードされ、`.git/lfs/` 以下に格納される
3. `sample01.jpg` ファイルとして作業ディレクトリにコピー

という処理が行われているようです。  
その後、post-merge フックスクリプトでファイルのロック状態をチェックしています。

git checkout 時も同じように、smudge フィルタにより、Git で管理しているポインタファイルをもとに、作業ディレクトリに `.git/lfs/` 以下の実体のファイルをコピーします。

また、post-checkout フックでもファイルのロック状態をチェックしています。

## Git LFS API

Git LFS API には

- Batch API
- File Locking API

があります。

## Batch API

batch API は LFS オブジェクトの転送指示を要求する API で、  
clean フィルタで変換したオブジェクトのメタ情報（oid と size）を JSON で POST すると  
実体のファイルのアップロード/ダウンロード用の URL を返します。

クライアントは、その URL に対してオブジェクトをアップロード/ダウンロードします。

## File Locking API

File Locking API は Git LFS の v2.0 で追加されました。  
LFS オブジェクトに対してロックを掛けたり解除したりする機能を提供します。

クライアントは各フックスクリプトでロックの状態を確認し、その後の処理を実行するかを判断します。

## 参考資料

- [https://git-scm.com/](https://git-scm.com/)
- [https://git-lfs.github.com/](https://git-lfs.github.com/)

Register as a new user and use Qiita more conveniently

1. You get articles that match your needs
2. You can efficiently read back useful information
3. You can use dark theme
[What you can do with signing up](https://help.qiita.com/ja/articles/qiita-login-user)

[Sign up](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fikmski%2Fitems%2F5cc8b8832336b8d85429&realm=qiita) [Login](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fikmski%2Fitems%2F5cc8b8832336b8d85429&realm=qiita)

[^1]: `.git/hooks/` ディレクトリにスクリプトを置くことで、特定のアクションが発生したタイミングでそのスクリプトを実行することができる。

[^2]: 個別のファイルやパスに対して属性（attribute）の設定を追加することで、そのファイルに対して個別の処理を行うことができます。