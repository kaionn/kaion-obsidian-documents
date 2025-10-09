---
title: ObsidianとClaude Codeを使ったドキュメント活用
source: https://zenn.dev/oikon/articles/obsidian-claude-code
author:
  - "[[Zenn]]"
published: 2025-08-15
created: 2025-10-09
description:
tags:
  - article
  - claude-code
  - context-engineering
  - documentation
  - japanese
  - knowledge-management
  - mcp
  - mcp-server
  - obsidian
  - productivity
  - tutorial
  - web-clipper
  - workflow
  - zenn
  - ドキュメント管理
  - ナレッジ管理
  - ワークフロー
  - 技術記事
---
202

121

Oikonです。外資系IT企業でエンジニアをしています。

AIエージェントのコンテキストエンジニアリングが最近は注目を集めています。今後もしばらくはこの流れが続くんじゃないかと。

AIエージェントにコンテキストを与える仕組みのために、 **知識の源泉としてObsidianを3ヶ月ほど前から試行錯誤しながら使っています** 。

最近になって、ようやく自分の中でしっくり来る運用が固まってきたので、その方法を共有します。

この記事は、先日Xにポストした内容の詳細版です。

Obsidianの運用方法は人によって全く違うと思うので、「こんな運用方法もあるのね」くらいに読んでいただければ幸いです。

## 運用の流れ

まず以下が揃っていることを前提とします。

