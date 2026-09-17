# 文章のルール

日本語で書く。textlintで機械的にチェックできる範囲は `packages/slides/.textlintrc.json` に寄せている。

## textlint

```bash
bun run --filter '@dachi023/slides' lint
```

有効にしているプリセットは次の2つ。

- `preset-ja-spacing` : 全角と半角の間のスペースなど、表記の揺れ
- `preset-ja-technical-writing` : 一文の長さ、助詞の連続、二重否定など

以下のルールは意図的に無効化している。有効に戻さないこと。

| ルール | 無効にした理由 |
| --- | --- |
| `no-exclamation-question-mark` | スライドで問いかけを使うため |
| `ja-no-weak-phrase` | 「〜かもしれない」を意図して使う場面があるため |
| `ja-no-mixed-period` | 体言止めと言い切りを混ぜるため |
| `no-mix-dearu-desumasu` | 同上 |

数式やコード片で誤検知が起きる箇所はコメントで囲んで除外する。

```markdown
<!-- textlint-disable -->
検知させたくない行
<!-- textlint-enable -->
```

除外は最小範囲にとどめる。ファイル全体を `textlint-disable` で囲まない。

## 既知のエラー

移設済みの登壇スライドには、発表当時から残っているtextlintエラーがある。発表済みの内容を後から書き換えないため、そのままにしている。既存スライドを触るときも、指示がない限りこれらを直さない。

## 書き方の指針

- 1文を短くする。冗長な言い回しを削る。情報自体は削らない。
- 太字は本当に強調したい箇所だけに使う。見出しとリストで構造を作る。
- 具体を入れる。日付、件数、固有名詞を書く。
