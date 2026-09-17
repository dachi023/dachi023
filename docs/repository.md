# リポジトリ構成

dachi023の個人モノレポ。GitHubプロフィールに表示される `README.md` を兼ねる。

## ディレクトリ

```
.
├── package.json          # ワークスペースのルート
├── AGENTS.md             # エージェント向けの入口
├── CLAUDE.md             # AGENTS.mdの参照のみ
├── README.md             # GitHubプロフィール（整備は保留中）
├── docs/                 # 詳細ドキュメント
│   ├── repository.md
│   ├── writing.md
│   └── slides/
└── packages/
    └── slides/           # Marp登壇スライド
```

## bun workspace

パッケージマネージャはbunを使う。ルートの `package.json` で `workspaces: ["packages/*"]` を宣言している。

```bash
# 依存のインストール（ルートで実行する）
bun install

# 全パッケージのlint
bun run lint

# 特定パッケージのスクリプトを実行
bun run --filter '@dachi023/slides' build
```

- 依存は各パッケージの `package.json` に書く。ルートには開発ツールを含めて置かない。
- `bun install` はルートで実行する。パッケージディレクトリで実行しない。
- ロックファイルは `bun.lock` のみ。`package-lock.json` などを生成しない。

## パッケージを追加する

1. `packages/<name>/` を作る。
2. `packages/<name>/package.json` を作る。`name` は `@dachi023/<name>`、`private` は `true` にする。
3. ルートで `bun install` を実行してワークスペースに認識させる。
4. `docs/` にそのパッケージのドキュメントを追加し、`AGENTS.md` の表に1行足す。

`AGENTS.md` は入口として薄く保つ。手順や仕様はdocs/配下に書く。
