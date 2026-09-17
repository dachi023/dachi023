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
<p class="version">[ ver.01 2026.02 ]</p>
<div class="cover-footer">
  <img class="cover-logo" src="./assets/logo-mosh.svg">
  <p class="description">MOSH develops and operates a platform that supports independent creators<br>in selling their services online.</p>
</div>
<p class="copyright">&copy; MOSH, Inc.</p>

# Branded Typesで「全部string」からの卒業

<p class="event-name">TSKaigi 2026</p>

---
<!-- _class: split -->

![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

<div class="split-left">

## dachi

MOSH株式会社。

- TypeScript
- Branded Types

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

Markdownファイルを加工するバッチ処理を題材に、**Branded Types**がどのような場面で便利かを紹介します。

- Branded Typesの仕組み
- 実際のコードでのBefore / After
- やり過ぎるとどうなるか

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 「全部string」問題

<p class="section-description">型検査が通っていても安全ではない</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 題材：Markdownバッチ処理

ファイル収集 → フィルタリング → Markdown正規化 → ファイル名整理 → メタデータ付与 → 出力

このスクリプトに登場する値、**ほぼ全部string型**。

- ファイルパス（入力用・出力用）
- コンテンツ（加工前・加工後）
- カテゴリ名、タイトル、スキップ理由

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## Before：パスの取り違え

```typescript
// sourcePath も outputPath も string
// 引数を逆に渡してもコンパイルは通る
function writeOutput(outputPath: string, content: string): void { ... }

writeOutput(sourcePath, content); // バグだが型エラーにならない
```

出力先に入力パスを渡してしまっても、**コンパイルは通ってしまう**。

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## Before：引数の入れ替え

```typescript
// title も category も string
function addFrontMatter(
  content: string, title: string, category: string
): string

addFrontMatter(content, category, title); // 逆でも通る
```

タイトルとカテゴリを取り違えても、**気づくのは実行後**。

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## Before：isEmptyPageの戻り値

```typescript
// empty が false のとき reason にアクセスしてもコンパイルは通る
function isEmptyPage(
  content: string
): { empty: boolean; reason?: string }
```

`empty: false` のときに `reason` を参照しても型エラーにならない。

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# Branded Typesとは

<p class="section-description">同じ型の値を区別する実装パターン</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 基本の仕組み

```typescript
type Brand<T, B extends string> = T & { readonly __brand: B };

type SourcePath = Brand<string, "SourcePath">;
type OutputPath = Brand<string, "OutputPath">;
```

- `__brand` プロパティは**実際には存在しない**（ランタイムコストゼロ）
- 型レベルでのみ区別される

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## スマートコンストラクタ

`as` を使ったキャストで値を作る。

```typescript
function toSourcePath(p: string): SourcePath {
  return p as SourcePath;
}

function toOutputPath(p: string): OutputPath {
  return p as OutputPath;
}
```

外部入力との**境界で一度だけキャスト**し、以降は型で守る。

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# After：Branded Types適用

<p class="section-description">取り違えをコンパイル時に検出</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## After：パスの区別

```typescript
type SourcePath = Brand<string, "SourcePath">;
type OutputPath = Brand<string, "OutputPath">;

function writeOutput(
  outputPath: OutputPath, content: string
): void { ... }

writeOutput(sourcePath, content); // コンパイルエラー！
```

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## After：タイトルとカテゴリの区別

```typescript
type DocumentTitle = Brand<string, "DocumentTitle">;
type Category = Brand<string, "Category">;

function addFrontMatter(
  content: string, title: DocumentTitle, category: Category
): string

addFrontMatter(content, category, title); // コンパイルエラー！
```

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## After：パイプラインの順序強制

```typescript
type RawMarkdown = Brand<string, "RawMarkdown">;
type NormalizedMarkdown = Brand<string, "NormalizedMarkdown">;
type MarkdownWithFrontMatter = Brand<string, "MarkdownWithFrontMatter">;

function normalizeMarkdown(
  content: RawMarkdown
): NormalizedMarkdown { ... }

function addFrontMatter(
  content: NormalizedMarkdown, ...
): MarkdownWithFrontMatter { ... }

// 加工前のコンテンツを渡すとエラー
addFrontMatter(rawContent, title, category); // コンパイルエラー！
```

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# 補足：組み合わせると便利な型テクニック

<p class="section-description">Branded Types以外のアプローチ</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## Discriminated Union

```typescript
// Before
{ empty: boolean; reason?: string }

// After
type EmptyPageResult =
  | { empty: true; reason: string }
  | { empty: false };
```

`empty: true` のときだけ `reason` が存在することが**型で保証**される。

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## Literal Union Types + exhaustiveness check

```typescript
type ImageExt = ".png" | ".jpg" | ".jpeg" | ".gif" | ".webp";
type VideoExt = ".mov" | ".mp4" | ".webm";

function classifyFile(ext: string):
  | { kind: "image" }
  | { kind: "video" }
  | { kind: "markdown" }
  | { kind: "unknown" }
```

新しい拡張子を追加したらswitch文で**未処理が検出**される。

---
<!-- _class: section-cover -->

![bg](./assets/bg-section-cover.svg)
<img class="logo" src="./assets/logo-mosh-white.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

# やり過ぎるとどうなるか

<p class="section-description">デメリットと適用範囲の判断基準</p>

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## デメリット・限界

- **asキャストの増加**：外部入力を受け取るたびにキャストが必要。境界が多いコードだとノイズになる
- **ライブラリとの相性**：`fs.readFile` は `string` を返すので、境界で毎回変換が発生
- **やり過ぎ例**：正規化の各ステップごとに型を分ける（`ImgTagsRemoved`, `AsideConverted`, ...）→ 型定義が爆発して本末転倒

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## 判断基準

適用すべき場面を**絞る**のが実用的。

- 同じ型の値が**2つ以上並ぶ**場面
- 取り違えたら**静かにバグになる**場面

これらに該当しない箇所にまで適用すると、キャストだらけで可読性が下がる。

---
![bg](./assets/bg-default.svg)
<img class="logo" src="./assets/logo-mosh.svg">
<p class="copyright">&copy; MOSH, Inc.</p>

## まとめ

- 「全部string」は型検査が通っていても**安全ではない**
- Branded Typesで意味の異なるstringを区別すれば、取り違えを**コンパイル時に防げる**
- **ランタイムコストゼロ**で導入できる
- 適用範囲は「取り違えが起きうる境界」に**絞るのが現実的**
- 型で守れるところは型に任せて、実行時のバグを減らそう
