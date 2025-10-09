この論文はレビュー。
[[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf]]
## Summary

ウェブ上の情報が爆発的に増加し、新しいウェブサービスが迅速に提供されるにつれて、**推薦システム**は、ユーザーが関心を持つ可能性のあるアイテムに関して助言を提供する効率的なメカニズムとして登場しました 。しかし、ユーザーの過去の行動やアイテムの説明のみをマイニングする従来の推薦システムには、いくつかの欠点がありました 。これらの限界を克服するため、近年大きな注目を集めている新しい解決策が**信頼データ**の利用です 。

しかし、このアプローチは、推薦システムのアプリケーション、根底にあるソーシャルネットワーク構造、およびユーザーのニーズに応じて、適切な信頼データを活用する上で課題を提示します 。さらに、選択的な意思決定システムにおいて、**信頼**はソーシャルネットワークデータの一種として重要な役割を果たし、推論を行うための適切なアプローチを必要とします 。

本稿は、現在の**信頼ベースのソーシャル推薦システム**に関する体系的な文献レビューを提供します 。また、既存の信頼関連ソーシャルネットワークデータ分析アプローチから利用および推論された信頼の種類の詳細な分類も提示します 。さらに、最も人気のある信頼ベースのソーシャル推薦システムの主な特性と課題について論じます 。最後に、本研究の発見を提示し、研究者がより強化された推薦システムを開発するための洞察を提供する未解決の問題について議論します 。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=2&selection=5,0,8,10&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.2]]
> > Furthermore, they are mostly pertained to machine learning, data mining, information retrieval and other research areas outside the scope of this study. Recommendation systems are generally classified into four main categories: demographic filtering systems, content-based methods, collaborative filtering methods and hybrid approaches
> 
> レコメンダーシステムは、機械学習、データ麻依イング、情報検索、および本研究の範囲外の他の研究分野に関連している。人口統計フィルタリングシステム、コンテンツベースの手法、協調フィルタリング手法、およびハイブリッドアプローチの4つの主要なカテゴリに分類される。

> [!PDF|red] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=2&selection=9,0,13,7&color=red|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.2]]
> > Though recommendation systems have been extensively investigated in both the industry and academia, the following major problems still persist: the sparsity problem which defines the conditions that a small fraction of the existing items is rated by users; the cold-start problem which defines users with no or few ratings or defines items with few provided ratings by users; and trustworthiness problem that represents the inability to distinguish users' credentials and ratings
> 
> レコメンダーシステムは、産業界と学術界の両方で広範に研究されてきましたが、以下の主要な問題が依然として残っています。既存のアイテムのごく一部しかユーザーによって評価されない状況を定義する**スパース性問題** 、評価の全くない、またはほとんどないユーザー、あるいはユーザーによってほとんど評価が提供されていないアイテムを定義する**コールドスタート問題** 、そしてユーザーの信頼性と評価を区別できないことを示す**信頼性問題**です 。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=2&selection=16,85,18,69&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.2]]
> > Among the presented techniques, a type of creation that has attracted a great deal of interest is integration of social networks information as a supplementary information to the traditional recommender systems data
> 
> 従来のレコメンダーシステムへの補足情報としてソーシャルネットワーク情報を統合することが特に注目を集めている。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=2&selection=27,64,29,1&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.2]]
> >  It has been proven that utilizing social network information improves the recommendation process. 
> 
> ソーシャルネットワーク情報を利用することで、レコメンデーションプロセスが改善されることが証明されている。

> [!PDF|red] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=2&selection=32,0,33,115&color=red|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.2]]
> > Moreover, among the various types of social information used, one which has attracted much attention and has played a significant role in enhancing recommender systems is the trust factor
> 
> 使用されるさまざまな種類のソーシャル情報の中で、レコメンダーシステムの強化において多くの注目を集め、重要な役割を果たしているのは信頼因子です。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=2&selection=38,26,40,95&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.2]]
> >  For instance, in relation to the matrix factorization method, which is a commonly used method in trust-based recommender systems, setting the initial value for it is effective in the final result. Therefore, finding a solution to set the appropriate initial value can help to further enhance the system.
> 
> 信頼性に基づくレコメンダーシステムで一般的に使用される行列分解法に関連して、その初期値の設定は最終結果に影響を与える。
>したがって、適切な初期値を設定するソリューションを見つけることは、システムの更なる強化に役立つ。

