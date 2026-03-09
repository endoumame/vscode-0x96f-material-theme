# ADR-002: tokenColors カラーパレットの確定

## ステータス

Accepted（ADR-001 を補足）

## コンテキスト

ADR-001 で BMT のカラースキームパターンを採用する方針を決定した後、初期実装では tokenColors 内で既に使われていた色（`#AEA3E6`, `#64D2E8`, `#C6E472`, `#FFD271` 等）を再利用していた。

しかし、これらの色は `colors` セクション（UI）のメインパレットとは微妙に異なる色（bright 系やバリエーション色）であり、テーマ全体の統一感に欠けていた。特に関数の色として `#64D2E8`（terminal.ansiBrightBlue）ではなく `#49CAE4`（アクセントカラー）の方がテーマの主要パレットと一貫性がある。

## 決定

tokenColors で使用するカラーパレットを、`colors` セクションのメインパレットに統一する。以下の 7 色 + 背景色をパレットとして定義する：

| 色 | 用途 | 出典 |
|---|---|---|
| `#FF7272` | red | terminal.ansiRed |
| `#BCDF59` | green | terminal.ansiGreen |
| `#FFCA58` | yellow | terminal.ansiYellow |
| `#49CAE4` | blue | terminal.ansiBlue / アクセントカラー |
| `#A093E2` | purple | terminal.ansiMagenta |
| `#AEE8F4` | cyan | terminal.ansiCyan |
| `#FCFCFA` | white | editor.foreground に近い |
| `#262427` | background | editor.background |

## 以前の色からの変更

| 旧 | 新 | 理由 |
|---|---|---|
| `#FCFCFC` | `#FCFCFA` | メインパレットに統一 |
| `#FF8787` | `#FF7272` | タグ等を赤に統一 |
| `#AEA3E6` | `#A093E2` | メインパレットの purple に統一 |
| `#64D2E8` | `#49CAE4` | メインパレットの blue に統一 |
| `#C6E472` | `#BCDF59` | メインパレットの green に統一 |
| `#FFD271` | `#FFCA58` | メインパレットの yellow に統一 |

## カラーコード表記規則

全てのカラーコードは大文字で統一する（例: `#49CAE4`、`#ff7272` ではなく `#FF7272`）。

## 結果

- tokenColors と colors セクションで同じパレットを共有するようになり、テーマ全体の統一感が向上した
- `#FF8787`（terminal.ansiBrightRed）を tokenColors から廃止し、赤系は `#FF7272` に一本化された
