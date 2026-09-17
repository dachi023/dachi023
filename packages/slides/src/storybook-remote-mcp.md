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
<p class="version">[ ver.01 2026.07 ]</p>
<div class="cover-footer">
  <img class="cover-logo" src="./assets/logo-mosh.svg">
  <p class="description">MOSH develops and operates a platform that supports independent creators<br>in selling their services online.</p>
</div>
<p class="copyright">&copy; MOSH, Inc.</p>

# Storybookを起点にAIへデザイン文脈を渡す

<p class="event-name">26-07-21 社内勉強会</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 話すこと

実装はStorybook、見た目はFigma、仕様はNotionにあります。
人間は頭の中で3つを繋いでいますが、AIには繋がって見えていません。

- 現状：Storyのコメント2行を経路にして3点を繋ぐ
- 構想：Cloudflare上でStorybookのMCPをリモート化する
- 応用：その経路を使うスキルを2つ作った

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 現状

<p class="section-description">StoryのJSDocからRemote MCPまでの経路</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 全体像

```plain
[ *.stories.tsx ]     meta 直上の JSDoc に Figma: / Notion: の 2 行
      |
      |  CSF 静的解析 + react-docgen
      v
[ manifests/components.json ]
      |               description に JSDoc / reactDocgen に props
      |  HTTP で配信
      v
[ localhost:6006/mcp ]  addon-mcp が dev サーバーに生やすルート
      |
      |  tool call
      v
[ AI エージェント ]
      |
      +--> [ Figma MCP  ]  mcp.figma.com/mcp    寸法・色・変数
      +--> [ Notion MCP ]  mcp.notion.com/mcp   用途・使い分け
```

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 書くのはmetaの直上2行だけ

コンポーネント単位でmetaのJSDocにラベル行を置きます。
Storyごとには書きません。

```ts
/**
 * Figma: https://www.figma.com/design/...?node-id=7-263
 * Notion: https://app.notion.com/p/moshjp/Buttons-...
 */
const meta: Meta<typeof Button> = {
  title: "Components/Button",
  parameters: { docs: { page: DesignDocsPage } },
};
```

このJSDocがそのままcomponent descriptionとしてmanifestに載ります。

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## parameters.designを使わない理由

addon-designsの`parameters.design`はAIからは見えません。
manifestはCSFの静的解析とreact-docgenから生成されるためです。

- ランタイムのparametersもDocsPageの中身も対象外になる
- metaのJSDocなら静的解析の対象に入る
- 同じJSDocを`DesignDocsPage`が人間向けにも描画する

URLを定数で二重に持たないので、片方だけ古くなる事故が起きません。

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 構想

<p class="section-description">CloudflareでStorybookのMCPをリモート化する</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 現状の弱点

`/mcp`はdevサーバーのルートなので、各自が手元でStorybookを起動しないと繋がりません。
静的ビルドには`manifests/components.json`が出力されています。

配信中のWorkerに`/mcp`を生やせば、起動なしで届きます。

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## Worker上のMCPサーバー

```plain
             [ Cloudflare Worker : tools/storybook ]

AI エージェント --POST /mcp--> src/index.ts --> bearer.ts ----> mcp-handler
                                   |           不一致は 401     @storybook/mcp
                                   |                                  |
ブラウザ ------- GET /* ---------> src/index.ts                       |
                                   |                                  |
                                   v                                  |
                          env.ASSETS.fetch()                          |
                                   |            manifestProvider が   |
                                   v            リクエストごとに取得 <--+
                          [ storybook-static ]
                            index.html / iframe.html
                            manifests/components.json
```

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 設計上の要点

`tools/`ディレクトリを新設するADRはマージ済みで、実装はレビュー中です。

- `McpAgent`やDurable Objectsは使わず、素のWorkerのfetchハンドラで組む
- 認証はOAuthで試作したあと、静的なBearerトークンに簡素化した
- devのtoolsetは開発サーバー前提なので、リモートではdocsのtoolsetが主用途
- `docs.json`はビルドで生成されないため、空のスタブを返している

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 応用

<p class="section-description">この経路を使うスキルを2つ作った</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## なぜスキルにするのか

MCPが繋がっただけでは、参照の取り方も出力の形も人によってばらつきます。
そこを固定するために`agents/skills/`へ2つ置きました。

どちらも`disable-model-invocation: true`で、AIが勝手に起動することはありません。

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 2つのスキルの関係

```plain
  [ design-context ] 実装の前        [ design-review ] 実装の後
           |                                  |
           +----------------+-----------------+
                            |
                 共通のデザイン参照レイヤー
                            |
      +---------------------+---------------------+
      |                     |                     |
 stories の JSDoc       Figma MCP             Notion MCP
 grep "Figma:|Notion:"  get_metadata →        notion-fetch
                        フレーム推定
      |                     |                     |
      +---------------------+---------------------+
                            |
           +----------------+----------------+
           |                                 |
     実装方針表                         Design Review 結果
     対象 / あるべき値 / 対応トークン    逸脱 / 箇所 / 根拠 / 推奨
     現状 / 差分 / 推奨アクション        対象外・判定不可を分離
```

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## design-context（実装の前）

Figmaの値とNotionのガイドラインを取得し、既存実装と突き合わせた方針表を出します。
実装方針の提示までが責務で、実装には進みません。

- Figmaのvariable名を`globals.css`の`@utility`や`--token-*`にgrepで突合する
- 対応が無ければ「未定義」と報告し、`#d1d5db`のような生値を直書きさせない
- URLが無ければ推測せずユーザーに確認する
- ページのframeから本体frameを推定し、variant軸の一致を最優先で判断する

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## design-review（実装の後）

Notionのガイドラインに照らして逸脱を指摘します。
ガイドラインは絶対のルールではなく推奨指針なので、出力も「要検討」として書かせます。

- 原因がコンポーネント定義側にあるときは、利用箇所ごとに重複させず1箇所へ帰属する
- Notionの参照が無いものは黙って飛ばさず「対象外」として一覧に残す
- コントラスト比など判定できない項目は分けて出す
- 見た目のピクセル準拠はVRTの責務として外している

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## まとめ

- 1つのデータソースから他を辿れるよう設計する
  - Storyのコメント2行が、FigmaとNotionへの入口になる
- 機械が読める場所に書く
  - 見た目が同じでも、静的解析に乗らない場所ではAIに届かない
- どこの何を正とするかを決める
  - 見た目と寸法はFigma、用途と意味はNotion、という前提で指示を出す