> [!PDF|red] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=2&selection=46,9,48,6&color=red|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.2]]
> > to the best of our knowledge, despite the usefulness of trust-related social networks data for recommender systems and the aforementioned issues, no precise and comprehensive systematic review of its analytical approaches exist.
> 
> しかし、私たちの知る限りでは、レコメンダーシステムにとって信頼性関連のソーシャルネットワークデータが有用であるにもかかわらず、また前述の問題にもかかわらず、その分析アプローチに関する正確で包括的なシステマティックレビューは存在しません 。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=2&selection=50,70,51,100&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.2]]
> >  However, none of these reviews are related to trust-based social recommender systems, which demonstrates the novelty of the present research topic
> 
> レコメンダーシステムの課題、ロケーションベースのレコメンダーシステム、リンク予測レコメンダーシステム、レコメンダーシステムにおける新規ユーザーのコールドスタート問題、モバイルベースのレコメンダーシステム、およびレコメンダーシステムのパーソナライズに関するレビューは行われてきたが、どれも信頼性に基づくソーシャルレコメンダーシステムに関連するものではなく、これが本研究テーマの新規性を示している。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=2&selection=52,100,54,106&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.2]]
> >  For this purpose, it was determined that social networks data analysis approaches for trust-based social recommendation systems can be investigated from two categories: trust of items-based category and trust of relationships-based category.
> 
> 信頼性に基づくソーシャルレコメンダーシステムのためのソーシャルネットワークデータ分析アプローチは「アイテムの信頼性に基づくカテゴリ」と「関係性の信頼性に基づくカテゴリ」の2つのカテゴリから調査できると判断された。

> [!PDF|red] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=2&selection=54,106,56,95&color=red|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.2]]
> >  Trust of itemsbased category investigates the recommender systems with main focus being on determining the more trustworthy products by utilizing the underlying network attributes in order to compute their trust degree.
> 
> **アイテムの信頼性に基づくカテゴリ**は、基盤となるネットワーク属性を利用してアイテムの信頼度を計算することにより、より信頼できる製品を特定することに主眼を置いたレコメンダーシステムを調査します 。

> [!PDF|red] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=2&selection=56,105,58,63&color=red|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.2]]
> > trust of relationships-based category considers the recommender systems, which use explicit or implicit friendship strength between users for utilizing their opinions for recommendations.
> 
> **関係性の信頼性に基づくカテゴリ**は、レコメンデーションのためにユーザー間の明示的または暗黙的な友情の強さを使用するレコメンダーシステムを考慮します 。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=3&selection=3,100,24,17&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.3]]
> > In summary, the present paper's contributions to existing knowledge are as follows: • Analysis of trust-related social networks for recommendation systems approaches based on evaluation goal, metrics, measures, datasets, benchmarks, and case studies • Investigating the upcoming challenges for recommendation systems and the functionality that trust-related social networks data can have • Presenting key finding of current social trust-based recommendation systems from different perspectives using a systematic review • Defining key areas that future research can enhance the trust-related social networks data's utilization for recommendation systems
> 
> 要約すると、本論文の既存の知識への貢献は以下の通り。
> - 評価目標、指標、測定、データセット、ベンチマーク、およびケーススタディに基づいた、レコメンデーションシステムアプローチのための信頼性関連ソーシャルネットワークの分析
> - レコメンデーションシステムにおける今後の課題と、信頼性関連ソーシャルネットワークデータが持ちうる機能の調査
> - システマティックレビューを用いた、現在のソーシャル信頼性に基づくレコメンデーションシステムの異なる視点からの主要な調査結果の提示
> - 将来の研究がレコメンデーションシステムにおける信頼性関連ソーシャルネットワークデータの利用を強化できる主要分野の定義

> [!PDF|red] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=3&selection=66,0,70,0&color=red|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.3]]
> > Content-based recommender systems: These recommender systems provide recommendation by making a comparison between the content of an item with the features of the user's favorite content.
> 
> **コンテンツベースのレコメンダーシステム**: これらのレコメンダーシステムは、アイテムのコンテンツとユーザーのお気に入りのコンテンツの特性を比較することによって、レコメンデーションを提供します 。

