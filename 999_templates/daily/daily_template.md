---
date: "{{DATE:YYYY-MM-DD}}"
aliases: []
tags:
  - daily
---

## ROUTINE

- [ ] 8:00 起床
- [ ] 8:30-9:00 勉強
- [ ] 9:30-10:00 筋トレ
- [ ] 22:00 ~ 明日の予定を作成

### やらないことリスト

- [ ] 朝二度寝しない
- [ ] 一日ゲームだけをする

---

## TODO

## month

- [ ] アプリケーションを作る

### week

- [ ] Zenn に何かしらの記事を投稿
- [ ] 朝二度寝しない
- [ ] カフェに行って気分転換をする

### day

- [ ] 03_output に気になった記事・内容のメモを追加
- [ ] Go の競プロ 1 問解く

---

## Memo

---

## Journal

### 今日の記録

```dataviewjs
const today = dv.date("today").toFormat("yyyy-MM-dd");
const dailyPath = `00_inbox/thino/${today}.md`;
const file = dv.page(dailyPath);

if (file) {
    const content = await dv.io.load(dailyPath);
    dv.paragraph(content);
} else {
    dv.paragraph("今日のデイリーノートがまだ作成されていません。");
}
```

---

## リンク

[[{{date:YYYY-MM-DD|offset:-1d}}|← 前日]] | [[{{date:YYYY-MM-DD|offset:1d}}|翌日 →]]
