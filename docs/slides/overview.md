# スライドパッケージ

`packages/slides` は登壇スライドを置くパッケージ。Markdownで書き、[Marp](https://marp.app/) でPDFに変換する。

## ディレクトリ

```
packages/slides/
├── package.json
├── .textlintrc.json
├── themes/
│   ├── mosh.css        # MOSH名義のテーマ
│   └── personal.css    # 個人名義のテーマ
├── src/
│   ├── *.md            # スライド本体
│   └── assets/
│       ├── bg-*.svg            # moshテーマの背景
│       ├── logo-mosh*.svg      # MOSHロゴ
│       ├── logo-x.svg          # Xアイコン（moshテーマ用・白）
│       ├── profile.jpg         # プロフィール画像
│       ├── profile-placeholder.svg
│       ├── personal/           # 個人テーマ用のブランドアイコン
│       └── <スライド名>/        # そのスライドでしか使わない画像
└── dist/               # ビルド出力（PDF、gitignore）
```

## コマンド

`packages/slides` で実行する。ルートからは `bun run --filter '@dachi023/slides' <script>` を使う。

| コマンド | 内容 |
| --- | --- |
| `bun run preview` | プレビューウィンドウを開いて確認する |
| `bun run dev` | サーバーのみ起動する（<http://localhost:8080>） |
| `bun run build` | `dist/` にPDFを出力する |
| `bun run lint` | `src/*.md` をtextlintでチェックする |

個別のファイルだけ変換する場合。

```bash
bunx @marp-team/marp-cli --theme-set themes/mosh.css themes/personal.css --html --pdf \
  src/<スライド名>.md -o dist/<スライド名>.pdf
```

## アセットの規約

- 画像の参照は `./assets/` からの相対パスで書く。
- 複数のスライドで使う画像は `assets/` 直下、そのスライド専用の画像は `assets/<スライド名>/` に置く。
- `assets/` 配下に `.md` を置かない。`marp --input-dir src/` が再帰的に拾ってビルド対象にしてしまう。
- 既存のアセットとテーマCSSは編集・削除しない。

## やってはいけないこと

- `themes/mosh.css` を編集する（テーマの統一性が崩れる）
- 発表済みスライドの内容を指示なく書き換える
- personalテーマのスライドにMOSHのロゴ・コピーライト・背景画像を混ぜる
- moshテーマのスライドにpersonalテーマ専用のクラス（`statement`、`cols` など）を使う
