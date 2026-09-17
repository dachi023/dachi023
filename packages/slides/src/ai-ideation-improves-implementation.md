---
marp: true
theme: mosh
paginate: false
style: |
  section {
    --marpit-html: enable;
  }
---

<!-- _class: lead -->

![bg](./assets/bg-cover.svg)
<p class="slides-label">PRESENTATION SLIDES</p>
<p class="version">[ ver.01 2026.03 ]</p>
<div class="cover-footer">
  <img class="cover-logo" src="./assets/logo-mosh.svg">
  <p class="description">MOSH develops and operates a platform that supports independent creators<br>in selling their services online.</p>
</div>
<p class="copyright">&copy; MOSH, Inc.</p>

# 良い機能を作るためにAIと壁打ちをしたら<br>実装も快適になってしまった話

<p class="event-name">MOSH Tech Meetup #3</p>

---
<!-- _class: split -->

![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

<div class="split-left">

## dachi

MOSH株式会社

- フロントエンド基盤開発
- 技術負債の解消
- 開発組織の生産性向上
- 社内ツール開発
- 技術広報

<p class="note"><img class="icon-x" src="./assets/logo-x.svg"> @dachi</p>

</div>
<div class="split-right">
  <img src="./assets/profile.jpg">
</div>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## このスライドの制作風景

cmuxとmarpはいいぞ

![w:780](./assets/ai-ideation-improves-implementation/cmux-marp.png)

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 用意するもの

- お好きなコーディングエージェント
- GitHub CLI

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## エージェントに何かを依頼するとき

当たり前だがやりたいことが明確であればブレは減る

### よくない依頼の仕方

「ユーザーの今月の売上を表示するページを作成して」

- 消費税は？
- 日毎の売上は？
- 振込状況は？

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## Issueの質を上げるために

最近ハマってるやり方

1. Issueを作成する (titleだけでもOK)
2. 「{org}/{repo}/issues/{num} の詳細を決めたいので壁打ちして」
3. やりとりしつつ流れの中でIssueをアップデートさせ続ける
4. できあがり

あとは「このIssueの実装を進めて」で実装を開始してもらう。

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## AIと壁打ちする良さ

対話の中で以下のような情報が整理されていく

- スコープを明確にすることでPRの肥大化を防止
- 技術選定や実装方針を文書化する
- 受け入れ条件を定義し何をテストすべきか？を明確にする

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 実装する時に嬉しいこと

- AIの実装がスムーズ、一発でうまく実装できる可能性が上がる
- なんか違うな？となった時に別セッションでリトライするのが楽
- レビュアーに実装の意図を伝えやすい

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## Issue以外に揃っていると良いもの

以下のような情報がまとまっていると更に賢くなる

- AGENTS.md, CLAUDE.md などリポジトリ全体についてまとまっているもの
- ADR, Design Docなどの過去の意思決定が分かるもの

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## その他Tips

- Issue templateで組織内での品質を均一化する
- 壁打ちの過程で重要な意思決定などはDesign Doc等に切り出すなども検討する
- Issueが完成したら：
    - 別セッション立ち上げて「検討漏れや決めたほうがいいことを確認して」と聞く
    - ↑ 初回とは違うモデルに依頼するのも効果的

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 👋

<p class="section-description">ありがとうございました</p>
