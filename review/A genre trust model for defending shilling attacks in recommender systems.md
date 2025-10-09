---
tags: 
created: 2025-06-05
source: "[[A genre trust model for defending shilling attacks in recommender systems.pdf]]"
---
---
## Abstract
**共調フィルタリング（CF）推薦システムはシリング攻撃に対し脆弱である**ことが指摘されており、**CF推薦アルゴリズムにおける信頼が、システム推薦の精度向上に役立つ**ことが証明されています。この分野で信頼に特化した研究が少ないため、本稿では信頼をシリング攻撃対策に利用することの利点を探ります。**単にユーザーが生成した信頼値を使用するのではなく、アイテムのジャンルによって異なる「ジャンル信頼度」を提案**し、信頼値とユーザーの信頼性の両方を考慮に入れます。本稿では、シリング攻撃の種類を複数提示し、ユーザーの信頼値と行動特性がシリング攻撃への防御に与える影響について考察します。同時に、ジャンル信頼度に基づく推薦モデルを構築するために、ユーザー類似度の計算アプローチを改善します。ジャンル信頼度に基づく推薦システムの性能は、Ciaoデータセットで評価されています。実験結果は、異なる種類のシリング攻撃に対する防御において、ジャンル信頼度が優れており、同等の推薦精度を持つことを示しました。

## Introduction
> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=1&selection=72,0,74,43&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2929]]
> > However, user’s trust relationships usually follow a power-law distribution, and a large, long tail of users only have few trusted or distrusted users [10]. 
> 
> しかし、ユーザーの信頼関係は通常、べき乗則分布に従い、信頼または不信の対象となるユーザーがごくわずかしかいない、長く裾野の広いユーザー層が存在します 。
> 
> → 信頼・不信される対象となるユーザーはごくわずか。インフルエンサーが良い例かな。

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=1&selection=80,36,82,15&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2929]]
> > The main idea of our model is the synthesis of user credibility and the trust degree between users. 
> 
> 我々のモデルの主なアイデアは、ユーザーの信頼性とユーザー間の信頼度を統合することです 。

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=1&selection=84,38,88,33&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2929]]
> >  first, we propose a new method to detect shilling attackers with higher accuracy and ensure the stability of the recommendation results. Second, we propose an improve trust degree calculation algorithm to resist shilling attacks.
> 
> 第一に、より高い精度でシリング攻撃者を検出し、推薦結果の安定性を確保する新しい手法を提案します 。第二に、シリング攻撃に耐えるための改良された信頼度計算アルゴリズムを提案します 。
> 
> → 推薦の精度は落とさず、シリング攻撃に耐えうるアルゴリズムを提案するってことね。

## Related Work

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=2&selection=17,0,19,64&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2930]]
> > The shilling attack [13] is one type of the profile-injection attacks that is regularly used, and research in the area of shilling attacks has made significant advances in the last few years
> 
> シリング攻撃 は、定期的に使用されるプロファイル注入攻撃の一種であり、シリング攻撃の分野の研究はここ数年で大きな進歩を遂げています。

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=2&selection=21,6,23,30&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2930]]
> >  From the intention perspective, shilling attacks were classified into two basic types in researchers’ early work: the push attack and “nuke” attack.
> 
> 意図の観点から、シリング攻撃は研究者の初期の研究で2つの基本タイプに分類されました。プッシュ攻撃と「ヌーク」攻撃です 。
> 
> → 知らなかった。

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=2&selection=32,59,35,19&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2930]]
> >  The commonly used methods of shilling attacks are the random attack, average attack, bandwagon attack, segmented attack, and sampling attack
> 
> 一般的に使用されるシリング攻撃の手法は、ランダム攻撃、アベレージ攻撃、バンドワゴン攻撃、セグメント攻撃、サンプリング攻撃です 。

