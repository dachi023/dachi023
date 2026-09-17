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
<p class="version">[ ver.01 2025.12 ]</p>
<div class="cover-footer">
  <img class="cover-logo" src="./assets/logo-mosh.svg">
  <p class="description">MOSH develops and operates a platform that supports independent creators<br>in selling their services online.</p>
</div>
<p class="copyright">&copy; MOSH, Inc.</p>

# コーディングエージェントに身を委ねた1年間の所感

<p class="event-name">MOSH Advent Calendar 2025</p>

---
<!-- _class: split -->

![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

<div class="split-left">

## dachi

MOSH株式会社。

- コーディングエージェント活用
- AI駆動開発の実践

<p class="note"><img class="icon-x" src="./assets/logo-x.svg"> @dachi</p>

</div>
<div class="split-right">
  <img src="./assets/profile-placeholder.svg">
</div>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## この発表について

開発業務でコーディングエージェントなどのAIツールが**必須**となりつつある中で、**実務ベースでどのケースが特に効果的なのか**を振り返る。

- 設計
- 実装
- 運用

の各フェーズで整理する。

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 設計

<p class="section-description">技術選定・調査フェーズでのAI活用</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 設計フェーズでの活用

技術選定や実現方法の調査をAIに代行してもらう機会が増加した。

- 複数の類似ライブラリから**活発に開発されているもの**を特定
- 機能実装の**他社事例**を調査
- 調査結果の正確性を確認した後に、組織に最適な選択肢を判断

AIによる調査の後、人間が検証・判断するフローへ移行した。

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 実装

<p class="section-description">コーディングエージェントの実務活用</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 実装 - 全般

- CLI型コーディングエージェントのエディタプラグイン化が一般的になった
- **Cursor**、**Zed**、**Antigravity**などAI前提エディタの利用が拡大
- モデル更新とコンテキストサイズ拡張により精度が向上
- **最終的に人間による判断は必須**
- `AGENTS.md`などのツール活用で再現性の高いミスを削減

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 実装 - UI

- Figma MCPやブラウザ操作機能が登場した
- デザインファイルの完全な再現は**まだまだ難しい**
- 必要情報を事前に整備すれば、コンポーネント単位で良質な成果物が生成可能
- 最終調整は手作業の方が迅速なケースも多い

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 実装 - ナレッジベース

- **Devin**のサービスを継続利用中
- **DeepWiki**の完成度が高い
  - コード関連のナレッジはこれで十分
  - Ask機能で必要情報を質問できる
- 社内ドキュメントとして有用

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 実装 - テスト

- ADRやIssueに実装前提を明記すれば、テスト条件を**的確に生成**
- 情報不足の場合は「網羅的だが不要なテストが混在」
- それでも手書きより楽
  - 不要なものを削除する作業で済む
- **テストの質はインプットの質に依存する**

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 実装 - レビュー

- PR上のAIレビューが事前に**凡ミスを指摘**
- 人によるレビュー負荷を軽減
- Linter併用により、人間は**機能や設計**など本質的な検討に集中
- GitHub環境では**Copilotレビュー**の導入が容易

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 運用

<p class="section-description">システム運用領域でのAI活用</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 運用フェーズでの活用

- **Amazon Q**や**Datadog Bits AI**など運用向けAI活用が始まった
- インシデント時のログ調査・原因特定への適用を想定
- 現状はエラーログをClaudeに質問するなど、複数ツール間の往復作業が発生
- 統合されたワークフローの実現に期待

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# まとめ

<p class="section-description">1年間の振り返りと今後</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## まとめ

- AI使用が**盲目的になりがち**だったことへの反省
- **何をやらせると有効か**を改めて整理することが重要
- 継続的な進化に対応しながら、適切に活用していく

| フェーズ | 効果的な用途 |
|----------|-------------|
| 設計 | 技術選定・他社事例調査 |
| 実装 | コーディング、テスト生成、レビュー |
| 運用 | ログ分析・原因特定（発展途上） |
