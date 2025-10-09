---
title: ABC_Abstract Base Classes
AI: "false"
published: 2025-07-28
description: 
tags:
  - permanent-note
---
---
# ABC（Abstract Base Classes）の基礎知識
## 概要

`abc`は Python の標準ライブラリで、抽象クラスを定義するために使用される。
  
## 主な役割  

### 1. 抽象クラスの定義

```python

from abc import ABC, abstractmethod

class BaseRecommender(ABC):  # ABCを継承
    pass

```

### 2. 抽象メソッドの強制

```python

@abstractmethod
def recommend(self, dataset: Dataset) -> RecommendResult:
    pass

```

  

## なぜ使うのか？

### 設計の統一

- 全ての子クラスが同じインターフェースを持つことを保証 
- コードの一貫性を維持  

### 実装の強制

- 子クラスで必ず実装しなければならないメソッドを定義
- 忘れやミスを防ぐ

### 統一されたインターフェース

```python

# 異なる実装でも同じ方法で呼び出せる

collaborative = CollaborativeFiltering()
content_based = ContentBasedFiltering() 

result1 = collaborative.recommend(dataset)
result2 = content_based.recommend(dataset)

```

## 実際の効果

### 抽象メソッドなし（問題のある例）
```python

class BaseRecommender:
    def some_method(self):
        pass

class MyRecommender(BaseRecommender):
    # recommendメソッドを忘れてしまった！
    pass

  

# エラーが発生するまで気づかない

```

### 抽象メソッドあり（安全な例）


```python

class BaseRecommender(ABC):
    @abstractmethod
    def recommend(self, dataset: Dataset) -> RecommendResult:
        pass

  

class MyRecommender(BaseRecommender):
    # recommendメソッドを実装しないとエラーになる
    pass

# → TypeError: Can't instantiate abstract class MyRecommender with abstract method recommend

```
## まとめ

`@abstractmethod`は**「必ず実装してください」という設計上の約束事**を強制する仕組み。コードの品質と一貫性を保つために重要な役割を果たす。