- [Obsidian Desktop](https://obsidian.md/)
- [Obsidian Mobile](https://apps.apple.com/jp/app/obsidian-connected-notes/id1557175442?l=en-US) (Optional)
- [Claude Code](https://docs.anthropic.com/ja/docs/claude-code/overview)

クリックで、それぞれの公式サイトに飛びます。

はじめに、Obsidianの運用の全体像を紹介します。現在の運用手順は以下の通りです。

1. **Obsidian Vaultをデバイス間で共有**
2. **Obsidian Web Clipperで記事を保存**
3. **Obsidianのフォルダ構成はシンプルに**
4. **Clippings/をClaude Codeで整理**
5. **Claude CodeとObsidian MCPサーバーを繋ぐ**
6. **Claude CodeからObsidianを活用する**

それぞれについて詳しく説明します。

### 1\. Obsidian Vaultをデバイス間で共有

![iCloud + GitHub](https://res.cloudinary.com/zenn/image/fetch/s--v9TVIF6i--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://pbs.twimg.com/media/GyTSGKva4AICPuq.jpg)

Obsidian Vault（ドキュメント保管庫）をどこに作るかですが、iCloudに作成しています。Appleアカウントでログインしていれば、ドキュメントフォルダを共有できるので、無料でObsidianを活用できます。Obsidianに課金している方は、あまり気にする必要はないかもしれないです。

iCloudフォルダはGitHubリポジトリも兼ねていて、iCloudでログインしていないデバイスでも、リポジトリをCloneしてくれば同じ運用が可能です。例えばMacとWindows間で共有したいケースなどが挙げられます。またGitHubのリポジトリにすればドキュメントの変更履歴も追うことができます。

つまり以下のように併用しています。

- **iCloud: Appleデバイス間のVault共有**
- **GitHub: MacとWindowsなどのデバイス間のVault共有 + 変更履歴の記録**

二重共有なのでクセがありますが、個人的にはこれで上手くいっています。

### 2\. Obsidian Web Clipperで記事を保存

Obsidianの拡張機能 **Obsidian Web Clipper** で、技術記事や参考文献として保存したいドキュメントをObsidianに保存します。

ChromeだけでなくiPhoneでも使用できるので、PC・スマホの両方からいつでもObsidianのClippings/ にドキュメントを一旦投げ込みます。

#### ブラウザ版Obsidian Web Clipper

Chromeなどブラウザの拡張機能は、以下のリンクからインストールします

インストールすると、Chromeの拡張機能にObsidianのマークが現れます。 **Obsidianに追加** のボタンを押すとデフォルトの `Clippings` フォルダに追加されます。

![Web Clipper](https://res.cloudinary.com/zenn/image/fetch/s--RcAwVAlN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://pbs.twimg.com/media/GyTSnaZasAAHJ89.jpg)

#### モバイル版Obsidian Web Clipper

モバイル版Obsidian Web Clipperも存在します。以下はApple Storeのリンクです↓

iPhoneでObsidian Web Clipperを使うには、URL部分左側の拡張機能をクリックします。

![obsidian-iphone1](https://res.cloudinary.com/zenn/image/fetch/s--XyGtmBin--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/1fc2ab1d827bd77c7c7d54df.png%3Fsha%3D2c12e21f02132a1179ac0c7893e9db7a85298600)

#### Web Clipperが使えないドキュメント

Web Clipperが使えないものは、リンクやスレッドをChatGPTやGrokに与えてObsidian形式のmdファイルを生成してもらい、同じく `Clippings/` に保存します。

またYoutubeの動画などは、NotebookLMに食わせることで同じことが可能です。

### 3\. Obsidianのフォルダ構成はシンプルに

Obsidianについて調べると必ずと言って良いほど **Zettelkasten** という手法を目にしますが、自分には合いませんでした。

フォルダ管理を色々試した結果、以下のような構成に落ち着いています。

- `00_inbox/`: 一時的なキャプチャ、メモ
- `01_sources/`: 外部ソース、知識保存場所
- `02_working/`: 作業中のドラフト
- `03_output/`: 自分の記事など
- `Clippings/`: Obsidian Web Clipperの保存場所

実際のフォルダ：  
![フォルダ構成](https://res.cloudinary.com/zenn/image/fetch/s--kzyEtJIX--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://pbs.twimg.com/media/GyTT9Vla4AIFE7_.png)

フォルダ構成は人によって最適なものが違うので、使いながら探す必要があります。

### 4\. Clippings/をClaude Codeで整理

![整理プロセス](https://res.cloudinary.com/zenn/image/fetch/s--L8dwPTwK--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://pbs.twimg.com/media/GyTVZoya4AQAItR.jpg)

Obsidian Web Clipperで `Clippings/` にドキュメントを放り込んだあとは、 `01_sources/` に振り分けて整理します。

手作業だと手間なので、 **Claude Codeのカスタムスラッシュコマンド** を使います。

- tag-list.mdの確認
- Obsidianタグ付け
- フォルダ振り分け
- tag-list.mdの更新

これらを一気に実行するカスタムSlash Commandを作成します。サーバーを立てられる方なら\*\*Clippings/\*\*を監視して、追加されたら即座にタグ付けと振り分けを実行しても良いと思います。

またタグ専用ドキュメント `tag-list.md` で既存タグを管理すると、無尽蔵にObsidianのタグが増えすぎることをある程度防げます。

実際のカスタムスラッシュコマンド

organize-clippings.md

```md
オプション実行モード:
```

### 5\. Claude CodeとObsidian MCPサーバーを繋ぐ

![活用方法](https://res.cloudinary.com/zenn/image/fetch/s--Wck3cCgN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://pbs.twimg.com/media/GyTXmrOa4AQywvc.jpg)

Obsidianに蓄えたドキュメントは、基本的にはMCPサーバー経由で活用しています。

Obsidian MCPサーバーはいくつか存在しますが、Obsidian Desktopからローカル接続するのが個人的には良かったです。MCPサーバーのツール群が一番使い易いことが理由です。

まずObsidian Desktopでコミュニティプラグインを2つ入れます

- **Local REST API**
- **MCP Tools**

この2つのプラグインにより、ローカルAPI（APIキー）とMCPサーバー機能を利用できます。

Claude CodeでこのObsidian DesktopのMCPサーバーに繋ぐには以下のコマンドを実行します。

```bash
# "/path/to/vault" はご自身の環境に合わせて修正してください
  claude mcp add obsidian-mcp-tools -s user -- "/path/to/vault/.obsidian/plugins/mcp-tools/bin/mcp-server" --env OBSIDIAN_API_KEY=YOUR_API_KEY
```

もしくは`.claude.json` に自分で追加します。

```json
{
    "mcpServers": {
      "obsidian-mcp-tools": {
        "command": "/path/to/vault/.obsidian/plugins/mcp-tools/bin/mcp-server",
        "args": [],
        "env": {
          "OBSIDIAN_API_KEY": "YOUR_API_KEY"
        }
      }
    }
  }
```

既にClaude DesktopとObsidianをMCPサーバーで連携している人は、以下のコマンドでClaude Codeでも設定できます。

```bash
claude mcp add-from-claude-desktop
```

Claude CodeとObsidian MCPサーバーが上手く接続できているかは、以下のコマンドで確認できます。

**Terminal**:

```bash
claude mcp list
```

`obsidian-mcp-tools: /path/to/.obsidian/plugins/mcp-tools/bin/mcp-server` が表示されているはずです

**Claude CodeのSlash Command**:

```bash
/mcp
```

`obsidian-mcp-tools  ✔ connected · Enter to view details` が表示されているはずです。

### 6\. Claude CodeからObsidianを活用する

実際にObsidianに蓄えたドキュメントを活用します。Obsidianのドキュメントの活用方法は人によって千差万別ですが、私の場合は以下のように活用しています。

主な活用方法：

- **タグからドキュメント検索**
- **技術記事の内容の査読**
- **参考文献用のリンクの取得**
- **登壇スライド内のコンテンツ作成**

これらをObsidianのドキュメントを使って、AIエージェントのコンテキストとして与えます。ドキュメントをVSCode上から扱えるようにしていて、Obsidian Desktopはサーバー的な役割で起動するのみです。

一応ObsidianのコミュニティプラグインのTerminalを使ってClaude Codeを実行できるのですが、個人的にはVSCode（もしくはCursor）でObsidianのフォルダを弄る、もしくはMCPサーバー経由で活用する方がよかったです。

### Obsidian運用の流れまとめ

ここまでの流れをまとめると、以下の運用になります。

1. **Obsidian Vaultをデバイス間で共有**
2. **Obsidian Web Clipperで記事を保存**
3. **Obsidianのフォルダ構成はシンプルに**
4. **Clippings/をClaude Codeで整理**
5. **Claude CodeとObsidian MCPサーバーを繋ぐ**
6. **Claude CodeからObsidianを活用する**

## まとめ

Obsidianが便利だと感じるまでに3ヶ月ほどかかりました。

Obsidian活用の個人的なコツは以下の通りです:

- 雑に放り込む
- AIに編集を任せる
- AIに整理を任せる
- AIに解説を任せる
- AIに引用を任せる
- 人間は文献付きの成果物だけを扱う

Obsidianは自由度が高い故に挫折してしまうことが多いですが、 **AI-Firstを心掛けると上手く活用できるのではないか** と考えています。

今後は自分の考えや知識のドキュメントが資産になって、生成AIに活用させる機会がますます増えているため、Obsidianに限らずドキュメント管理は重要になると思っています。

この記事が参考になれば幸いです。

## 𝕏フォローしてくれると嬉しいです！

𝕏でも情報発信しているので、フォローしていただけると励みになります！

## 参考文献

[GitHubで編集を提案](https://github.com/oikon48/zenn-contents/blob/main/articles/obsidian-claude-code.md)

202

121

202

121