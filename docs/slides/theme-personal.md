# personalテーマ

会社と関係のない個人名義の登壇で使う。テーマの実体は `packages/slides/themes/personal.css`。

MOSHのロゴ・コピーライト・ブランドカラーは一切使わない。

## 設計方針

- 背景画像を使わず、装飾はすべてCSSで描画する。
- 背景・ロゴ・コピーライトの記述は不要。`---` の直後から本文を書ける。
- フッターの署名（デフォルト `dachi`）とページ番号はテーマが自動で描画する。

## Frontmatter

`--marpit-html` はテーマ側で有効化しているため `style` は不要。

```yaml
---
marp: true
theme: personal
paginate: true
---
```

## 使えるクラス

| クラス | 指定場所 | 用途 |
| --- | --- | --- |
| `lead` | `_class` | 表紙 |
| `split` | `_class` | プロフィールなど左右2分割 |
| `section-cover` | `_class` | 章タイトル（濃色背景） |
| `statement` | `_class` | 一言だけ大きく中央に見せる |
| 指定なし | - | 本文スライド |
| `cols` | `div` | 本文を2カラムに分割する |
| `figure` | `div` | 図のプレースホルダ（破線枠） |
| `lede` | `p` | 本文より大きいリード文 |
| `note` | `p` | 本文より小さい補足 |

## レイアウト

### 1. 表紙

```markdown
<!-- _class: lead -->

# タイトルをここに書く

<p class="event-name">イベント名 / 2026.09.01</p>

<div class="speaker">
  <img class="speaker-avatar" src="./assets/profile.jpg">
  <div>
    <p class="speaker-name">dachi</p>
    <p class="speaker-links">
      <span><img class="icon" src="./assets/personal/x-logo-black.png">@dachi_023</span>
      <span><img class="icon" src="./assets/personal/github-invertocat-black.svg">dachi023</span>
    </p>
  </div>
</div>
```

表紙だけはフッターの署名とページ番号を出さない。

### 2. プロフィール（split）

```markdown
---

<!-- _class: split -->

<div class="split-left">

## 名前

<p class="role">Frontend / Developer Experience</p>

- 自己紹介の項目1
- 自己紹介の項目2

<p class="links">
  <span><img class="icon" src="./assets/personal/x-logo-black.png">@dachi_023</span>
  <span><img class="icon" src="./assets/personal/github-invertocat-black.svg">dachi023</span>
</p>

</div>
<div class="split-right">
  <img src="./assets/profile.jpg">
</div>
```

右側の画像は円形にトリミングされる。

### 3. セクション表紙

```markdown
---

<!-- _class: section-cover -->

# セクションタイトル

<p class="section-description">セクションの説明文</p>
```

背景は濃色に反転する。テキストの色指定は不要。「Section 01」のような通し番号のラベルは付けない。

### 4. 主張（statement）

一言だけを大きく中央に見せたいときに使う。

```markdown
---

<!-- _class: statement -->

## 伝えたいこと

<p class="note">補足があればここに書く</p>
```

### 5. 通常スライド

```markdown
---

## スライドタイトル

本文をここに書く
```

### 2カラム（cols）

```markdown
<div class="cols">
<div>

### 左カラム

左の内容

</div>
<div>

### 右カラム

右の内容

</div>
</div>
```

### 図のプレースホルダ（figure）

図がまだ無い段階では枠だけ置いておく。

```markdown
<div class="figure">［図］探索範囲の比較</div>
```

高さを変えたい場合は `style="min-height: 260px"` を付ける。

## 文体の指針

見た目より文体のほうが印象を左右する。

- 箇条書きを既定にしない。短い断定文を改行で並べる。
- 文末は体言止めか言い切り。「〜します」「〜できます」で全文を揃えない。
- 1枚に1メッセージ。入り切らない場合はスライドを分ける。
- 強調はアクセント色（`**太字**`）でインラインに差す。多用しない。
- 日付、件数、固有名詞など具体を入れる。一般論だけのスライドを作らない。
- 文字だけのスライドが続かないよう、図・スクリーンショットを挟む。

## 配色

[yosegi](https://github.com/dachi023/yosegi) のブランドパレット（`assets/brand/README.md`）に準拠する。色を足す場合もこの5色から選ぶ。

| 名前 | Hex | 用途 |
| --- | --- | --- |
| Sumi | `#14110F` | 本文の文字色、セクション表紙の背景 |
| Mizuki | `#F0E7D6` | 濃色背景の上の文字色 |
| Walnut | `#5B3B27` | 明色背景の差し色（`--color-accent`） |
| Keyaki | `#B98A52` | 濃色背景の差し色（`--color-accent-on-dark`） |
| Jindai | `#7A7268` | 補足テキスト、フッター |

背景の `#F7F2E9` と補足文字の `#56504A` は、yosegiのドキュメントサイト（`docs/.vitepress/theme/style.css`）と同じ値を使っている。

書体は英数字がNunito Sans、日本語がZen Kaku Gothic New（`--font-sans`）。等幅（`--font-mono`）はコードにだけ使う。

## カスタマイズ

`personal.css` の `:root` を書き換えると全体の印象を変えられる。

- `--color-accent` : 明色背景の差し色
- `--color-accent-on-dark` : セクション表紙（濃色背景）で使う差し色
- `--signature` : フッターに出す署名。デッキ単位で変えたい場合はfrontmatterで上書きする

```yaml
style: |
  section { --signature: 'dachi'; }
```

## ブランドアイコン

`packages/slides/src/assets/personal/` に各サービスの公式配布物をそのまま置いている。

| ファイル | 入手元 | 元のファイル名 |
| --- | --- | --- |
| `github-invertocat-black.svg` | <https://github.com/logos> の `GitHub_Logos.zip` | `SVG/GitHub_Invertocat_Black.svg` |
| `x-logo-black.png` | <https://about.x.com/en/who-we-are/brand-toolkit> の `x-logo.zip` | `logo-black.png` |
| `x-logo-white.svg` | 同上 | `logo.svg` |

- Xの公式配布物にはSVGが白しか含まれないため、明るい背景では `x-logo-black.png` を使う。
- 色の変更やパスの改変はブランドガイドラインに反するため行わない。
- 大きさと縦位置は `icon` クラスで揃うため、ロゴごとの調整は不要。
- 新しいサービスのアイコンが必要になったら、公式のブランドページから取得してこの表に追記する。