> [!PDF|red] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=3&selection=73,0,76,63&color=red|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.3]]
> > Collaborative filtering recommender systems: These recommendation systems provide recommendation by utilizing the items-related judgment of similar users to the target user.
> 
> **協調フィルタリングレコメンダーシステム**: これらのレコメンデーションシステムは、ターゲットユーザーに類似するユーザーのアイテム関連の判断を利用してレコメンデーションを提供します 。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=3&selection=78,0,80,17&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.3]]
> > Collaborative recommendation systems are divided into two categories: memory-based collaborative recommendation systems that are also called neighborhood collaborative recommendation systems and model-based collaborative recommendation systems
> 
> 協調レコメンデーションシステムは、**メモリベース協調レコメンデーションシステム**（近傍ベース協調レコメンデーションシステムとも呼ばれる）と**モデルベース協調レコメンデーションシステム**の2つのカテゴリに分けられます 。

> [!PDF|red] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=3&selection=80,18,80,115&color=red|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.3]]
> >  The memory-based recommendation system differentiates a set of similar users to the target user,
> 
> メモリベースのレコメンデーションシステムは、ターゲットユーザーに類似するユーザーのセットを区別し 、次に類似するユーザーの意見を集約することによって、アイテムに関するターゲットユーザーの意見を決定します 。

> [!PDF|red] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=4&selection=0,103,2,109&color=red|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.4]]
> >  A model-based recommendation system is a method of latent factor models that demonstrates the items and users in low-dimensional space; the recent demonstration of items and user are calculated by minimizing the regularized squared error.
> 
> **モデルベースのレコメンデーションシステム**は、アイテムとユーザーを低次元空間で表現する潜在因子モデルの手法であり 、アイテムとユーザーの最近の表現は、正則化された二乗誤差を最小化することによって計算されます。

[[Recommender System.pdf]] にメモリベース法とモデルベース法は記載されている。

利用者間型メモリベース法
→ **活動利用者と嗜好が似ている人が好きなアイテム**を推薦

アイテム間型メモリベース法
→ 活動利用者が好きな**アイテムと類似したアイテム**を推薦

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=4&selection=6,0,9,90&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.4]]
> > Demographic recommender systems: These recommendation systems provide the recommendation established on the utilization of the user's personal features (gender, location, age, income and so on).
> 
> **人口統計レコメンダーシステム**: これらのレコメンデーションシステムは、ユーザーの個人的な特徴（性別、場所、年齢、収入など）の利用に基づいてレコメンデーションを提供します。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=4&selection=9,4,17,3&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.4]]
> > utilization of the user's personal features (gender, location, age, income and so on). Hybrid recommender systems: These recommendation systems incorporate two or more kinds of recommendation systems into one model; they usually have superior results to simple recommender techniques, but are much more complex in design. 7,8
> 
> **ハイブリッドレコメンダーシステム**: これらのレコメンデーションシステムは、2つ以上の種類のレコメンデーションシステムを1つのモデルに組み込みます。これらは通常、単純なレコメンダー技術よりも優れた結果をもたらしますが、設計がはるかに複雑です。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=4&selection=19,0,30,1&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.4]]
> > Social networks-based recommender systems: Social recommendation systems can extract heterogeneous information from social networks for improving the classical recommendation approaches and present novel kinds of recommendations. For instance, they can suggest not only items but also movies, users, events, locations, groups, and services to users, using particular algorithms. 9 Figure 1 presents an illustrative instance of these recommender systems. 9
> 
> **ソーシャルネットワークベースのレコメンダーシステム**: ソーシャルレコメンデーションシステムは、従来のレコメンデーションアプローチを改善し、新しい種類のレコメンデーションを提示するために、**ソーシャルネットワークから異種情報を抽出できます** 。例えば、特定のアルゴリズムを使用して、アイテムだけでなく、動画、ユーザー、イベント、場所、グループ、サービスなどもユーザーに提案できます 。図1は、これらのレコメンダーシステムの例を示しています 。

![[Pasted image 20250520200137.png]]

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=4&selection=32,0,38,2&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.4]]
> > Trust-based social recommendation systems: The main utilized social networks information of these systems is the trust factor related information such as trust explicit statements between users and implicit trust calculated from other social networks data for further improving recommendation results. 10
> 
> **信頼性に基づくソーシャルレコメンデーションシステム**: これらのシステムの主な利用ソーシャルネットワーク情報は、**ユーザー間の明示的な信頼声明**や、レコメンデーション結果をさらに改善するために**他のソーシャルネットワークデータから計算される暗黙的な信頼**などの、**信頼因子**関連情報です 。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=5&selection=10,113,12,38&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.5]]
> >  In the following section, the used metrics for amelioration evaluation in the primary studies and some other important metrics for recommender systems are discussed.
> 
> 以下のセクションでは、主要な研究で改善評価のために使用された指標と、レコメンダーシステムにとって他のいくつかの重要な指標について議論します 。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=6&selection=45,0,46,29&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.6]]
> > In this subsection, the measures used to present volume of the effects on the metrics used for evaluation of the recommender systems are explained.
> 
> このサブセクションでは、レコメンダーシステムの評価に用いられる指標に対する効果の大きさを表すために使用される尺度について説明します。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=7&selection=31,0,31,103&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.7]]
> > This subsection presents the real datasets utilized for the trust-based recommender systems evaluation.
> 
> このサブセクションでは、**信頼性に基づくレコメンダーシステムの評価に利用される実際のデータセットについて説明**します。

