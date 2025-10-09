---
title: Conventional Commits の基礎知識とそれを支える技術（commitlint + husky）
source: https://zenn.dev/wakamsha/articles/about-conventional-commits
author:
  - Zenn
published: 2024-07-22
created: 2025-05-31
description: Conventional Commits
tags:
  - clippings
  - commit
  - conventional-commits
---
58

4[tech](https://zenn.dev/tech-or-idea)

## これはなに

コミットメッセージの書式を統一するための規約である Conventional Commits について、その基礎知識とそれを取り入れた開発プロセスを円滑に進めるためのノウハウをまとめたものです。

## Conventional Commits とは

Conventional Commits とは、Git などにおけるコミットメッセージを一定の書式に統一することを目的とした規約です。人間と機械のどちらも理解しやすいように設計されており、端的でありながら明瞭なコミットメッセージを記述できます。これにより後からコードベースの変遷を追跡しやすくなるだけでなく、リリースワークフローやコードレビューを自動化するツールとの連携も容易になります。

### 基本的な書式

```
<type>[optional scope]: <description>
  │     │                │
  │     │                └─ コミットの内容を簡潔に記述
  │     │
  │     └─ コミットの影響範囲を指定するスコープ(省略可)
  │
  └─ コミットの種類を指定するタイプ
```

記述例

```
feat: add new feature
fix(api): fix a bug
```

### type

Conventional Commits 公式が定義する type のうち、代表的なものは以下の2種類です。

- `fix`: バグの修正パッチ
- `feat`: 新機能の追加

その他にも以下の type が定義されており、ほぼ全ての変更に対応できるようになっています。

- `docs`: ドキュメントの変更
- `style`: コードのスタイルの変更（セミコロンの追加や改行の修正など）
- `refactor`: コードのリファクタリング（バグの修正や機能の追加は含まない）
- `perf`: パフォーマンスの向上
- `test`: テストの追加・修正
- `build`: ビルドシステムの変更または依存関係のアップデート
- `ci`: CI/CD の設定の変更
- `chore`: その他の変更

### BREAKING CHANGE

Conventional Commits には `BREAKING CHANGE` という特別なキーワードがあり、これをコミットメッセージのフッター部分に含めることでそのコミットが既存のコードに対して破壊的に変更していることを明示できます。

記述例

```
feat: add new feature

BREAKING CHANGE: this is a breaking change
```

もしくは `!` を type / スコープの後に付けることで `BREAKING CHANGE` キーワードを代替できます。

記述例

```
feat!: add new feature
feat(api)!: add new feature
```

## コミットメッセージをチェックするツール

Conventional Commits を開発プロセスに取り入れるためには、コミットメッセージの書式をチェックするツールを導入するのが効果的です。

### commitlint

その名の通りコミットメッセージをチェックするための Linter です。Conventional Commits や独自のメッセージ規約を設定でき、コミットメッセージが規約に適合しているかどうかをチェックできます。本稿では Conventional Commits に準拠したメッセージをチェックするセットアップを紹介します。

パッケージのインストール

```bash
npm install --save-dev @commitlint/cli @commitlint/config-conventional
# or yarn
yarn add --dev @commitlint/cli @commitlint/config-conventional
```

`@commitlint/config-conventional` は Conventional Commits の設定を提供するパッケージです。これ1つで Conventional Commits の設定を簡単に導入できます。先述した type の定義や `BREAKING CHANGE` のチェックもすべてサポートされています。

次に設定ファイルを作成します。リポジトリーのルートディレクトリーに `commitlint.config.js` という名前で以下の内容を記述します。

commitlint.config.js

```js
module.exports = {
  // インストール済みのパッケージを参照する
  extends: ['@commitlint/config-conventional'],
};
```

コミットメッセージなのだからコミット時にチェックするのが妥当です。よって `husky` と組み合わせてコミット実行時にコミットメッセージのチェックが実行される仕組みを構築します。

パッケージのインストール

```bash
npm install --save-dev husky
# or yarn
yarn add --dev husky
```

次にリポジトリーのルートディレクトリーで以下のコマンドを実行して husky の設定ファイルを作成します。

```bash
npx husky install
```

コマンドが成功するとルートディレクトリーに `.husky` ディレクトリーが作成されます。

```
.
+├── .husky/
+│   └── _/
 └── package.json
```

`.husky/_` ディレクトリーに husky の設定ファイルが格納されています。これらのファイルはデフォルトで `.gitignore` に追加されているため、Git で管理されることはありません。したがってリポジトリーをクローンする度に husky の設定ファイルを再生成する必要があります。この作業を自動化するために `prepare` スクリプトを追加します。

package.json

```json
{
  "scripts": {
    "prepare": "husky install"
  }
}
```

`prepare` スクリプトは `npm install` が実行される度に自動で実行される Lifecycle スクリプトの一種です。よって頻繁に実行される `npm install` にあわせて husky の設定ファイルが生成されるので、セットアップのし忘れを予防できます。

最後に husky によってコミット時にコミットメッセージのチェックが実行されるように設定します。`.husky/commit-msg` ファイルを作成し、以下の内容を記述します。

```
.
 ├── .husky/
 │   ├── _/
+│   └── commit-msg
 └── package.json
```

.husky/commit-msg

```bash
npx --no-install commitlint --edit $1
```

これでコミット時に `commit-msg` という [Git フック](https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA-Git-%E3%83%95%E3%83%83%E3%82%AF) が作動してコミットメッセージがチェックされるようになりました。実際にコミットしてエラーが発生するか確認してみましょう。

失敗例

```bash
# 不正なコミットメッセージを入力するとエラーが発生し、コミットが中断される。
git commit -m "add new feature"

⧗   input: add new feature
✖   subject may not be empty [subject-empty]
✖   type may not be empty [type-empty]

✖   found 2 problems, 0 warnings
ⓘ   Get help: https://github.com/conventional-changelog/commitlint/#what-is-commitlint
```

成功例

```bash
# 適切なコミットメッセージを入力すると、何事もなくコミットが完了する。
git cm -a -m 'feat: add new feature'

[ci/commitlint 2aef98a] feat: add new feature
 1 file changed, 2 insertions(+), 1 deletion(-)
```

## 締め

コミットメッセージの記述はソフトウェアを実装する過程で日常的な作業ですが、その重要性が意識されることは少ないです。特に小規模かつ迅速な開発が要求されるプロジェクトでは、コミットメッセージが軽視されがちです。コミット直後は変更内容を鮮明に記憶しているため、コミットメッセージとしてして記録しようという意識が働きにくいからです。しかし、コミットメッセージはコードベースの変遷を追跡するための重要な情報源です。書式がバラバラであったりコミット内容が不明瞭だと、後からコードベースの変遷を追跡するのが困難になります。

Conventional Commits はコミットメッセージの書式を統一するための非常に洗練された規約です。また、本稿で紹介したもの以外にもそれを支える技術として多くの優れたツールが存在します。ぜひ Conventional Commits を取り入れて、未来の自分やチームメンバーにとって読みやすく、追跡しやすいコードベースを作り上げていきましょう。

## 参考文献

- [Conventional Commits](https://www.conventionalcommits.org/ja/v1.0.0/)
- [Angular Commit Message Conventions](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#-commit-message-format)
- [commitlint](https://commitlint.js.org/)
- [Husky](https://typicode.github.io/husky/)
- [Git - Git フック](https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%82%AB%E3%82%B9%E3%82%BF%E3%83%9E%E3%82%A4%E3%82%BA-Git-%E3%83%95%E3%83%83%E3%82%AF)

58

4