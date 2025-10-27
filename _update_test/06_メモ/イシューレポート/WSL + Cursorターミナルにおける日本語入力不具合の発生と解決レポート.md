---
title:
AI:
published:
created: " 2025-10-27"
description:
tags:
  - permanent-note
  - knowledge
  - wsl
  - Cursor
---

# 🎯 **WSL + Cursorターミナルにおける日本語入力不具合の発生と解決レポート**

## 📌 1. 問題の概要

- **現象**：Cursor（VSCodeベースのAIエディタ）の統合ターミナル上で、日本語入力を行うと文字が意図しない位置に確定されたり、入力途中で確定される（例：`yamlファ` のようになってしまう）
- **発生環境**：
    - OS：Windows 10/11        
    - ターミナル：Cursor 統合ターミナル（WSL使用）
    - 入力方式：Windows IME
- **影響**：YAML、ファイル名、ディレクトリ名などの入力が困難になり、作業効率が著しく低下

---
## 🔍 2. 原因分析

調査の結果、以下の要因が重なって問題を引き起こしていたと考えられる：

|要因|内容|
|---|---|
|Terminalが旧API（WinPTY）で動作|Windows 10以前のターミナル通信方式を使用しておりIMEとの整合性が悪い|
|CursorはVSCodeと設定体系が異なる|`rendererType` のような従来のキーが存在しないため、設定が見つからなかった|
|Windows IMEとの相性問題|新IMEとターミナル仮想レイヤーの組み合わせにより、入力バグが発生|

---

## 🛠️ 3. 解決に至った方法

### ✅ 有効だった設定変更：

**Terminal > Integrated: Windows Use Conpty DLL** を有効化

### 📌 手順：

1. Cursor の設定を開く（`Ctrl + ,`）
2. 検索窓に `ConPTY` と入力
3. 以下の項目を確認：
    - ✅ **Windows Enable ConPTY**：ON（デフォルトでON）
    - ✅ **Windows Use Conpty DLL（Preview）**：**ONに設定**
4. Cursor を再起動
5. ターミナルで日本語入力が正常化したことを確認

---

## ✨ 4. 効果

- 入力途中の日本語が勝手に確定される問題が解消
- カーソル位置のズレがなくなり、ファイル操作やCLI入力がスムーズに
- 作業ストレスが大幅に低減

---

## 📚 5. 補足オプション（再発防止・他環境での再現時対策）

|対策|説明|推奨度|
|---|---|---|
|Windows IME互換モードON|IME設定 →「以前のバージョンのIMEを使用」|⭐⭐⭐⭐|
|Windows Terminalを使う|統合ターミナルではなく外部ターミナルでWSLを操作|⭐⭐⭐⭐⭐（最強）|
|renderer設定変更（VSCodeの場合）|`"terminal.integrated.rendererType": "dom"`|Cursorでは不使用|

---

## ✅ 6. 結論

本事象は **CursorターミナルとWindows IMEの互換性の問題**に起因しており、  
**ConPTY DLLの有効化により改善された。**

CursorやVSCode系のターミナルで同様の症状が発生した場合、  
**ConPTYの設定を最優先で確認すべきである。**

---

## 📝 7. 今後の運用上の推奨事項

- Cursorアップデート後も再度設定がリセットされる可能性に注意
- 他の開発マシンでも再現する場合は、本レポートをテンプレとして展開
- IME入力が不安定な場合は、まず「ConPTY設定 → IME互換モード → 外部ターミナル」の順で対応する