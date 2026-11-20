# GDGL — Godot Galgame Language

GDGL 是一個給 Godot 遊戲引擎使用的 Galgame（視覺小說、文字冒險）劇本語言，以 C# 實作的簡易直譯式程式語言。

## 運作方式

一段 GDGL 原始碼從讀入到執行，會經過以下流程：

```
原始碼文字 → [Lexer] → Token 串 → [Parser] → 語法樹 → [直譯器] → 執行
```

| 階段 | 說明 | 實作 |
|---|---|---|
| Token | 語言中最小的有意義單位，例如名稱、字串、數字、括號 | `src/GDGL/Language/Tokens.cs`（自定義） |
| Lexer | 把原始碼文字切成 Token 串 | `src/GDGL/Language/Lexer.cs`（自行實作，繼承 Superpower 的 `Tokenizer`） |
| Parser | 依照語法規則，把 Token 串組成語法樹 | 使用 [Superpower](https://github.com/datalust/superpower) 函式庫（尚未實作） |
| 直譯器 | 走訪語法樹並直接執行 | 尚未實作 |

舉例來說，`func(a, b, c)` 經過 Lexer 會切成：

```
Identifier(func) LeftParen Identifier(a) Comma Identifier(b) Comma Identifier(c) RightParen
```

## 目前支援的 Token

- 名稱（Identifier）：以字母或 `_` 開頭，後接字母、數字或 `_`
- 字串（String）：以 `"` 包住，不可跨行，支援跳脫字元 `\"` `\\` `\n` `\r` `\t`
- 數字：整數（Integer）與浮點數（Float）
- 符號：`(` `)` `,`
- 註解：`#` 到行尾（目前只在名稱後面有作用）

## 開發進度

- [x] Token 定義
- [x] Lexer 第一版
- [ ] Lexer 支援換行、`+`、`:`
- [ ] Parser（Superpower）
- [ ] 直譯器
- [ ] 與 Godot 整合

## 專案結構

```
GDGL.slnx
src/GDGL/                 主程式庫
  Language/
    Tokens.cs             Token 種類定義
    Lexer.cs              詞法分析器
tests/GDGL.Tests/         單元測試（xUnit）
  Language/
    LexerTests.cs
```

## 建置與測試

需要 .NET 10 SDK。

```sh
dotnet build
dotnet test
```

## 授權

MIT
