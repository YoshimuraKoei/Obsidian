---
title:
AI:
published: "2022"
created: " 2025-10-27"
description:
tags:
  - knowledge
  - recommend
  - github
---

## ディレクトリ構成


> [!NOTE] 主要ディレクトリ構成とその役割
> 1. `reco_utils` ：**レコメンダー実装のコアライブラリ**
> 	- `recommender/deeprec/` ：深層学習ベースのレコメンダー
> 		- `models/sequential/` ：シーケンシャル (動作履歴) モデル
> 			- `clsr.py` ：この研究のCLSRモデル
> 			- `gru4rec.py`, `din.py`, `dien.py`など：参考モデル
> 		- `config/`：モデル設定ファイル(YAML)
> 			- `clsr.yaml` ：CLSR設定
> 		- `io/` ：データ読み込み (イテレータなど)
> 	- `dataset/` ：前処理・データ分析
> 		- `sequential_reviews.py` ：シーケンシャルデータ前処理
> 		- `python_splitters.py` ：分割
> 	- `evaluaton/` ：評価 (AUC、NDCGなど)
> 	- `common/` ：ユーティリティ
> 2. `tests/resources/` ：データセット
> 	- `deeprec/sequential/` ：
> 		- `taobao/`, `tenrec` など
> 		- `README.md` ：データ説明
> 3. `examples/00_quick_start/` ：学習・評価スクリプト実行コード
> 	- `sequential.py` ：モデル選択と実行のエントリーポイント
> 		- データロード、モデル初期化、学習、評価
> 		- taobao / Kuaishou で実行
> 	- `CLSR/taobao-clsr-debug/` ：出力
> 		- `model/` ：チェックポイント
> 		- `summary/` ：学習ログ


