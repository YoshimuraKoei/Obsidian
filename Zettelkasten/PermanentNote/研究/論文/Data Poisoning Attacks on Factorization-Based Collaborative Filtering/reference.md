# 参考文献リスト

## 論文の主要なカテゴリ

### データポイズニング攻撃

敵対的サンプルやポイズニング攻撃に関する研究

- [ ] Biggio, B., & Roli, F. (2018). Wild patterns: Ten years after the rise of adversarial machine learning. Pattern Recognition, 84, 317-331. - 敵対的機械学習の 10 年間の進歩を概観し、データポイズニング攻撃の理論的基盤を提供
- [ ] Barreno, M., Nelson, B., Sears, R., Joseph, A. D., & Tygar, J. D. (2006). Can machine learning be secure? In Proceedings of the 2006 ACM Symposium on Information, computer and communications security (pp. 16-25). - 機械学習システムのセキュリティ問題を体系的に分析
- [ ] Nelson, B., Barreno, M., Chi, F. J., Joseph, A. D., Rubinstein, B. I., Saini, U., ... & Tygar, J. D. (2008). Exploiting machine learning to subvert your spam filter. LEET, 8, 1-9. - スパムフィルターに対する攻撃手法を提案

### 協調フィルタリング

推薦システムと協調フィルタリングに関する研究

- [ ] Koren, Y., Bell, R., & Volinsky, C. (2009). Matrix factorization techniques for recommender systems. Computer, 42(8), 30-37. - 推薦システムのための行列因子分解手法を包括的に解説
- [ ] Su, X., & Khoshgoftaar, T. M. (2009). A survey of collaborative filtering techniques. Artificial intelligence review, 31(1), 19-25. - 協調フィルタリング手法の包括的なサーベイ
- [ ] Ricci, F., Rokach, L., & Shapira, B. (2015). Introduction to recommender systems handbook. In Recommender systems handbook (pp. 1-35). Springer. - 推薦システムの基礎理論と実践的応用

### 行列因子分解と最適化

行列補完と最適化アルゴリズムに関する研究

- [7] Candès, E. J., & Recht, B. (2009). Exact matrix completion via convex optimization. Foundations of Computational mathematics, 9(6), 717-772. - 凸最適化による行列補完の理論的基盤
- [8] Recht, B., Fazel, M., & Parrilo, P. A. (2010). Guaranteed minimum-rank solutions of linear matrix equations via nuclear norm minimization. SIAM review, 52(3), 471-501. - 核ノルム最小化による行列補完の理論
- [9] Jain, P., Netrapalli, P., & Sanghavi, S. (2013). Low-rank matrix completion using alternating minimization. In Proceedings of the forty-fifth annual ACM symposium on Theory of computing (pp. 665-674). - 交互最小化による低ランク行列補完

### ロバスト性と防御

機械学習システムのロバスト性と防御手法に関する研究

- [10] Madry, A., Makelov, A., Schmidt, L., Tsipras, D., & Vladu, A. (2017). Towards deep learning models resistant to adversarial attacks. arXiv preprint arXiv:1706.06083. - 敵対的攻撃に対して堅牢なディープラーニングモデルの構築
- [11] Papernot, N., McDaniel, P., Jha, S., Fredrikson, M., Celik, Z. B., & Swami, A. (2016). The limitations of deep learning in adversarial settings. In 2016 IEEE European symposium on security and privacy (EuroS&P) (pp. 372-387). - ディープラーニングの敵対的設定における限界を分析
- [12] Goodfellow, I. J., Shlens, J., & Szegedy, C. (2014). Explaining and harnessing adversarial examples. arXiv preprint arXiv:1412.6572. - 敵対的サンプルの生成と防御の理論

### 推薦システムのセキュリティ

推薦システムのセキュリティとプライバシーに関する研究

- [13] Lam, S. K., & Riedl, J. (2004). Shilling recommender systems for fun and profit. In Proceedings of the 13th international conference on World Wide Web (pp. 393-402). - 推薦システムに対するシリング攻撃の分析
- [14] O'Mahony, M. P., Hurley, N. J., & Silvestre, G. C. (2006). Detecting noise in recommender system databases. In Proceedings of the 11th international conference on Intelligent user interfaces (pp. 109-115). - 推薦システムデータベースのノイズ検出
- [15] Burke, R., Mobasher, B., Bhaumik, R., & Williams, C. (2005). Segment-based injection attacks against collaborative filtering recommender systems. In Fifth IEEE international conference on data mining (ICDM'05) (pp. 4-pp). - 協調フィルタリング推薦システムに対するセグメントベース攻撃

### その他：関連分野の研究

- [16] Li, B., Wang, Y., Singh, A., & Vorobeychik, Y. (2016). Data poisoning attacks on factorization-based collaborative filtering. Advances in Neural Information Processing Systems, 29, 1885-1893. - 本論文：因子分解ベース協調フィルタリングに対するデータポイズニング攻撃
- [17] McSherry, F., & Mironov, I. (2009). Differentially private recommender systems: Building privacy into the netflix prize contenders. In Proceedings of the 15th ACM SIGKDD international conference on Knowledge discovery and data mining (pp. 627-636). - 差分プライバシーを考慮した推薦システム
- [18] Berkovsky, S., Eytani, Y., Kuflik, T., & Ricci, F. (2007). Enhancing privacy and preserving accuracy of a distributed collaborative filtering. In Proceedings of the 2007 ACM conference on Recommender systems (pp. 9-16). - 分散協調フィルタリングにおけるプライバシー保護

## 研究の影響と発展

### 主要な引用論文

この論文は以下の分野で重要な影響を与えている：

1. **敵対的機械学習**：データポイズニング攻撃の理論的基盤を提供
2. **推薦システムセキュリティ**：協調フィルタリングシステムの脆弱性を初めて体系的に分析
3. **行列因子分解**：交互最小化と核ノルム最小化の両方に対する攻撃手法を開発

### 後続研究への影響

- 推薦システムのセキュリティ研究の新たな方向性を開拓
- 敵対的機械学習の推薦システムへの応用を促進
- 防御手法の開発を刺激