> [!PDF|yellow] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=2&selection=35,19,50,32&color=yellow|A genre trust model for defending shilling attacks in recommender systems, p.2930]]
> > : (a) Random attack: The ratings of filler items are chosen randomly and the target item is assigned with a pre-specified rating. The selected item sets are empty. (b) Average attack: A new attack model with a better attack effect than random attack. The selected items sets are still empty, while the ratings of filler items are the individual mean for that item rather than the global mean. (c) Bandwagon attack: Attackers pick up a part of frequently rated items in system as the selected items. The filler items are randomly chosen while the rating value for them is the mean across the whole system. (d) Segmented attack: Avoiding being detected, attackers give high ratings to those items similar to target items. (e) Sampling attack: This attack model needs to know more system information for copying existing user models. Attackers imitate real users selecting and rating items, while the target item are assigned with the highest rating
> 
> (a) **ランダム攻撃**: フィラーアイテムの評価はランダムに選択され、ターゲットアイテムには事前に指定された評価が割り当てられます。選択アイテムのセットは空です 。
> (b) **アベレージ攻撃**: ランダム攻撃よりも攻撃効果が高い新しい攻撃モデルです 。選択アイテムのセットは空のままですが、フィラーアイテムの評価は、全体平均ではなくそのアイテムの個別平均になります 。
> (c) **バンドワゴン攻撃**: 攻撃者は、システム内で頻繁に評価されるアイテムの一部を選択アイテムとして選びます 。フィラーアイテムはランダムに選ばれ、その評価値はシステム全体の平均値となります 。
> (d) **セグメント攻撃**: 検出を避けるため、攻撃者はターゲットアイテムに類似したアイテムに高い評価を与えます。
> (e) **サンプリング攻撃**: この攻撃モデルは、既存のユーザーモデルをコピーするために、より多くのシステム情報を知る必要があります 。攻撃者は、アイテムを選択し評価する実際のユーザーを模倣しますが、ターゲットアイテムには最高の評価を割り当てます 。
> 
> → これめっちゃ大事や！！！

> [!PDF|yellow] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=2&selection=55,33,69,17&color=yellow|A genre trust model for defending shilling attacks in recommender systems, p.2930]]
> >  Alostad et al. [15] presented an improved support vector machine (SVM) and Gaussian mixture model (GMM)-based shilling attack detection algorithm (SVM-GMM). Samaiya et al. [16] combined PCA and SVM—two classification algorithms—to improve the effect of the defense against shilling attacks. Lee et al. [17] proved that users’ evaluations are affected by other users and that users connected by a network of trust exhibit significantly higher similarity on items. Ardissono et al. [18] proposed a compositional recommender system based on multi-faceted trust using social links and global feedback about users. Jaehoon et al. [19] proposed a new trust recommendation algorithm, TCRec. Pan et al. [20] presented an adaptive learning method to map trust values to the [0, 1] interval and use “directionality” to distinguish users
> 
> Alostadら は、改良されたサポートベクターマシン（SVM）とガウス混合モデル（GMM）に基づくシリング攻撃検出アルゴリズム（SVM-GMM）を発表しました 。Samaiyaら は、主成分分析（PCA）とSVMという2つの分類アルゴリズムを組み合わせることで、シリング攻撃に対する防御効果を向上させました 。Leeら は、ユーザーの評価が他のユーザーに影響されること、そして信頼ネットワークでつながっているユーザーはアイテムに対して著しく高い類似性を示すことを証明しました 。Ardissonoら は、ソーシャルリンクとユーザーに関するグローバルなフィードバックを利用し、多面的な信頼に基づく構成的なレコメンダーシステムを提案しました 。Jaehoonら は、新しい信頼推薦アルゴリズムTCRecを提案しました 。Panら は、信頼値を[0, 1]の区間にマッピングし、「指向性」を用いてユーザーを区別する適応学習手法を提示しました 。
> 
> → 先行研究を一望できる場所！！

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=2&selection=70,0,71,40&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2930]]
> > However, the limitation that these methods need more extraneous information is also obvious, 
> 
> しかし、これらの手法がユーザータイプやアイテムカテゴリといった、より多くの外部情報を必要とするという限界も明らかであり、依然としてコールドスタートの問題に悩まされています 。

## Trust degree in recommender system

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=2&selection=96,0,105,27&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2930]]
> > Trust is generally considered to be a complex concept; two common definitions of trust, reliability trust [21]and decision trust [22], are most widely used.
> 
> 信頼は一般的に複雑な概念と考えられており、「信頼性信頼（reliability trust）」と「決定信頼（decision trust）」という2つの一般的な定義が最も広く使われています。

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=2&selection=105,28,108,28&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2930]]
> > However, in recommender systems, trust is mostly defined as being correlated with similar preferences toward items commonly rated by two users [23, 24].
> 
> しかし、レコメンダーシステムでは、信頼は主に、2人のユーザーが共通して評価したアイテムに対する嗜好の類似性と相関するものとして定義されます。
> 
> → ユーザーがそれを意識して信頼ネットワークの構築を行っているか？は疑問。

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=3&selection=6,0,7,58&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2931]]
> > With intensive and deeper study, trust measurement methods can be divided into global trust and local trust [26].
> 
> 研究が深化するにつれて、信頼の測定方法はグローバル信頼とローカル信頼に分けられるようになりました。ローカル信頼手法の方がシリング攻撃への耐性において優れた性能を示すことがわかっています。
> 
> → これは知らなかった。どのようにグローバル信頼とローカル信頼を分けるのだろうか。