- **Amazon ウェブサイト**: このデータセットは、製品のメタデータと、ユーザーが様々な製品について提供したレビュー情報に関連する 。

- **Epinions**: このデータセットには、ユーザーがアイテムに評価を付け、自身のレビューを提供する製品レビュー情報が含まれている 。さらに、各ユーザーが信頼する人物を指定することに基づいて、信頼のソーシャルネットワークが構築されている 。

- **Advogato**: このデータセットは、ソフトウェア開発者のためのオンラインコミュニティである 。このデータセットでは、各ユーザーがオブザーバー、見習い、熟練者、マスターの4つの評価基準で他のユーザーを承認できる 。

- **Wikipedi vote network**: このデータセットは、投票プロセスを通じて接続が構築されるソーシャルネットワークウェブサイトである 。例えば、ユーザーXからユーザーYへの有向エッジは、XがYを管理者に投票したことを意味する 。

- **Enron email**: これは、メールの送受信によって形成されたメールデータセットである 。

- **Flixster**: このデータセットでは、ユーザーが映画に評価を付けるだけでなく、ソーシャルネットワークを構築するために他のユーザーを友達として追加できる 。

- **MovieLens**: このデータセットは、ユーザーの映画の評価と、ユーザーに関するいくつかの人口統計情報を含む比較的小さなデータセットである 。

- **Baidu**: このデータセットには、異なる映画に対するユーザーの評価、ユーザーの関係、映画のタグが含まれている 。

- **Facebook**: このデータセットは、ユーザーが家族や友人と連絡を取り合い、情報を共有できる有名なソーシャルネットワークウェブサイトの1つである 。

- **Sina Weibo**: このデータセットは、フォロワー/フォロー関係を含むミニブログサイトである 。

- **Tencent Weibo**: このデータセットは、すべてのユーザーを接続する最大のミニブログウェブサイトの1つである 。

- **Youku**: このデータセットは大規模な動画共有ウェブサイトであり、各動画にはユーザーによって異なる短い文章で動画の内容を説明するタグのリストが提供されている 。

- **Slashdot**: このデータセットは、ユーザーが友達と敵の関係でつながる技術関連のニュースウェブサイトである 。

- **Renren**: このデータセットは、Facebookに似た中国のソーシャルネットワーキングサービスであり、ユーザーは互いに接続してコミュニケーションを取り、情報を共有し、モバイルライブストリーミングにアクセスできる 。

- **INFOCOM**: このデータセットには、ユーザーの連絡先と属性を介した接続が含まれている 。

- **Douban**: このデータセットはソーシャルネットワーキングウェブサイトであり、登録ユーザーは最近のイベント、本、音楽、活動に関連する評価などの情報を記録し、そのレビューを友人と共有できる 。さらに、DoubanのメカニズムはTwitterとまったく同じである 。

- **Ciao**: このデータセットには、ユーザーが付けた映画の評価の記録と、ユーザーが発行した信頼の記述が含まれている 。

- **Meetup**: このデータセットは、イベント主導型のソーシャルネットワーキングサイトであり、イベント開発者がイベントを告知する 。さらに、ユーザーはイベントに参加したり、自身の個人的な興味に関連するグループに参加したりできる 。

- **Film Trust**: このデータセットには、ユーザーのアイテムに対する評価と、他のユーザーに対するユーザーの信頼の記述が含まれている 。

- **Foursquare**: このデータセットは、ユーザープロフィール、ユーザー間の信頼関係、チェックイン活動、興味のある場所を含む位置情報ベースのソーシャルネットワーキングサイトである 。より詳細には、各チェックイン訪問には、場所ID、ユーザーID、場所カテゴリ、日付と時刻、経度と緯度、およびこれらの訪問でタグ付けされたユーザーが含まれる 。友情は、これらの訪問におけるタグ付けされた情報を利用して達成され、同じ訪問でタグ付けされたユーザーは互いに友人であると見なされる 。

