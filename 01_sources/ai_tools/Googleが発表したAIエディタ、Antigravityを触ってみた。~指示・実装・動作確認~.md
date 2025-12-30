---
title: "Googleが発表したAIエディタ、Antigravityを触ってみた。~指示・実装・動作確認~"
source: "https://qiita.com/yokko_mystery/items/bb5615ebcd385a597c41"
author:
  - "[[yokko_mystery]]"
published: 2025-11-19
created: 2025-11-21
description: "はじめに Google が 2025-11-18（米国時間）に発表した AI エディタ、「Antigravity」を使って、実際に「AIチャットアプリ」を開発してみました。 本記事では、拡張機能である Antigravity Browser Extension を利用した..."
tags:
  - ai-development
  - ai-tools
  - japanese
  - qiita
  - review
  - tutorial
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=142839)

## はじめに

Google が 2025-11-18（米国時間）に発表した AI エディタ、「 [**Antigravity**](https://antigravity.google/) 」を使って、実際に「AIチャットアプリ」を開発してみました。  
本記事では、拡張機能である [Antigravity Browser Extension](https://chromewebstore.google.com/detail/antigravity-browser-exten/eeijfnjmjelapkebgockoeaadonbchdd) を利用した体験も踏まえて簡潔にまとめています。

Chrome拡張機能を利用したUI修正のデモ動画  
[![Antigravity_UI_fix_1.gif](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F243d5dd9-14b3-4ef7-a104-651c19dc05e4.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=03ad2147161ca912d97b0473a0492194)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F243d5dd9-14b3-4ef7-a104-651c19dc05e4.gif?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=03ad2147161ca912d97b0473a0492194)

## Google Antigravity とは何か

Google が 2025-11-18（米国時間）に発表した、 **エージェントファースト（agent-first）の開発プラットフォーム** です。  
開発者が直接コードを書く “従来の IDE” ではなく、AI エージェントを指揮・監督し、エージェントが自律的に「計画 → 実装 →検証」までを遂行する新たなワークフローを実現しています。

### 主な特徴

- **エージェントファースト開発体験**  
	開発者は“指示”を出すだけで、エージェントが計画を立て、コードを記述・実行・検証します。エディタ・ターミナル・ブラウザを横断してタスクを自律的に遂行する点が特徴です。
- **マネージャービューとエディタビュー**  
	「マネージャービュー」では、複数ワークスペース・複数エージェントを一元管理し、各エージェントの進行状況や成果物を俯瞰できます。  
	「エディタビュー」では、従来のコード編集画面に近い操作感の中で、エージェントとの対話や修正も行えます。
- **作業の可視化（Artifacts）**  
	エージェントが生成する「タスクリスト」「実装プラン」「スクリーンショット／録画」「コード差分」などの成果物を自動で保存・表示。開発者は作業の履歴や検証を容易に確認でき、信頼性の高い “検証可能な成果物” を得られます。

詳細は「 [**Antigravity**](https://antigravity.google/) 」を参照ください。

## 実践

### 1.インストール/起動

Antigravity は [公式サイト](https://antigravity.google/) からダウンロードできます。諸々の設定をして起動。VSCode のフォークなので見慣れた画面構成です。

[![スクリーンショット 2025-11-19 9.02.01.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/7a23536a-71b3-48d1-9120-62eee33affe1.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F7a23536a-71b3-48d1-9120-62eee33affe1.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=72c2af28226a0527224da65251b38011)

Model は下記から選択可能です。　もちろん、Gemini 3 も利用できます。

[![スクリーンショット 2025-11-19 11.52.18.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/701a0a91-973e-4ee7-b33d-730ad84c5c4d.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F701a0a91-973e-4ee7-b33d-730ad84c5c4d.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fed3edcc291f92ad0d9688c08c1b9bf5)

`Conversation Mode` では、以下の2つのモードを切り替えることができます。

- **Planning**: 複雑なタスクや調査、共同作業向け。実行前に計画を立てます。
- **Fast**: シンプルなタスク向け。直接タスクを実行し、素早く完了させます。

[![スクリーンショット 2025-11-19 12.11.55.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/1d2bafb9-be7f-413f-8f4b-34f22ff3a90a.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F1d2bafb9-be7f-413f-8f4b-34f22ff3a90a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=419e15da5023ad37f753c15e25f1be27)

### 2.「Open Agent Manager」から「Agent Manager」を起動

Antigravity には複数エージェントや複数ワークスペースを束ねて管理できる「Agent Manager」が存在します。  
ここから新規プロジェクトを開始したり、別ワークスペースのタスク状況を一覧で確認できます。

[![スクリーンショット 2025-11-19 9.02.29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/5d3ed875-5d46-422f-98e9-9714c957783e.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F5d3ed875-5d46-422f-98e9-9714c957783e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6a09be45e5cabbd3ed6099baab65d84b)

### 3.指示を入力

エージェントに対し以下の 1 文だけを入力しました。

> 「AI ハムスターと会話できるチャットアプリを作成してください。」

するとエージェントは下記を実行しました。

- プロジェクト初期化
- Next.js + Tailwind CSS の構成生成
- 画面レイアウト作成
- Chat UI コンポーネント構築
- API Route による簡易チャットロジックの生成
- ローカル開発サーバーの起動

[![スクリーンショット 2025-11-19 9.05.18.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/07ea2a5d-4bc5-43b0-bb86-aba5dbdcec3e.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F07ea2a5d-4bc5-43b0-bb86-aba5dbdcec3e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=779d34d9d9b8d7ab086bcb895f50892c)

また、左側ではリアルタイムに "実際にエージェントが実行している内容" を確認でき、  
右側の **Task パネル** ではエージェントが自律的に作成した *タスク分解* がチェックリストとして表示されます。  
どの工程を実行しているのかが一目で分かります。

※AIチャットに必要なGEMINI\_API\_KEYの設定だけは手作業で行いました。

### 4.実装が完了すると Chrome でアプリが起動

この時に Chrome 拡張機能である [Antigravity Browser Extension](https://chromewebstore.google.com/detail/antigravity-browser-exten/eeijfnjmjelapkebgockoeaadonbchdd) をインストールしました。

視覚的フィードバックやユースケースについては公式サイトの下記もご参考ください。  
[Why frontend developers choose Google Antigravity](https://antigravity.google/use-cases/frontend?_gl=1*1fqz6ni*_up*MQ..*_ga*MTg4NzUxODgwMi4xNzYzNTIwMTI1*_ga_47V54ZJ3EV*czE3NjM1MjAxMjUkbzEkZzAkdDE3NjM1MjAxMjUkajYwJGwwJGgw)

実際にチャットができるか試してみるとちゃんと応答が返ってきます。

[![スクリーンショット 2025-11-19 9.35.45.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/30f836a9-43e2-4e1f-9e8a-6281289e6d31.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F30f836a9-43e2-4e1f-9e8a-6281289e6d31.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=40a3fb3f1fc8d697b013c0a3b8f3d1a1)

### 5.追加の指示を入力

> 「AI キャラクターの名前をハム次郎にしてください。途中経過をスクショで記録してください」

Chrome 上でアプリが起動し、修正していく様子をリアルタイムで確認できました。  
また、Playback available と表示されていたので「View」をクリックすると右側で AI が実施した実際の動作確認のキャプチャを見ることができました。  
このキャプチャはArtifactとして保存されています。

**Editor／Terminal／Browserをまたぐ作業** が実現されていることがわかります。

[![スクリーンショット 2025-11-19 9.18.45.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/9589cd28-8159-436c-9e47-6975ae33ff27.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F9589cd28-8159-436c-9e47-6975ae33ff27.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=4a1b5953908acbe8325a4beac31e1eea)

追加の実装が完了すると、Walkthrough が作成され、指示した通りにキャプチャも一緒に保存してくれました。

[![スクリーンショット 2025-11-19 9.19.04.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/56fa99bc-6919-4553-bf38-d42298c70f7f.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F56fa99bc-6919-4553-bf38-d42298c70f7f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=a43ea98d816e540659d8409d6693411c)

### 6.ArtifactからUIの修正を依頼

2025/11/19 19:58 追記

作成したArtifactは画面の右に一覧表示することが可能です。  
UI上で修正したい箇所がある場合、そのArtifactの画面キャプチャを選択、変更したい箇所をドラッグし、コメントをすることでその要望が反映されます。

**①変更したい箇所をドラッグ選択**  
[![スクリーンショット 2025-11-19 19.12.46.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/13f9791f-a0e3-4d80-9649-9d441f133b6f.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F13f9791f-a0e3-4d80-9649-9d441f133b6f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=bcf08cc42d53e4225a024e187a7b7b8d)

**②変更したい内容をコメント**  
絵文字の表示位置についてコメント  
[![スクリーンショット 2025-11-19 19.13.08.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/32fb1bba-cdc9-4625-8e3b-15b12adf6224.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F32fb1bba-cdc9-4625-8e3b-15b12adf6224.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=881a437aafcf1f4f5f380cfeec88a408)

吹き出しの色も変えてほしいのでコメント  
[![スクリーンショット 2025-11-19 19.13.29.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/fb886da5-1740-4dc4-b59d-f48be048da3b.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2Ffb886da5-1740-4dc4-b59d-f48be048da3b.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=5d1492e8f4039bcb4293ab5e7805bb27)

**③コメント内容通りに実装される！**  
[![スクリーンショット 2025-11-19 19.14.07.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/c3c321bf-5fb5-4999-ba69-39c5d0a9c614.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2Fc3c321bf-5fb5-4999-ba69-39c5d0a9c614.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7ad7396ec701e836d9826f8e945031d7)

2025/11/19 23:50 追記

ドラッグして対象箇所のUIを修正する方法ですが、生成された単一の画像にも利用でき、綺麗に修正してくれるようです。  
カップケーキの箇所を選択、コメントを入力して修正依頼を実施  
[![スクリーンショット 2025-11-19 23.52.45.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/1306cd84-c22d-4ebe-a963-32b334e27a45.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F1306cd84-c22d-4ebe-a963-32b334e27a45.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6afaf9823ac4425c988f9af18ab665f7)

なお、Antigravity の画像生成や画面モック制作はNanoBanana（Gemini 2.5 Flash Image） モデルが担当しているとのことです。

[Welcome to Google Antigravity 🚀](https://www.youtube.com/watch?v=SVCBA-pBgt0)  
→1:48-1:52

> Nano banana: Used by the generative image tool, when the Agent wants to produce a UI mockup or needs images to populate a web page or application.  
> Gemini 2.5 Pro UI Checkpoint: Used by the browser subagent to actuate the browser, such as clicking, scrolling, or filling in input.  
> Gemini 2.5 Flash: Used in the background for checkpointing and context summarization.  
> Gemini 2.5 Flash Lite: Used by the codebase semantic search tool.

### 7.Task と Walkthrough の保存先について

Antigravity では、エージェントが実行したタスクや生成された Walkthrough は、ユーザーのホームディレクトリ配下の隠しフォルダ `.gemini` 内に自動的に保存されます。

具体的には、以下のパスに保存されていることが確認できました。

- **保存場所**: `~/.gemini/antigravity/brain/<session_id>/`
	- `task.md`: タスクリストと進捗状況
	- `walkthrough.md`: 作業の記録（スクリーンショットや録画へのリンク含む）
	- `implementation_plan.md`: 実装計画書

これらのファイルは Markdown 形式で保存されているため、エディタ外でもテキストとして確認したり、Git で管理することも可能です

[![スクリーンショット 2025-11-19 12.07.50.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/42718f9b-3545-4eb4-a17b-73a731511cc8.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F42718f9b-3545-4eb4-a17b-73a731511cc8.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=26ba74dd51e7d173c8950689fc04e5a1)

キャプチャについては `~/.gemini/antigravity/browser_recordings/<session_id>/` にありました。

[![スクリーンショット 2025-11-19 12.09.48.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/581663/963c5ac7-dc47-43ee-84ee-f8ed89c60872.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F581663%2F963c5ac7-dc47-43ee-84ee-f8ed89c60872.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=d220f95544cda6e6c822fd2327c106be)

もちろん、Antigravity 上の **Task パネル** や **Agent Manager** からもこれらを視覚的に確認・管理できます。

## 気づき・良かった点

- **タスクの可視化がスムーズ**:  
	エージェントの進捗が一目でわかる。Task パネルでタスク分解の様子がリアルタイムで確認でき、どの工程を実行しているのかが明確。
- **自動化された作業フロー**:  
	ブラウザ操作・スクリーンショット取得・Walkthrough 生成など、通常手作業で煩雑な工程が補助される感覚。エージェントが自律的にテストや検証まで行ってくれる。
- **作業履歴の保存**:  
	Task や Walkthrough が自動的に保存され、後から確認できる。複数のプロジェクトやワークスペースを Agent Manager で一元管理できる。
- **シンプルな指示で完結**:  
	1 文の指示だけで、プロジェクト初期化から開発サーバー起動まで自動で実行される。技術的な詳細を気にせず、やりたいことだけを伝えれば良い。
- **視覚的なフィードバックが強い**  
	ブラウザ操作や UI の変化がスクショや録画で即確認でき、ドラッグ＋コメントだけで UI 修正を依頼できる。

## おわりに

AI エージェントが何をやっているのかの確認が格段にわかりやすくなったと感じます。  
タスク分解と進捗状況がリアルタイムで反映されていくのはみていて気持ちよかったです。

フロントエンド開発においては、ブラウザ操作やスクリーンショット取得など、視覚的なフィードバックが重要な工程も自動化されるため、開発効率が向上する可能性はあるなと感じました。

まだ機能を使いきれていないところもあるため、引き続き普段使いしているCursorなどと比較しながら使用してみようと思います。

皆さんもぜひ、 [Antigravity](https://antigravity.google/) を一度試してみて、ご自身の開発ワークフローにどう馴染むかを検証してみてください。

## 参考資料

- [Antigravity - 公式サイト](https://antigravity.google/) - Google の AI エージェントファースト開発プラットフォーム
- [Why frontend developers choose Google Antigravity](https://antigravity.google/use-cases/frontend) - フロントエンド開発者向けのユースケース
- [A new era of intelligence with Gemini 3 – Google Blog](https://blog.google/products/gemini/gemini-3/) - Gemini 3 の発表と Antigravity の紹介
- [Google Antigravity introduces agent-first architecture for asynchronous, verifiable coding workflows – VentureBeat](https://venturebeat.com/ai/google-antigravity-introduces-agent-first-architecture-for-asynchronous) - Antigravity のアーキテクチャについての解説
- [Antigravity Is Google's New Agentic Development Platform – The New Stack](https://thenewstack.io/antigravity-is-googles-new-agentic-development-platform/) - プラットフォームとしての Antigravity の詳細

[0](https://qiita.com/yokko_mystery/items/#comments)

コメント一覧へ移動

X（Twitter）でシェアする

Facebookでシェアする

はてなブックマークに追加する

## @yokko\_mysteryのピックアップ記事

### Googleが発表したAIエディタ、Antigravityを触ってみた。~指示・実装・動作確認~

194いいね

投稿日2025年11月19日

[

## @yokko\_mystery(Yokko)

](https://qiita.com/yokko_mystery)

SIer3年→iOSエンジニアを1年→会社員兼個人事業主 アプリ作ってます。

[Web](https://mysterylog.com/) [RSS](https://qiita.com/yokko_mystery/feed)

## Qiita Advent Calendar 開催！

[194](https://qiita.com/yokko_mystery/items/bb5615ebcd385a597c41/likers)

いいねしたユーザー一覧へ移動

105