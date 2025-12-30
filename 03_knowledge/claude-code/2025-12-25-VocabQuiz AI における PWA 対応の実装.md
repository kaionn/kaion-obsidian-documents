---
date: "2025-12-25"
session_id: "test-session-ollama-2"
tags:
  - claude-code
  - PWA
  - プログレッシブウェブアプリ
  - モバイル
---

# VocabQuiz AI における PWA 対応の概要

PWA（Progressive Web App）を導入することで、VocabQuiz AI のモバイル利用体験を向上させることができます。

## PWA の主な特徴
1. **インストール可能** - ブラウザから「ホーム画面に追加」でネイティブアプリのように使える
2. **オフライン対応** - Service Worker でキャッシュし、ネット接続なしでも動作可能
3. **プッシュ通知** - ユーザーに通知を送れる（任意機能）

## VocabQuiz AI で PWA 対応するメリット
- スマホのホーム画面からワンタップで起動
- 電車やオフライン環境でも過去のクイズを復習できる
- ブラウザの URL バーが消え、フルスクリーンで没入感アップ

## 実装に必要なもの
- `manifest.json` - アプリ名、アイコン、テーマカラーの定義
- Service Worker - キャッシュ戦略の実装
- Next.js では `next-pwa` パッケージで比較的簡単に導入可能