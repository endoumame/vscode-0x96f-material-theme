# ADR-004: BMT とのスコープ差異の修正

## ステータス

Accepted

## コンテキスト

BMT のカラースキームパターンを採用（ADR-001）した後、実際の使用において「クラス定数を関数の引数に渡した時だけシアン系の色になる」という問題が発見された。

調査の結果、以下の 2 つの複合要因が原因であった：

1. **`meta.function-call` スコープの過剰な範囲**: 0x96f の「Function, Special Method」エントリに `meta.function-call` が含まれていたが、BMT にはこのスコープがなかった。`meta.function-call` は関数呼び出し全体（引数含む）にマッチするため、引数内の定数の色がシアンで上書きされていた。

2. **`variable.other.constant` スコープの欠落**: VS Code はクラス定数に `variable.other.constant` スコープを割り当てるが、0x96f にはこのエントリが存在しなかった。BMT では `#F07178`（赤系）が割り当てられている。

さらに包括的な比較により、他にも以下の差異が発見された。

## 決定

BMT との差異を以下のように修正する。

### 修正 1: `meta.function-call` の削除

「Function, Special Method」エントリから `meta.function-call` を削除する。BMT にもこのスコープは含まれていない。

**理由**: `meta.function-call` は関数呼び出しのコンテキスト全体にマッチする非常に広いスコープであり、引数内のトークン（定数、変数等）の色を意図せず上書きしてしまう。

### 修正 2: `variable.other.constant` の追加

`variable.other.constant` スコープに `#FF7272`（赤）を割り当てる新規エントリを追加する。BMT では `#F07178`（赤系）。

### 修正 3: `meta.tag` の削除

「Operator, Misc」エントリから `meta.tag` を削除する。BMT の同等エントリにはこのスコープが含まれていない。Tag エントリ（`entity.name.tag`）で十分にカバーされている。

### 修正 4: Block Level Variables の修正

スコープを `meta.block variable.other`（全言語対象）から `source.cpp meta.block variable.other`（C++ 限定）に変更し、色を `#FCFCFA`（白）から `#FF7272`（赤）に変更する。BMT の設定に合わせた。

### 修正 5: 欠落エントリの追加

| エントリ | スコープ | 色 | BMT 対応色 |
|---|---|---|---|
| Invalid deprecated | `invalid.deprecated` | `#A093E2` (紫) | `#C792EA` (紫) |
| PHP Constants | `constant.other.php` | `#FFCA58` (黄) | `#FFCB6B` (黄) |

## 結果

- クラス定数が関数引数内でシアンに上書きされる問題が解消された
- テーマ設計における「広すぎるスコープを避ける」という原則が確立された
