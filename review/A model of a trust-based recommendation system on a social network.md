---
created: 2025-05-20
tags:
  - recommend
  - trust-based
---
## Abstract

本論文では、ソーシャルネットワーク上におけるトラストベース推薦システムに関するモデルを提示しています 。このモデルのアイデアは、エージェントが情報に到達するためにソーシャルネットワークを利用し、それをフィルタリングするために信頼関係を用いるというものです 。エージェント間の信頼のダイナミクスがシステムのパフォーマンスにどのように影響するかを、頻度ベースの推薦システムと比較することで調査しています 。さらに、ネットワーク密度、エージェント間の選好の異質性、知識の希薄さがシステムのパフォーマンスにとって重要な要因であることを特定しました 。システムは最適な状態に近いパフォーマンスで自己組織化します 。グローバルレベルでのパフォーマンスは、エージェントの局所的な相互作用から、明示的な調整なしに達成されるシステムの創発的特性です 。

> [!PDF|red] [[A model of a trust-based recommendation system on a social network.pdf#page=2&selection=26,0,32,1&color=red|A model of a trust-based recommendation system on a social network, p.58]]
> > Trust is a topic which has recently been attracting research from many fields, including, but not limited to, computer science, cognitive sciences, sociology, economics, and psychology. As a result of this, there exists a plethora of definitions of trust, some similar to each other, some different from each other. In the context of our model, trust can be defined as the expectancy of an agent to be able to rely on some other agent’s recommendations.
> 
> 信頼は、コンピュータ科学、認知科学、社会学、経済学、心理学など、多くの分野で最近研究の注目を集めているテーマです 。その結果、信頼には多くの定義が存在し、互いに似ているものもあれば、異なるものもあります 。**私たちのモデルの文脈では、信頼は、あるエージェントが他のエージェントの推薦に頼ることができるという、エージェントの期待として定義できます 。**

> [!PDF|red] [[A model of a trust-based recommendation system on a social network.pdf#page=2&selection=40,90,55,1&color=red|A model of a trust-based recommendation system on a social network, p.58]]
> >  The idea at the core of the model is that agents • leverage their social network to reach information; and • make use of trust relationships to filter information.
> 
> モデルの核心にあるアイデアは、エージェントが以下のことを行うというものです。
> - 情報に到達するために**ソーシャルネットワークを活用する** 。
> - 情報をフィルタリングするために**信頼関係を利用する** 。

> [!PDF|red] [[A model of a trust-based recommendation system on a social network.pdf#page=3&selection=44,0,47,95&color=red|A model of a trust-based recommendation system on a social network, p.59]]
> > The model deals with agents which have to decide for a particular item that they do not yet know based on recommendations of other agents. When facing the purchase of an item, agents query their neighbourhood for recommendations on the item to purchase. Neighbours in turn pass on a query to their neighbours in case that they cannot provide a reply themselves
> 
> このモデルは、まだ知らない特定の商品について、他のエージェントからの推薦に基づいて意思決定をしなければならないエージェントを扱います。商品の購入に直面すると、エージェントは購入する商品について、自身の**近隣（neighbourhood）に推薦を問い合わせます。近隣のエージェントは、自分で返答できない場合、さらに自身の近隣に問い合わせを伝播させます。**

> [!PDF|yellow] [[A model of a trust-based recommendation system on a social network.pdf#page=4&selection=3,59,4,71&color=yellow|A model of a trust-based recommendation system on a social network, p.60]]
> > Thus, we explore means to incorporate knowledge of trustworthiness of recommendations into the system.
> 
> しかし、エージェントの**選好の異質性（heterogeneity of preferences）**のために、これは効用の観点から最も効率的な戦略ではないかもしれません。
> そこで、私たちは推薦の**信頼性（trustworthiness）**に関する知識をシステムに組み込む手段を探求します。

## 3.1 Agents, objects, and profiles

![[Pasted image 20250520211208.png]]
![[Pasted image 20250520211224.png]]
![[Pasted image 20250520211324.png]]
> [!PDF|red] [[A model of a trust-based recommendation system on a social network.pdf#page=5&selection=6,0,13,6&color=red|A model of a trust-based recommendation system on a social network, p.61]]
> > Agents rating objects: this is a bipartite graph with the agents on the left hand side and the objects on the right hand side, the ratings being the connections. The set of all possible ratings of an agent constitutes its respective profile
> 
> これは、左側にエージェント、右側にオブジェクトが配置された**二部グラフ（bipartite graph）**で、**評価（ratings）**が接続を表しています。あるエージェントの取りうるすべての評価の集合が、そのエージェントの**プロファイル（profile）**を構成します。

## 3.2 Trust relationships

![[Pasted image 20250520213236.png]]
![[Pasted image 20250520213251.png]]
- 信頼度に推移性が存在し、割引を積で定義しているのには違和感が存在しなくもない
- この場合**信頼度はユーザーiからユーザーjに対する信頼度として定義されている**が、実際にはユーザーjはあるカテゴリkの専門家でありkに関してはめちゃめちゃ信頼できるけど、lに関してはあまり信頼できないっていうパターンがあると思う。だから、**ユーザーの信頼度 = アイテムの信頼度 とするのは少し現実と乖離がある**と思うのだけれど、どうか
	- カテゴリによる信頼のバラつきを表現できていないのでは
	- そのカテゴリにおいて専門的
- この論文において、信頼度は隣人が影響してくるが、隣人は現実世界でどういうことか？
	- SNSのフォロー、友達
	- ECサイトでの購入履歴が似ているユーザー
- 吉村の考え
	1. あるユーザーに隣人を動的に更新させたい(バックエンドで更新 or 推薦をもとにユーザーが自らフォロー)
		- この論文では隣人はあらかじめ存在するリンク集合として固定されており、実験中に追加・削除は行わない
		- そういうユーザーを隣人とするべきなのではと考えるので、どのようにユーザーに隣人候補として他のユーザーを推薦させるかを考えるのは面白いと思う
	2. 隣人の信頼度はカテゴリ別に定義させるのが筋だと思っている
		- あるカテゴリについての専門家は、他のモノを購入したとしてもそれについては門外漢
		- イメージ的には、「例えば美容のカテゴリでこの人が購入するものは信用できるけど、他の食品とかは興味ない」
	- 協調フィルタリングは全ユーザーから購買行動が似ているユーザーを発見しそこから推薦をする。一方、隣人制度を導入する場合、ユーザーに自発的に隣人を定義させる仕組みを作ることでユーザーがアルゴリズムをパーソナライズ化できるから面白いなと思う。
		- SNSにおいてあるカテゴリに詳しいユーザーが発信をしているが、そのようなイメージ。

## 3.3 Temporal structure, search for recommendations

![[Pasted image 20250520214452.png]]
![[Pasted image 20250520214512.png]]
![[Pasted image 20250520214524.png]]
- 不完全探索は、隣人に聞いてみて、評価がある場合はそれで終わりというもので、計算コストは低いが高信頼パスを逃す可能性がある
- 吉村の考え
	- 隣人にのみクエリを送信するっていうのは若干違和感があり引っかかる。隣人が増えていかないし。

## 3.4 Decision Making

![[Pasted image 20250520223805.png]]
![[Pasted image 20250520223818.png]]
- この論文では完全探索モデルを採用している
	- このあと計算量はどうなるんだろうか？
- ロジット関数を使って信頼度をマッピングしているが、他に良い方法はないだろうか？
## 3.5 Trust dynamics

![[Pasted image 20250520224506.png]]
![[Pasted image 20250520224521.png]]
- 信頼の動的な変化について書かれているが、この定式化はまあ理解できないがよさげ
	- 信頼はゆっくりと構築される一方で、素早く失われるというものを表現できているらしい。
- 吉村の考え
	- 隣接するユーザー間でのみしか信頼の更新はされないとされているが、レスポンスの集合から結局選ばれた = 意見を参考にしたユーザーの信頼度を更新するべきではないか？
		- 遠いユーザーの意見を参考にした結果悪かったから隣人の信頼度を下げてしまうと、その隣人のレスポンスが良かった可能性を否定できないと思っている
## 3.6 Utility of agents, performance of the system

![[Pasted image 20250520225009.png]]

## 4 Results

> [!PDF|red] [[A model of a trust-based recommendation system on a social network.pdf#page=9&selection=230,0,235,64&color=red|A model of a trust-based recommendation system on a social network, p.65]]
> > One of the most important results of the model is that the system self-organises in a state with performance near to the optimum. Despite the fact that agents only consider their own utility function and that they do not try to coordinate, long paths of high trust develop in the network, allowing agents to rely on recommendations from agents with similar preferences, even when these are far away in the network. Therefore, the good performance of the system is an emergent property, achieved without explicit coordination.
> 
> モデルの最も重要な結果の一つは、システムが**最適なパフォーマンスに近い状態へと自己組織化する**ことです 。エージェントが自身の効用関数のみを考慮し、協調しようとしないにもかかわらず、ネットワーク内では**高い信頼の長いパスが形成されます**。これにより、エージェントはネットワーク内で遠く離れていても、類似の選好を持つエージェントからの推薦に頼ることができます 。したがって、システムの優れたパフォーマンスは、**明示的な調整なしに達成される創発的な特性**なのです 。