> [!PDF|yellow] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=3&selection=12,0,64,24&color=yellow|A genre trust model for defending shilling attacks in recommender systems, p.2931]]
> > • Asymmetry/subjectivity: A user may hold different opinions toward different target users, and different users also may have various opinions or trust degrees with the same user. Thus, we can have trust(u, v) ≠ trust(v, u). • Transitivity: If user u trusts user v, and user v trusts user w, we believe that user u trusts user w to same degree. • Dynamicity/temporality: Trust is the users’ previous interactions and it changes over time. • Context Dependence: Trust is context-specific, which indicates a user who is trustable in art may not be helpful in computer science.
> 
> - **非対称性/主観性**: あるユーザーは、異なるターゲットユーザーに対して異なる意見を持つことがあり、また、異なるユーザーが同じユーザーに対して様々な意見や信頼度を持つこともあります。したがって、trust(u,v)=trust(v,u) となります
> - **推移性**: ユーザーuがユーザーvを信頼し、ユーザーvがユーザーwを信頼している場合、ユーザーuはユーザーwをある程度信頼していると考えられます。
> - **動的性/時間性**: 信頼はユーザーの過去の相互作用であり、時間とともに変化します。
> - **文脈依存性**: 信頼は文脈に固有であり、これは芸術の分野で信頼できるユーザーが、コンピュータサイエンスの分野では役立たない可能性があることを示しています。
>   
>   → カテゴリ別の信頼の考慮はここでいう文脈依存性だったのか。

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=3&selection=99,19,101,18&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2931]]
> >  Sotos et al. [31] proposed that the Pearson correlation coefficient is not transitive unless users are highly correlated.
> 
> Sotosらは、ユーザー間に高い相関がない限り、ピアソン相関係数には推移性がないと提案しました。
> 

> [!PDF|yellow] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=3&selection=247,0,250,22&color=yellow|A genre trust model for defending shilling attacks in recommender systems, p.2931]]
> > The model described in this paper improves trust calculation methods by distinguishing trust value by ratings in different genres and providing a more reasonable user similarity calculation algorithm.
> 
> 本稿で説明するモデルは、異なるジャンルの評価によって信頼値を区別し、より合理的なユーザー類似度計算アルゴリズムを提供することで、信頼度計算手法を改善します。

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=3&selection=386,24,388,32&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2931]]
> > In our paper, we propose that trust should be useful not only to generate item predictions but also to suggest reliable users. 
> 
> 本稿では、信頼はアイテムの予測を生成するだけでなく、信頼できるユーザーを提案するためにも有用であるべきだと提案します。

> [!PDF|yellow] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=3&selection=389,27,394,35&color=yellow|A genre trust model for defending shilling attacks in recommender systems, p.2931]]
> > there are two parts to the trust degree in the GTD model: the traditional trust value between users and the credibility of users.
> 
> 従来のユーザー信頼度計算手法とは対照的に、GTDモデルにおける信頼度には2つの部分があります。それは、ユーザー間の従来の信頼値と、ユーザーの信頼性です 。
> 
> → 気になる。

> [!PDF|red] [[A genre trust model for defending shilling attacks in recommender systems.pdf#page=3&selection=436,14,438,21&color=red|A genre trust model for defending shilling attacks in recommender systems, p.2931]]
> >  In this paper, we suppose that users have their own preferences, which leads to users better understanding their familiar areas.
> 
> 本稿では、ユーザーは自身の好みを持っており、それがユーザーが精通している分野をよりよく理解することにつながると仮定します 。







## 単語
- homophile ... 同質性、同性愛者
- profile-injection ... プロファイル注入攻撃
- extraneous ... 外部の、無関係な
- ameliorate ... 改善する
