---
date: "2025-12-30"
tags:
  - claude-code
---

# 受託開発プラン策定のためのClaude Codeコマンドの使用

## 使い方と推奨モード

### コマンド名
`/plan-with-agent`

### モード別推奨コマンド
- **ソロモード**: `/plan`
- **2エージェントモード (CursorとClaude Code)**: `/plan-with-cc`

### 使い分け
- `ソロモード`ではClaude Codeだけで計画から実行、レビューを行う。
- `2エージェントモード`ではCursorで計画を立て、Claude Codeで実行を行う。Cursorとの連携は行われない。

## 使用するスキル

- `adaptive-setup`: プロジェクト構造の初期化
- `vibecoder-guide`: VibeCoder向けガイダンス
- `core-read-repo-context`: プロジェクト状態の把握

### 実行フロー
1. **クライアント要望ヒアリング**: クライアントからの要望を確認し、エンドユーザー属性や類似サービス等を聞き出す。
2. **解像度上げ**: 要求を具体的化するための質問を3つ行う。
3. **技術調査 (自動)`: Claude Codeが調査を行い、提案を行う。