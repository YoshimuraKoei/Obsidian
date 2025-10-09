---
title: "プロンプトエンジニアリングの基礎と応用"
source: "https://aismiley.co.jp/ai_news/what-is-prompt-engineering/"
author: "AIsmiley"
published: 2023-05-25
created: 2024-03-19
tags:
  - "prompt-engineering"
  - "AI"
  - "LLM"
  - "ChatGPT"
---

# プロンプトエンジニアリングとは

![プロンプトエンジニアリングの概要](https://aismiley.co.jp/wp-content/uploads/2023/05/2023-05-26-170905.png)

プロンプトエンジニアリングは、大規模言語モデル（LLM）を効率的に使用するために、言語モデルへの命令（プロンプト）を開発・最適化する学問分野です。ChatGPT などの AI モデルを最大限に活用するために必須のスキルとなっています。

## プロンプトの基本要素

![プロンプトの要素](https://aismiley.co.jp/wp-content/uploads/2023/05/49_prompt_engineering.png)

優れたプロンプトには以下の 4 つの要素が含まれます：

1. **指示（Instruction）**: モデルが実行する特定のタスクや命令
2. **背景（Context）**: モデルの回答精度を高めるための追加情報や文脈
3. **入力データ（Input Data）**: モデルに応答を求める入力や質問
4. **出力形式/出力指示子（Output Indicator）**: 出力タイプやフォーマット

# プロンプトの種類と実践方法

## 1. 初歩的なプロンプト

### Zero-shot Prompting

![Zero-shot Promptingの例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_06.png)

- 例やデモンストレーションなしに直接質問を投げる方法
- 単純なタスクに適している
- 複雑なタスクでは精度が低下する可能性がある

### Few-shot Prompting

![Few-shot Promptingの例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_07.png)

- モデルに例やデモンストレーションを提供する方法
- 文脈学習を通じて質問と回答のパターンを学習
- デモの数が多いほど適切な回答の確率が高まる

## 2. 応用プロンプト

### Chain-of-Thought (CoT) Prompting

![CoT Promptingの例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_10.png)

- 連鎖的な思考をさせることで出力精度を高める
- 段階的な推論が必要な問題に適している
- 思考プロセスを可視化できる

### Zero-shot CoT

![Zero-shot CoTの例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_11.png)

- CoT の手法を Zero-shot で用いる
- 「ステップに分けて考えてください」などの指示を追加
- 段階的な推論を促す

### Self-Consistency

![Self-Consistencyの例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_13.png)

- 複数の推論パスを生成し、最も整合性の高い回答を選択
- 複雑な推論タスクに有効

### Generate Knowledge Prompting

![Generate Knowledge Promptingの例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_15.png)

- プロンプトに知識や情報を組み込む
- 正しい推論の出力確率を高める

### ReAct

![ReActの例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_16.png)

- 推論（Reasoning）と行動（Acting）を組み合わせる
- 外部環境からの情報収集と推論を交互に行う

# セキュリティとリスク対策

## 敵対的プロンプトの種類

1. **プロンプトインジェクション**

![プロンプトインジェクションの例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_20.png)

- モデルの出力を乗っ取る攻撃手法
- 機密情報の開示や虚偽情報の拡散のリスク

2. **プロンプトリーク**

![プロンプトリークの例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_21.png)

- プロンプトの情報を引き出す攻撃
- 機密情報の漏洩リスク

3. **ジェイルブレイク**

![ジェイルブレイクの例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_23.png)

- モデルの制限を外す手法
- 非倫理的な内容の出力リスク

## リスク対策

1. **指示に「無視」を意味するプロンプトを含める**

![無視プロンプトの例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_26.png)

- プロンプト攻撃を回避する基本的な対策

2. **敵対的プロンプト検知システムの活用**

![敵対的プロンプト検知の例](https://aismiley.co.jp/wp-content/uploads/2023/05/what-is-prompt-engineering_28.png)

- 言語モデルによる攻撃検出
- セキュリティ強化

# プロンプト設計のベストプラクティス

1. **明確で具体的な指示**

   - 曖昧な質問を避け、詳細な背景情報を提供
   - 質問の焦点を明示

2. **コンテキストの提供**

   - 関連する背景情報を含める
   - より適切な回答を引き出す

3. **オープンエンド質問の活用**

   - 深い洞察やアイデアを引き出す
   - 段階的な質問の展開

4. **適切な長さの維持**
   - 簡潔でありながら必要な情報を含める
   - 理解しやすい形式を心がける

# まとめ

プロンプトエンジニアリングは、AI モデルを効果的に活用するための重要なスキルです。適切なプロンプト設計により、より正確で有用な回答を得ることができます。ただし、セキュリティリスクへの対策も必要不可欠です。今後も進化が期待される分野であり、継続的な学習と実践が重要です。
