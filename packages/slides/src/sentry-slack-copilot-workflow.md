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

# アプリエラーの対応を自動化したい

<p class="event-name">MOSH x RABO x AnotherBall 勉強会</p>

---
<!-- _class: split -->

![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

<div class="split-left">

## dachi

MOSH株式会社
Technical Enablement ユニット

- フロントエンド基盤開発
- 技術負債の解消
- 開発組織の生産性向上
- 社内ツール開発
- 技術広報

<p class="note"><img class="icon-x" src="./assets/logo-x.svg"> @dachi_023</p>

</div>
<div class="split-right">
  <img src="./assets/profile.jpg">
</div>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## この発表について

Sentryの通知をトリガーにIssueの作成とPRの作成を自動化するワークフローを構築。

- なぜ必要か
- ワークフローの設計
- 作ってみた感想

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 背景・課題

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## エラー対応：従来のフロー

1. Sentryがエラーを検知する
2. Slackに通知が届く
3. エラー内容を確認し対応すべきかどうかを判断
4. 担当者が作業、その後リリース

**課題**
- 検知から内容の理解までに時間がかかる場合がある
- シンプルな不具合でも人手が必要でそれなりのコストがかかる
- 日頃から通知が多いと対応が雑になる

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## エラー対応：こうなりたい

1. Sentryがエラーを検知する
2. Slackに通知が届く
3. 自動でIssueが作成され、CopilotがPRを作成する
4. 人がレビューしマージ、リリース

**改善**
- AIがエラー内容を要約、エラーの内容をすぐに理解できる
- 初動対応はCopilotが実施するのでコードレビューだけで済む
- 雑なArchive, Resolveが減る(減って欲しい)

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# ワークフロー設計・実装

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## コンポーネント

| コンポーネント | 役割 |
|--------------|------|
| Sentry | エラー検知、Seerによるエラー解析 |
| Slack | 通知、ワークフロー開始の起点 |
| Cloudflare Workers | エラー解析(OpenAI + Sentry Seer)、GitHub Issue作成 |
| GitHub SWE Copilot | Issueの内容から修正PRを作成 |

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 処理の流れ

![](./assets/sentry-slack-copilot-workflow/sequence.svg)

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## Cloudflare Workersの中

### 自動で調査

1. Slackイベントを受信、Cloudflare Queueにenqueue
    - fetch handlerは30秒でタイムアウト、queue handlerは15分
2. AI agent (gpt-5-nano)がSentry APIでエラー情報を収集
3. 調査結果をSlackに投稿

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

### Issueの作成

1. GitHub Issueを作成
2. Cloudflare Queueにenqueue
3. AI agentがSentry Seerの分析結果などを材料にIssue本文を生成
4. Issueを更新後、Copilot SWE Agentをアサイン

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 作ってみた感想

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## エラー対応が少し気楽になった

- エラー解説、Issueの作成など初動は速く出来そう
- 原因がシンプルであればCopilotの修正だけで十分なものもある
- レビューしてリリースすればいい

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 課題

- Issueの質のみがPR作成の成功を決める
    - Seerは結構検討違いなことを言う
    - Workers内のエージェントにコード検索させる方が良い説
- Sentry内のノイズを減らさないとIssueが無駄に増える
    - 現状はノイズが多いのでIssue作成は任意にした
- 人がレビューする部分はまだ外せない
    - Issueの精度問題があり、信頼できないケースがあった

---
<!-- _class: split -->

![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

<div class="split-left">

## 今の落とし所

- 解析させるのはデフォでやる
- Issue作るかどうかは人が決める
- Issue作ったらCopilotが動く

</div>
<div class="split-right">
  <img src="./assets/sentry-slack-copilot-workflow/slack.png">
</div>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## まとめ

- アプリエラーを検知しつつAIに勝手に対応させるためのフローを構築
- エラーの解説やIssue作成などを自動化することで人の負担を軽減
- ノイズや原因分析の精度を高めれば完全自動化もできそう

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 👋

<p class="section-description">ありがとうございました</p>
