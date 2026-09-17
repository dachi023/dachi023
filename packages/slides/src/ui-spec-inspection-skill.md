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
<p class="version">[ ver.01 2026.06 ]</p>
<div class="cover-footer">
  <img class="cover-logo" src="./assets/logo-mosh.svg">
  <p class="description">MOSH develops and operates a platform that supports independent creators<br>in selling their services online.</p>
</div>
<p class="copyright">&copy; MOSH, Inc.</p>

# ただしいUIってなに

<p class="event-name">26-06-22 社内勉強会</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 余談

ワールドカップ見てますか？

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 本題

<p class="section-description">デザインファイルやガイドラインとの乖離をどう埋めに行くか？</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 課題

以下の可視化、検査、改善することができていないのが現状の課題。

- 実装とデザインが揃っていない
- デザインガイドラインに記載されているルールを網羅していない

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# ただしいUIとは

<p class="section-description">前提を揃える</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 正しく実装できている、の定義

大きく分けると2つ。

- スタイル：Figmaで定義されたトークン、コンポーネント定義に準拠
- 内部実装：ガイドラインに準拠したインタフェース(Props)、a11yに準拠

上記を満たしているか？を検査・修正できるようにしたい。

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# デザインの検査を仕組み化する

<p class="section-description">Skillsでできるか試してみる</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 仕組みの全体像

1. `/design-skill Button` 的なコマンドを実行する
2. Storybook MCPがドキュメントURLを返却する
3. ボタンコンポーネントのデザインデータ、ガイドラインをMCP経由で取得
4. 実装との乖離を検査し修正に進む

taiyakiの場合、デザインデータはFigmaからガイドラインはNotionから取得できる。
ブラウザ上での見た目の話に関してはVRTで担保するので対象から外している。

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 実装

StorybookファイルのJSDocにURLを記載しておくとMCP経由で取得可能になる。
コーディングエージェントにURLを取得させ、Figma/Notion MCP経由で取りに行かせる。

```ts
/**
 * Figma: https://www.figma.com/design/...
 * Notion: https://app.notion.com/p/...
 */
const meta: Meta<typeof Button> = {
  // ...
}
```

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 実行結果 その1

```plain
用途・使い分け（Notion）

- variant の役割: default(primary) = 画面/モーダルで最重要のアクション、1画面1つだけ（公開・保存・確定）。secondary = primary
を補完する中重要度（プレビュー・設定・招待）。ghost = 低強調・安全な操作（キャンセル・閉じる・後で）。link =
遷移・補助情報（詳しく見る）。destructive = 不可逆・破壊的操作（削除・解除）。
- 2 ボタン並びは「左=secondary / 右=primary」の期待値に沿う。
- アイコンは装飾でなく意味の補強がある時だけ。同一グループ内はスタイルを揃える（全部アイコン付き or 全部なし）。
- 文中リンクは Button を使わず、インラインの link-text（下線付き）で表現する。

UX ライティング / アクセシビリティ（Notion）

- ラベルは行動を示す動詞・短く明快に（「OK」でなく「保存」「公開」）。
- タッチ領域は最低 48px（Design specifications でも md/lg ともに padding 込みで 48px を要求）。
- フォーカス時の枠線表示が必須。コントラスト比を確保。
- アイコンのみボタンには aria-label を必ず設定（※ Notion・確認事項とも「Icon button は Button と別管理が望ましい」とコメント。現行 button.tsx
のスコープ外）。
```

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 実行結果 その2

```plain
実装方針（variant / size / state 別）

トークン値はすべて globals.css に対応ユーティリティが存在することを確認済み。

┌─────────────────┬───────────────────┬───────────────────────────────┬───────────────────┬───────────────────────┬─────────────────────────────┐
│      対象       │ あるべき値（Figma │  対応トークン（globals.css）  │ 現状（button.tsx  │         差分          │       推奨アクション        │
│                 │        ）         │                               │        ）         │                       │                             │
├─────────────────┼───────────────────┼───────────────────────────────┼───────────────────┼───────────────────────┼─────────────────────────────┤
│ 角丸（全        │ 8px = Corner      │ rounded-lg（--radius=0.5rem=8 │ rounded-md（6px） │ 2px 小さい            │ rounded-md → rounded-lg     │
│ variant）       │ Radius/Small      │ px）                          │                   │                       │                             │
├─────────────────┼───────────────────┼───────────────────────────────┼───────────────────┼───────────────────────┼─────────────────────────────┤

# 差分の一覧がババーっと表示されている、長過ぎるので割愛

```

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 実行結果 その3

```plain
要判断 / 未取得

- destructive の塗り（Figma vs Notion の衝突）: Figma コンポーネントは「赤塗りつぶし」の destructive を定義し、現行実装もそれに準拠。一方 Notion
ガイドライン §1.5/§5 は「赤塗りつぶしは避け、Outline/Ghost + 赤テキストにする」と明記。見た目は Figma 優先・用途文言は Notion
優先という原則が真っ向から衝突するため、デザイナー確認が必要（Figma の filled destructive を正とするか、ガイドライン通り text/outline に寄せるか）。
- focused 表現: Figma は枠線 #222222、codebase の border-focused は #111827。色も表現方法（border か ring か）も差がある。トークン値を Figma
に合わせるか、現行 ring を許容するかは要判断。
- タッチ領域 48px（md）: 視覚高 40px を保ったまま 48px のヒットエリアを作る実装（外側 py-4px の透明ラッパー or
擬似要素）は構造変更を伴う。対応要否と方式は要判断。
- loading / state 管理: Figma・確認事項は loading（スピナー）を含む state 設計を要望。現行 button.tsx に loading prop
なし。機能追加スコープのため、本ブリーフ範囲外として別途要判断。
- 確認事項セクションの他の指摘（asChild/Badge/bgColor/width prop は不要・不明との designer コメント）は API
設計の見直し論点であり、見た目方針とは別軸。実装着手前に整理が必要。
```

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## まとめ

- 1つのデータソースから他を辿れるよう設計してあげる
  - 個別に定義するとステップ数が増え安定したアウトプットを得られない
- どこの何を正とするか、を定義する
  - 今回の場合であればFigmaにあるトークン、コンポーネント定義やNotionのガイドラインが正ですよ、という前提でコーディングエージェントに指示を出す