- **Gowalla**: このデータセットは、位置情報ベースのソーシャルネットワークサイトであり、ユーザーが訪問した場所の共有を可能にするチェックインと呼ばれる情報をユーザーが提供する 。

- **Geolife**: このデータセットは、ユーザーの特定の期間にわたる軌跡に関連しており、日常生活だけでなく趣味やスポーツ活動も含まれている 。

- **T-Drive**: このデータセットには、北京内の特定の期間における多数のタクシーのGPS軌跡が含まれている 。

- **Last.fm**: このデータセットは、音楽とトラックのレコメンデーションを行うソーシャルネットワーキングウェブサイトである 。また、ユーザー間の友情関係や、これらのユーザーがタグ付けした音楽トラックやアーティストも含まれている 。

- **Delicious**: このデータセットは、ウェブブックマークの保存、共有、タグ付け、発見のためのソーシャルブックマーキングウェブサービスである 。また、ユーザー間にはソーシャルネットワーキングウェブサイトを形成する相互のファン関係がある 。

- **Hi5**: [http://www.hi5.com](http://www.hi5.com) 。

- **Brightkite**: このデータセットは、ユーザーがチェックインすることで興味のある場所を共有する位置情報ベースのソーシャルネットワークサイトである 。さらに、このデータセットにはユーザーの友情活動が含まれている 。

> [!PDF|yellow] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=8&selection=51,0,52,63&color=yellow|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.8]]
> > In this subsection, several important factors are described from different perspectives that were perceived by further investigation of the trust-based social recommendation systems.
> 
> このサブセクションでは、信頼性に基づくソーシャルレコメンデーションシステムのさらなる調査によって認識された、異なる視点からのいくつかの重要な要素について説明します。

- **信頼 (Trust)**: 有向グラフにおいて、正の重みを持つエッジはユーザー間の友情を表します 。
    
- **不信 (Distrust)**: 有向グラフにおいて、負の重みを持つエッジはユーザー間の敵対関係を表します 。
    
- **明示的信頼 (Explicit trust)**: これは、信頼情報がユーザーの声明から明示的に収集されることを意味します 。明示的に収集された信頼において、ユーザーは、相互作用のある他のユーザーに対する信頼度を表明します 。
    
- **暗黙的信頼 (Implicit trust)**: これは、信頼情報がユーザーの行動から暗黙的に推論されることを意味します 。
    
- **局所的信頼 (Local trust)**: 局所的信頼は、信頼者と被信頼者間の信頼関連の過去の相互作用を示します 。
    
- **全体的信頼 (Global trust)**: 全体的信頼は、コミュニティにおける被信頼者の評判レベルを示します 。全体的信頼の計算は、ネットワーク内で他者によって提供される特定のユーザーの信頼レベル（すなわち、ユーザーノードの入次数）の組み合わせに基づいています 。
    
- **直接信頼 (Direct trust)**: これは、ユーザー間の信頼関係が非対称であるソーシャル信頼ネットワークにおいて、結果として得られるネットワークが直接信頼関係を持つことを意味します 。
    
- **間接信頼 (Indirect trust)**: これは、ユーザー間の信頼関係が対称であるソーシャル信頼ネットワークにおいて、結果として得られるネットワークが間接信頼関係を持つことを意味します 。
    
- **二値信頼 (Binary trust)**: 二値信頼は、ソーシャルネットワークユーザー間に存在する各ソーシャル信頼接続の度合いを二値で表現したものです 。
    
- **数値信頼 (Numerical trust)**: 数値信頼は、ソーシャルネットワークユーザー間の各信頼関係の強さを数値で表したものです 。
    
- **信頼伝播 (Trust propagation)**: 推移性は、局所的信頼の明白な特性です 。局所的信頼は、ソーシャル信頼ネットワーク内でユーザーからユーザーへと伝播し、暗黙的な関係を見つけることで、より正確なユーザー評価を提供します 。
    
- **信頼集約 (Trust aggregation)**: ユーザー間の信頼関係から構築されたソーシャルネットワークにおいて、信頼者ノードに入力されたすべての被信頼者パスは、全体的な効果を表すために集約されます。
- **信頼の文脈化 (Trust contextualization)**: ソーシャルネットワークユーザー間の信頼推論は、いくつかの文脈的要因を考慮することに基づいており、したがってユーザー間の信頼評価は文脈情報に従って行われます 。また、文脈自体は、存在の状態を記述するあらゆる情報となり得ます 。
    
