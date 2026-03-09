# ADR-005: fontStyle (italic) の適用方針

## ステータス

Accepted

## コンテキスト

テーマ開発の過程で、fontStyle の italic に関して以下の 3 段階の変遷があった：

### フェーズ 1: BMT パターンでの初期実装

ADR-001 の初期実装時に、BMT に合わせて以下のスコープに italic を追加した：
- Comment, Keyword/Storage, keyword.control, variable.language, entity.name.method.js, entity.other.attribute-name, Decorators, ES7 Bind Operator

### フェーズ 2: italic の全削除

「italic を全て戻して」という要望により、シンタックス要素の italic を全て削除した。ただし、Markup 系（markup.italic, markup.quote 等）は意味的なスタイルとして維持した。

### フェーズ 3: BMT 準拠での再追加

「BMT と同じスコープに italic を追加して」という要望により、BMT で italic が設定されている全スコープに再度 italic を追加した。

## 決定

BMT と同一のスコープに対してのみ italic を適用する。適用対象は以下の通り：

### foreground + italic（色の定義と同じエントリ内）

| スコープ | 色 |
|---|---|
| `comment`, `punctuation.definition.comment` | `#757075` |
| `variable.language` | `#FF7272` |
| `entity.name.method.js` | `#49CAE4` |
| `entity.other.attribute-name` | `#A093E2` |
| Decorators (`tag.decorator.js ...`) | `#49CAE4` |
| ES7 Bind Operator | `#FF7272` |

### fontStyle のみのエントリ（色は定義しない）

| スコープ | 説明 |
|---|---|
| `Keyword`, `Storage` | Keyword/Storage の色エントリとは別に、italic のみを付与するエントリ |
| `keyword.control` | Operator, Misc の色エントリとは別に、italic のみを付与するエントリ |

### Markup 系（意味的なスタイルとして常に維持）

| スコープ | fontStyle |
|---|---|
| `markup.italic` | italic |
| `markup.bold`, `markup.bold string` | bold |
| `markup.bold markup.italic` 等 | bold |
| `markup.underline` | underline |
| `markup.quote` | italic |
| `meta.separator` | bold |

## fontStyle のみのエントリの仕組み

VS Code はスコープのマッチ度が同じ場合、後に定義されたエントリの設定をマージする。「Keyword, Storage Italic」（fontStyle のみ）と「Keyword, Storage」（foreground のみ）は別エントリだが、両方が適用されることで「紫色 + italic」という最終結果になる。

## 結果

- BMT と同一の視覚的体験を提供しつつ、0x96f 独自のカラーパレットを維持する
- Markup 系と装飾的な italic が明確に区別され、各エントリの役割が明確になった
