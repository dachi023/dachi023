# AGENTS.md

dachi023の個人モノレポ。成果物はパッケージ単位で `packages/` に置く。

## 構成

| パス | 内容 |
| --- | --- |
| `packages/slides` | Marpで作った登壇スライド |
| `docs/` | 詳細ドキュメント |

## ドキュメント

作業前に該当するドキュメントを読むこと。このファイルには詳細を書かず、docs/配下に分割する。

| ドキュメント | 読むタイミング |
| --- | --- |
| [docs/repository.md](docs/repository.md) | リポジトリ構成、bun workspaceの運用、パッケージを追加するとき |
| [docs/writing.md](docs/writing.md) | 日本語の文章を書くとき、textlintのエラーに対応するとき |
| [docs/slides/overview.md](docs/slides/overview.md) | スライドパッケージを触るとき |
| [docs/slides/authoring.md](docs/slides/authoring.md) | スライドを新規作成・編集するとき |
| [docs/slides/theme-mosh.md](docs/slides/theme-mosh.md) | moshテーマのスライドを書くとき |
| [docs/slides/theme-personal.md](docs/slides/theme-personal.md) | personalテーマのスライドを書くとき |

## 基本ルール

- パッケージマネージャはbun。npm・yarn・pnpmのコマンドを使わない。
- コミットメッセージはConventional Commitsに従う。
- `README.md` はGitHubプロフィールに表示される。整備は保留中のため指示がない限り編集しない。
- 既存の成果物（スライド本文、テーマCSS、アセット）は指示なく書き換えない。
