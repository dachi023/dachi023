# moshテーマ

MOSH名義の登壇で使う。テーマの実体は `packages/slides/themes/mosh.css`。編集しないこと。

背景・ロゴ・コピーライトはCSSではなくスライド本文に書く。**すべてのスライドに3行を含める**必要がある。

## Frontmatter

```yaml
---
marp: true
theme: mosh
paginate: false
style: |
  section {
    --marpit-html: enable;
  }
---
```

## レイアウト

| レイアウト | `_class` | 背景 | 用途 |
| --- | --- | --- | --- |
| 表紙 | `lead` | `bg-cover.svg` | 1枚目のタイトル |
| 縦割り | `split` | `bg-default.svg` | プロフィールなど左右2分割 |
| セクション表紙 | `section-cover` | `bg-section-cover.svg` | 章タイトル |
| 通常 | 指定なし | `bg-default.svg` | 本文 |

### 1. 表紙

```markdown
<!-- _class: lead -->

![bg](./assets/bg-cover.svg)
<p class="slides-label">PRESENTATION SLIDES</p>
<p class="version">[ ver.01 2026.02 ]</p>
<div class="cover-footer">
  <img class="cover-logo" src="./assets/logo-mosh.svg">
  <p class="description">MOSH develops and operates a platform that supports independent creators<br>in selling their services online.</p>
</div>
<p class="copyright">&copy; MOSH, Inc.</p>

# タイトルをここに書く

<p class="event-name">イベント名をここに書く</p>
```

### 2. プロフィール（split）

```markdown
---
<!-- _class: split -->

![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

<div class="split-left">

## 名前

MOSH株式会社 XXX部
XXXチーム

- スキル1
- スキル2

<p class="note"><img class="icon-x" src="./assets/logo-x.svg"> @ユーザー名</p>

</div>
<div class="split-right">
  <img src="./assets/profile-placeholder.svg">
</div>
```

- プロフィール画像がある場合は `profile-placeholder.svg` を実際の画像パスに差し替える。
- `split-left` の中ではMarkdown記法をそのまま使える。

### 3. セクション表紙

```markdown
---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# セクションタイトル

<p class="section-description">セクションの説明文</p>
```

背景が暗いため、ロゴは `logo-mosh-white.svg` を使う。

### 4. 通常スライド

```markdown
---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## スライドタイトル

本文をここに書く
```

## 注意点

- 背景画像・ロゴ・コピーライトの3行を省略しない。
- 参照パスを変えない。相対パス `./assets/` を維持する。
- personalテーマ専用のクラス（`statement`、`cols`、`lede` など）を使わない。
