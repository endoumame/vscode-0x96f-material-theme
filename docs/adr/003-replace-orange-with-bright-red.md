# ADR-003: orange 系カラー (#FC9D6F) を #FF8787 に変更

## ステータス

Accepted

## コンテキスト

ADR-002 で tokenColors のパレットを確定した際、orange 系の `#FC9D6F` は「変更なし」として残していた。この色は以下のスコープに使用されていた：

- Number, Constant（数値・定数・ユニット等）
- JSON Key - Level 2
- Markup - Underline

しかし、`#FC9D6F` は ADR-002 で定義した 7 色パレットに含まれておらず、パレット外の色が残ることでテーマの統一性が損なわれていた。

## 決定

tokenColors 内の `#FC9D6F`（orange）を `#FF8787`（bright red）に変更する。

## 変更対象

| スコープ | 変更前 | 変更後 |
|---|---|---|
| constant.numeric, constant.language 等 | `#FC9D6F` | `#FF8787` |
| JSON Key - Level 2 | `#FC9D6F` | `#FF8787` |
| markup.underline | `#FC9D6F` | `#FF8787` |

## 変更しないもの

`colors` セクション内の `#FC9D6F`（editorWarning.foreground, inputValidation.warningBorder, statusBar.debuggingBackground）はそのまま維持する。これらは UI 要素の警告色として適切であり、tokenColors のパレット統一とは無関係である。

## 結果

- tokenColors で使用される色が 7 色パレット + コメント用グレー（`#757075`）に完全に収まるようになった
- `#FF8787` は ADR-002 のパレットには含まれないが、`#FF7272` の bright バリエーションとして terminal.ansiBrightRed で使われている色であり、パレットとの親和性は高い