```
.
├── analysis
│   ├── eda_taobao.ipynb
│   └── eda_tenrec.ipynb
├── docs
│   └── README.md
├── examples
│   └── 00_quick_start
│       ├── CLSR
│       │   └── taobao-clsr-debug
│       │       ├── model
│       │       │   ├── checkpoint
│       │       │   ├── epoch_1.data-00000-of-00001
│       │       │   ├── epoch_1.index
│       │       │   ├── epoch_1.meta
│       │       │   ├── epoch_2.data-00000-of-00001
│       │       │   ├── epoch_2.index
│       │       │   ├── epoch_2.meta
│       │       │   ├── epoch_3.data-00000-of-00001
│       │       │   ├── epoch_3.index
│       │       │   └── epoch_3.meta
│       │       ├── summary
│       │       │   └── events.out.tfevents.1758041389.ae658715b166
│       │       ├── model.tar.gz
│       │       └── README.md
│       └── sequential.py
├── reco_utils
│   ├── __pycache__
│   │   ├── __init__.cpython-313.pyc
│   │   └── __init__.cpython-36.pyc
│   ├── common
│   │   ├── __pycache__
│   │   │   ├── __init__.cpython-36.pyc
│   │   │   └── constants.cpython-36.pyc
│   │   ├── __init__.py
│   │   ├── constants.py
│   │   ├── general_utils.py
│   │   ├── gpu_utils.py
│   │   ├── notebook_memory_management.py
│   │   ├── notebook_utils.py
│   │   ├── plot.py
│   │   ├── python_utils.py
│   │   ├── spark_utils.py
│   │   ├── tf_utils.py
│   │   └── timer.py
│   ├── dataset
│   │   ├── __pycache__
│   │   │   ├── __init__.cpython-313.pyc
│   │   │   ├── __init__.cpython-36.pyc
│   │   │   ├── download_utils.cpython-313.pyc
│   │   │   ├── download_utils.cpython-36.pyc
│   │   │   ├── sequential_reviews.cpython-313.pyc
│   │   │   └── sequential_reviews.cpython-36.pyc
│   │   ├── __init__.py
│   │   ├── blob_utils.py
│   │   ├── covid_utils.py
│   │   ├── download_utils.py
│   │   ├── pandas_df_utils.py
│   │   ├── python_splitters.py
│   │   ├── sequential_reviews.py
│   │   ├── spark_splitters.py
│   │   ├── sparse.py
│   │   ├── split_utils.py
│   │   └── wikidata.py
│   ├── evaluation
│   │   ├── __init__.py
│   │   ├── python_evaluation.py
│   │   └── spark_evaluation.py
│   ├── recommender
│   │   ├── __pycache__
│   │   │   ├── __init__.cpython-313.pyc
│   │   │   └── __init__.cpython-36.pyc
│   │   ├── deeprec
│   │   │   ├── __pycache__
│   │   │   │   ├── __init__.cpython-313.pyc
│   │   │   │   ├── __init__.cpython-36.pyc
│   │   │   │   ├── deeprec_utils.cpython-313.pyc
│   │   │   │   └── deeprec_utils.cpython-36.pyc
│   │   │   ├── config
│   │   │   │   ├── asvd.yaml
│   │   │   │   ├── caser.yaml
│   │   │   │   ├── clsr.yaml
│   │   │   │   ├── dien.yaml
│   │   │   │   ├── din.yaml
│   │   │   │   ├── gru4rec.yaml
│   │   │   │   ├── lgn.yaml
│   │   │   │   ├── lightgcn.yaml
│   │   │   │   ├── ncf.yaml
│   │   │   │   ├── nextitnet.yaml
│   │   │   │   └── sli_rec.yaml
│   │   │   ├── DataModel
│   │   │   │   └── ImplicitCF.py
│   │   │   ├── io
│   │   │   │   ├── __pycache__
│   │   │   │   │   ├── __init__.cpython-36.pyc
│   │   │   │   │   ├── iterator.cpython-36.pyc
│   │   │   │   │   └── sequential_iterator.cpython-36.pyc
│   │   │   │   ├── __init__.py
│   │   │   │   ├── dkn_iterator.py
│   │   │   │   ├── iterator.py
│   │   │   │   ├── nextitnet_iterator.py
│   │   │   │   └── sequential_iterator.py
│   │   │   ├── models
│   │   │   │   ├── __pycache__
│   │   │   │   │   ├── __init__.cpython-36.pyc
│   │   │   │   │   └── base_model.cpython-36.pyc
│   │   │   │   ├── sequential
│   │   │   │   │   ├── __pycache__
│   │   │   │   │   │   ├── asvd.cpython-36.pyc
│   │   │   │   │   │   ├── caser.cpython-36.pyc
│   │   │   │   │   │   ├── clsr.cpython-36.pyc
│   │   │   │   │   │   ├── dien.cpython-36.pyc
│   │   │   │   │   │   ├── din.cpython-36.pyc
│   │   │   │   │   │   ├── gru4rec.cpython-36.pyc
│   │   │   │   │   │   ├── lgn.cpython-36.pyc
│   │   │   │   │   │   ├── ncf.cpython-36.pyc
│   │   │   │   │   │   ├── rnn_cell_implement.cpython-36.pyc
│   │   │   │   │   │   ├── rnn_dien.cpython-36.pyc
│   │   │   │   │   │   ├── sequential_base_model.cpython-36.pyc
│   │   │   │   │   │   └── sli_rec.cpython-36.pyc
│   │   │   │   │   ├── asvd.py
│   │   │   │   │   ├── caser.py
│   │   │   │   │   ├── clsr.py
│   │   │   │   │   ├── dien.py
│   │   │   │   │   ├── din.py
│   │   │   │   │   ├── gru4rec.py
│   │   │   │   │   ├── lgn.py
│   │   │   │   │   ├── ncf.py
│   │   │   │   │   ├── nextitnet.py
│   │   │   │   │   ├── rnn_cell_implement.py
│   │   │   │   │   ├── rnn_dien.py
│   │   │   │   │   ├── sequential_base_model.py
│   │   │   │   │   └── sli_rec.py
│   │   │   │   ├── __init__.py
│   │   │   │   └── base_model.py
│   │   │   ├── __init__.py
│   │   │   └── deeprec_utils.py
│   │   └── __init__.py
│   ├── __init__.py
│   └── README.md
├── tests
│   ├── resources
│   │   └── deeprec
│   │       └── sequential
│   │           ├── retailrocket
│   │           │   ├── archive
│   │           │   │   ├── category_tree.csv
│   │           │   │   ├── events.csv
│   │           │   │   ├── item_properties_part1.csv
│   │           │   │   └── item_properties_part2.csv
│   │           │   ├── archive.zip
│   │           │   └── archive.zipZone.Identifier
│   │           ├── taobao
│   │           │   ├── category_vocab.pkl
│   │           │   ├── instance_output
│   │           │   ├── instance_output_1.0
│   │           │   ├── item_vocab.pkl
│   │           │   ├── original_test_data
│   │           │   ├── original_train_data
│   │           │   ├── preprocessed_output
│   │           │   ├── taobao_business_recommenders.csv
│   │           │   ├── taobao_review_recommenders.csv
│   │           │   ├── user_vocab.pkl
│   │           │   ├── UserBehavior.csv
│   │           │   └── valid_data
│   │           ├── Tenrec
│   │           │   ├── __MACOSX
│   │           │   │   └── Tenrec
│   │           │   │       ├── ._.DS_Store
│   │           │   │       └── ._Readme.txt
│   │           │   └── Tenrec
│   │           │       ├── .DS_Store
│   │           │       ├── cold_data_0.3.csv
│   │           │       ├── cold_data_0.7.csv
│   │           │       ├── cold_data_1.csv
│   │           │       ├── cold_data.csv
│   │           │       ├── ctr_data_1M.csv
│   │           │       ├── QB-article.csv
│   │           │       ├── QB-video.csv
│   │           │       ├── QK-article.csv
│   │           │       ├── QK-video.csv
│   │           │       ├── Readme.txt
│   │           │       ├── sbr_data_1M.csv
│   │           │       ├── task_0.csv
│   │           │       ├── task_1.csv
│   │           │       ├── task_2.csv
│   │           │       ├── task_3.csv
│   │           │       └── test_write.tmp
│   │           ├── README.md
│   │           ├── taobao.tar.gz
│   │           ├── taobao.tar.gzZone.Identifier
│   │           ├── Tenrec.zip
│   │           └── Tenrec.zipZone.Identifier
│   └── __init__.py
├── .gitignore
├── Dockerfile
├── LICENSE
├── README.md
└── コマンド.md
```


## コード読み解き


> [!NOTE] 順番
> 1. 設定ファイル(5分)
> 2. エントリーポイント(15分)
> 3. データ前処理(30分)
> 4. モデル実装(1~2時間)

