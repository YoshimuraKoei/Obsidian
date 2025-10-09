---
title: "スマホのObsidianをGitで同期(2024.11)"
source: "https://zenn.dev/ishikawa096/articles/158246fc5a5d62"
author:
  - "Zenn"
published: 2024-11-10
created: 2025-05-11
description:
tags:
  - "clippings"
---
48

14[tech](https://zenn.dev/tech-or-idea)

## 背景

Obsidianのコミュニティプラグイン「Git [^1] 」を利用することで無料でデバイス間データ同期ができる。  
しかしPC側は設定できたものの、スマホ側の設定で少し手こずったので記録。

検索すると「Working Copy」を使ってモバイル端末上でリポジトリを管理する方法がいくつか出てきたが、現在(2024年11月)は不要だった。

## 手順

先に完了させておくこと

- PC側のObsidianのインストール
- PC側のObsidianでコミュニティプラグイン「Git」のインストール
- Obsidian同期用のgitリポジトリの作成

### GithubでPersonal access tokenを発行する

モバイル端末からリポジトリにアクセスするためのPersonal access tokenを発行する。

1. [github](https://github.com/) にログイン
2. 自分のアイコンをクリックし、メニューから「Settings」を選択
3. 下にスクロールし、設定項目の一番下にある「Developer Settings」を選択
4. 「🔑Personal access tokens」をクリック、「Fine-grained tokens」を選択
1. Generate new tokenボタンをクリック
2. token nameなどを適当に入力（例: obsidian-git-for-iPhone)
3. Repository accessで、Only select repositoriesにチェックし、事前に作成したObsidian同期用のgitリポジトリを選択  
	![Repository access設定](https://storage.googleapis.com/zenn-user-upload/d6d8dd333edb-20241109.png)
4. Permissionsで、Repository permissionsの中から「Contents」を「Access: Read and write」に変更  
	![](https://storage.googleapis.com/zenn-user-upload/eaf79e9e7522-20241109.png)
5. 間違いないか確認し、Generate token
6. tokenをコピーしてローカルに保管。  
	※tokenは再表示できないので、スマホへの設定が完了する前に紛失した場合は再度作成する。

### スマホ側でインストール

1. AppストアからObsidianアプリをインストール
2. Create new vault
	- Vault nameは適当に入力。
	- 今回はgitで同期するため、「Store in iCloud」はOFFにする。
3. 左上のサイドメニューアイコンをタップ
4. 左上の設定アイコン⚙️をタップ
5. Optionsの中から「Community plugins」をタップ
6. 下にスクロールし「Turn on community plugins」ボタンをタップ
7. Community pluginsの「Browse」ボタンをタップ
8. 検索欄に「git」と入力し、「Git」プラグインを選択、installボタンをタップ
9. インストール後、Enableボタンをタップ。出てきたOptionボタンをタップ。
10. スクロールし、Authentication/commit authorセクションで、Github usernameと先ほど生成したpersonal access tokenを入力。
11. ←ボタンで戻り、xでSettingsを閉じ、右から左にスワイプしてサイドメニューを閉じる。

### スマホへGit Clone

1. No file is openと表示されている画面で、右下のハンバーガーメニューアイコン(≡)をタップ。
2. 「 `Open command palette` 」をタップ。
3. 検索欄に「git clone」と入力して、 `Git: Clone an existing remote repo` コマンドを選択する
4. Enter remote URL欄にリポジトリのHTTPS git URLを入力しdone  
	※githubで該当のリポジトリ画面を開いて「Code」ボタンクリック→ 「HTTPS」を選択して出てくるURL
5. Enter directory for clone, It needs to be...欄が出てくる。  
	新規Vaultの場合特に必要ないためそのままdoneを押すか、Vault rootをタップ。
6. Dose your remote repo contain a.obsidian...欄が出てくる。  
	リポジトリに.obsidianディレクトリをpush済みであれば `YES` を選択。
7. YESを選択すると、iPhone側の設定を削除してリポジトリの設定で上書きするか、cloneを中止するかの選択肢が出る。  
	上書きで問題なければ「 `DELETE ALL YOUR LOCAL CONFIG AND PLUGINS` 」をタップ。
8. Specify depth of clone. Leave empty for fullはそのままdone。

左サイドメニューを開き、リポジトリ上のファイルが表示されるようになればclone成功。

### スマホから同期する

git author情報を入力しないとgit pushがエラーとなるため設定を追加する。

1. 一度obsidianアプリを閉じて再度開く。
2. 左サイドメニューを開き、設定アイコンをタップ。
3. 下にスクロールし最下部にあるCommunity pluginsセクションの「Git」をタップ。
4. スクロールすると、Authentication/commit authorセクションに「 `Author name for commit` 」欄と「 `Author email for commit` 」欄が出現しているので入力する。
	- 適当に入力しても同期できるが、emailがgithubアカウントのものと異なる場合、commitがgithubアカウントに紐付かなくなるので注意。

右から左にスワイプするとsource controlメニューを表示できる。  
適当にメモを編集したら、source controlの➕アイコンで全ての変更をstage、✔️アイコンでcommit、↑ボタンで手動でpushできる。

もしgit author情報が登録されていない場合、「Git author information is not set.」のメッセージが出るので、設定漏れがないか確認しよう。  
githubリポジトリを確認して、問題なくpushされていれば同期成功。

## 関連リンク

[Obsidian](https://obsidian.md/)  
[obsidian-git](https://github.com/Vinzent03/obsidian-git)  
[Github Docs: Managing your personal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)

脚注

48

14

### Discussion

![](https://static.zenn.studio/images/drawing/discussion.png)

ログインするとコメントできます

[^1]: プラグイン名が「Obsidian-Git」となっている記事もある、2024年11月時点では「Git」の模様。