- **信頼の時間的特性 (Trust temporalization)**: 信頼因子の時間感応性という特徴を持つため、ユーザー間の信頼推論プロセスに時間の要素を組み込みます 。
    
- **信頼のプライバシー保護 (Trust privacy-preservation)**: ソーシャルネットワークユーザー間の信頼情報に関するプライバシー漏洩と保護アプローチを考慮します。なぜなら、それは個人の友情やソーシャルサークルに関する情報を潜在的に明らかにする可能性があるからです 。

> [!PDF|red] [[Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A.pdf#page=9&selection=15,0,17,40&color=red|Int J Communication - 2023 - Vatani - Social networks data analytical approaches for trust‐based recommender systems  A, p.9]]
> > This section presents the most relevant issues and challenges of trust-based social recommender systems, its current approaches, utilized social networks data and possible recommender systems techniques. The present research thus attempted to answer the below questions:
> 
> このセクションでは、**信頼性に基づくソーシャルレコメンダーシステム**の最も関連性の高い問題と課題、現在の**アプローチ**、利用されている**ソーシャルネットワークデータ**、および考えられるレコメンダーシステムの**技術**を提示します。本研究は、以下の質問に答えようと試みました。

- **RQ1. レコメンデーションシステムの利用が増加する中、信頼関連ソーシャルネットワークデータの重要性は何ですか？** この質問の主な目的は、特定の期間に発表された信頼性に基づくソーシャルレコメンデーションシステム研究の総数を特定し、レコメンダーシステムの利用増加に加えて信頼関連ソーシャルネットワークデータの重要性を強調することでした。
    
- **RQ2. 信頼性に基づくソーシャルレコメンダーシステムにおいて、将来の方向性としてどのような問題と解決策が特定されていますか？** この質問は、信頼関連ソーシャルネットワークデータを使用するレコメンダーシステムを理解し、その課題と、データを使用して目標を改善するために利用される技術を認識することを目的としていました。
    
- **RQ3. レコメンダーシステムの主要な指標に従って、信頼関連ソーシャルネットワーク情報を利用することでレコメンダーシステムはどの程度改善されますか？** この質問は、評価目標、指標、尺度、データセットまたはベンチマーク、およびケーススタディを含む主要なレコメンダーシステム指標に基づいて、信頼関連ソーシャルネットワークデータを使用する現在のレコメンダーシステム技術を評価することを目的としていました。
    
- **RQ4. 信頼性に基づくレコメンダーシステムには、どのようなソーシャルネットワークデータ分析アプローチが含まれていますか？** この質問は、信頼性に基づくレコメンダーシステムのためのソーシャルネットワーク分析アプローチの分類を提供することを目的としていました。さらに、これらのアプローチがどれくらいの頻度で研究されたかを示しました。
    
- **RQ5. 信頼性に基づく文献において利用可能な主要な研究の発表統計は何ですか？** この質問への回答により、この分野で人気の高い出版社間の分布を特定することが可能になりました。
    
- **RQ6. 主要なレコメンダーシステム指標に従って、信頼関連ソーシャルネットワーク情報を利用することでレコメンダーシステムはどの程度改善されますか？** この質問は、信頼性に基づく分析アプローチにおける改善された指標に関する包括的な情報を取得することを目的としていました。
    
- **RQ7. 既存の信頼性に基づくソーシャルレコメンダーシステムでは、どのデータセットが使用されていますか？** この質問は、既存の信頼性に基づくソーシャルレコメンダーシステムで評価に使用されているデータセットと、どのデータセットがより多く利用されているかを特定することを目的としていました。
    
- **RQ8. 信頼性に基づくソーシャルレコメンダーシステムによって、どの利用可能なデータがより多く推奨されていますか？** この質問の目的は、信頼性に基づくソーシャルレコメンダーシステムによってより多く推奨されているデータを提示し、より必要とされるデータを特定することでした。
    
- **RQ9. ソーシャルネットワークにおいて、どの利用可能な信頼関連データがレコメンデーションに最も効果的ですか？** この質問の目的は、信頼性に基づくレコメンダーシステム技術が活用すべき、評価、タグ、ソーシャルコンタクトなどの**ソースデータ**を提示することでした。



- フェアネス
- 多様性
- LLM
- 説明可能性
