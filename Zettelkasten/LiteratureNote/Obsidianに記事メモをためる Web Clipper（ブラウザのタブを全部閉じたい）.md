---
title: Obsidianに記事メモをためる Web Clipper（ブラウザのタブを全部閉じたい）
source: https://zenn.dev/sh11235/articles/07bb24f98b93e7
author:
  - Zenn
published: 2024-11-16
created: 2025-05-09
description: 
tags:
  - Obsidian
  - Cursor
  - Zenn
---
15

3[idea](https://zenn.dev/tech-or-idea)

## 背景

ZennやX（旧Twitter）、Qiitaで話題の記事について、気になったタイトルはとりあえず開いてタブにため込む習性がある。  
読み終えて頭に入れるまでは置いておきたいと思うとなかなか閉じられないし、ブックマークやリーディングリストに放り込むのも何だかなぁと。  
メモはMarkdownで書くのが好みだが、Obsidianというマークダウン管理ツールがあるらしく、これにWeb Clipperというブラウザ拡張があるのを知った。  
Webページをマークダウンファイルとして取り込んでくれるものである。これを使ってlocalに取り込めば後腐れなく（？）ブラウザのタブを閉じられるなと思い、Obsidianを使ってみることにした。  
副次的に、記事のメモが検索可能な形で保存されていくので、自分が読んだ記事専用の全文検索が出来るようになる。別の案として記事のメモを貯める目的では他にNotionのDBなどでやろうかと思ったが、ObsidianのWeb Clipperがとりあえずブラウザの開きっぱなしのタブを閉じる目的には合致した [^1] 。

2025/05/04追記: 今時はMCP連携などもできそうですね

## やったこと

### Obsidianインストール

インストール後に起動し、保存先フォルダの指定をする。

### ブラウザ拡張のWeb Clipperを入れる

普段使いのブラウザとは別のブラウザ（or 新規に専用のprofile）を用意して、そこにWeb Clipperをインストールした（ブラウザの拡張機能は各々リスクを考えて入れるのがいいと思う）。  
開いていたタブは一旦仮のブックマークフォルダに全て入れて、Web Clipper用のブラウザでimportして開きなおした。

### 記事の保存の挙動確認

ブラウザ拡張をクリックして使ってもよいが、「ALT + SHIFT + O」のショートカット（Windows PC）が割り当てられていたので、これを使った。  
保存したときにObsidian側で取り込んだぺージが開かれていくので、大量のタブを保存していく際には Obsidian Web Clipper拡張の設定 で「ノートを開かずに保存」をしておいてもいい。  
![](https://storage.googleapis.com/zenn-user-upload/1c5199b4f1ac-20241116.png)

### Web Clipperテンプレート

拡張機能の設定画面 からテンプレートを指定できる。設定はexport, import出来るので、Obsidianを使いこなしている人の設定を真似てもいいと思う（自分の設定はとりあえずタブを閉じたい、くらいのモチベーションで少しカスタマイズした程度なので、もっと洗練されたものはあると思う）。  
Web Clipper拡張のデフォルト設定でそこそこの情報量はあり、不要なものは削ってもいいかも。  
取り込んだ記事について、既読か未読かの区別をして未読リストを作りたかった（後述）ので、statusという項目を足した。valueに値を入れておくと取り込み時のデフォルト値としてセットしてくれるので、status: unreadがデフォルトで入るようにした。  
![](https://storage.googleapis.com/zenn-user-upload/acf12841e98f-20241116.png)

自分の設定jsonファイル

```
\`\`\`
{
    "schemaVersion": "0.1.0",
    "name": "Default ",
    "behavior": "create",
    "noteContentFormat": "{{content}}",
    "properties": [
        {
            "name": "title",
            "value": "{{title}}",
            "type": "text"
        },
        {
            "name": "source",
            "value": "{{url}}",
            "type": "text"
        },
        {
            "name": "author",
            "value": "{{author|split:\\\", \\\"|wikilink|join}}",
            "type": "multitext"
        },
        {
            "name": "status",
            "value": "unread",
            "type": "text"
        },
        {
            "name": "published",
            "value": "{{published}}",
            "type": "date"
        },
        {
            "name": "created",
            "value": "{{date}}",
            "type": "date"
        },
        {
            "name": "description",
            "value": "{{description}}",
            "type": "text"
        },
        {
            "name": "tags",
            "value": "clippings",
            "type": "multitext"
        },
        {
            "name": "memo",
            "value": "",
            "type": "text"
        },
        {
            "name": "updated",
            "value": "",
            "type": "text"
        }
    ],
    "triggers": [],
    "noteNameFormat": "{{title}}",
    "path": "Clippings"
}
\`\`\`
```

### プロパティ

取り込んだWebページの一例が以下のスクショ。  
Obsidianで開くとWeb Clipperが自動でセットしたプロパティと、先ほど自分で設定したstatus: unreadがセットされている。  
![](https://storage.googleapis.com/zenn-user-upload/f1e0fd8125b0-20241116.png)  
マークダウンファイルをエディタで開くと以下のようになっている。

```
---
title: Slackコメント読み上げずんだもん（Rust、Tauri）
source: https://zenn.dev/sh11235/articles/b1116a0d8f5a0a
author:
  - "[[Zenn]]"
status: unread
published: 
created: 2024-11-16
description: 
tags:
  - clippings
memo: 
updated: 2024-11-16T20:15
---
## 要旨

Python（任意の書き馴れた言語 or ChatGPTなどで自動生成しやすい言語）でスクリプトを書いてRustに移植したらTauriでそのまま簡単にデスクトップアプリに出来るじゃん！という話です。  
......
```

### Obsidianコミュニティプラグイン: Dataview

Obsidianにプラグインを入れることでカスタマイズが出来る。  
導入は自己責任ということで、デフォルトでは無効化されている。  
Dataviewというプラグインを入れるとSQL感覚でドキュメントを検索出来て、テーブル表示などが出来るようになる。  
書き方は以下のドキュメントを参照。

Obsidianで「未読の記事リスト」のように新規ドキュメントを作り、例えば以下のようなクエリを書くとstatus: unreadと設定したドキュメントの一覧を表示できる。（dataviewクエリを書くまえのバッククォート3つをそのままzennで書くとzennエディタでのコードブロックと干渉するためクエリ部分だけ）。  
プロパティ名をカラムと見なしたSQL感覚の記述が出来る。

```
table source
from ""
where status = "unread"
sort created asc
```

![](https://storage.googleapis.com/zenn-user-upload/063604b6da7f-20241116.png)  
クエリ実行結果の表示には（Obsidianエディタの右上から）プレビューモードに切り替えるとよい。  
このマークダウンファイルを開けば適宜未読記事リストのマークダウンおよびそのマークダウンの元記事リンクを見ることが出来る。  
![](https://storage.googleapis.com/zenn-user-upload/77d3fc2e47f9-20241116.png)

際に該当ファイルの更新時刻順にソートするとWebページの取り込み順に並べられるので、プロパティにupdatedを追加およびupdatedを自動更新する設定も入れた。  
以下の記事が参考になった。バックアップ・端末共有目的でGit連携するのもいいと思う。  
本文を対象にクエリ発行する方法は見つからなかったが、Obsidian標準搭載の検索があるので本文検索はこちらで。  
![](https://storage.googleapis.com/zenn-user-upload/ba1ecd7430e8-20241116.png)  
もっと高度な検索がしたいならプロパティに検索用の項目を設ける、もしくはWeb Clipperがデフォルトで用意してあるtagsプロパティを活用するとよさそう。  
tagsのような、複数テキストをセット可能なプロパティを対象にWHERE句を書きたい場合は `contains` を使えばよい。

```
table source
from ""
where contains(tags, "clippings")
sort created asc
```

## 感想

とりあえずタブをさっさと閉じたいというモチベーションからObsidianを触り始めたが、カスタマイズを凝ったりするとその時間で記事を読めたのでは、という本末転倒感がある。  
しかし、読んだ（読もうとした）記事を検索可能な状態に出来るのは今後嬉しいかなと思う。  
VSCodeで開いてCopilotに質問する、みたいな使い方も出来そう（NotionでやっていればおそらくNotion AIで同じようなことが出来る）。  
一旦はブラウザのタブがスッキリしたことと、本記事をアウトプットしたことで満足した。

脚注

15

3

[^1]: 実はNotionにも同様の機能はあった。  
[https://www.notion.so/web-clipper](https://www.notion.so/web-clipper)  
他にも [Evernote Web Clipper](https://chromewebstore.google.com/detail/evernote-web-clipper/pioclpoplcdbaefihamjohnefbikjilc?hl=ja) 、 [Pocket](https://getpocket.com/about) 、 [Raindrop.io](https://raindrop.io/) 、 [Joplin Web Clipper](https://chromewebstore.google.com/detail/joplin-web-clipper/alofnhikmmkdbbbgpnglcpdollgjjfek?hl=ja-JP) 、 [Save to Obsidian](https://chromewebstore.google.com/detail/save-to-obsidian/lpjpknbaigncdplelcbbpfadnepginhc?hl=ja) 、 [Zotero](https://www.zotero.org/) など、色々な選択肢がある（ChatGPT調べ）。