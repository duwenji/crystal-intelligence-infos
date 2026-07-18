# Crystal Intelligence 学習教材 再編成設計

## 背景・目的

`crystal-intelligence-infos` は SB OAI Japan（ソフトバンク × OpenAI）の
「クリスタル・インテリジェンス」について、2026-07-18〜19にかけて作成された
学習教材である。00〜09の10ファイル・フラット構成で、追記のたびに
ファイル番号をリナンバーしてきた（git log上で3回確認）。

このままの構成では、今後も教材が追記され続けた場合（実績あり）に
リナンバーが繰り返され、リンク切れや構成の分かりにくさが蓄積する。
また、README自身が「補足」と位置付けている
`09-anthropic-comparison.md`（Anthropic陣営との比較、全体最大の300行）が
本編（01〜08）と同じ番号列に並んでおり、本編と補足の境界が構造上
表現されていない。

本設計では、内容そのものは書き換えず、**役割別フォルダへの再編成**によって
これらの構造的な問題を解消する。

## 現状の構成と問題点

```
crystal-intelligence-infos/
├── 00-README.md
├── 01-overview.md
├── 02-technical-architecture.md
├── 03-frontier-platform.md
├── 04-long-term-memory-deep-dive.md
├── 05-key-sources.md
├── 06-glossary.md
├── 07-open-questions.md
├── 08-understanding-the-enterprise.md
└── 09-anthropic-comparison.md
```

- ファイル追加のたびに全体リナンバーが発生している（03→04→05...の履歴）
- README「読み方の推奨順序」に05・09が含まれておらず、抜け漏れがある
- 08章の「8.0 企業を理解する手段の全体像（4層まとめ）」は02・03・04・07章を
  横断する教材全体のサマリーであり、章タイトル（メッセージングの発信経緯）と
  スコープがズレている
- 09（Anthropic比較）はREADME上で「補足」と明記されているが、番号上は
  01〜08と同格に並んでいる

## 新しい構成

```
crystal-intelligence-infos/
├── 00-README.md                          （全面改訂）
├── core/                                  Crystal Intelligence自体の技術説明
│   ├── 01-overview.md                     ← 01-overview.md
│   ├── 02-architecture.md                 ← 02-technical-architecture.md
│   ├── 03-frontier-platform.md            ← 03-frontier-platform.md
│   └── 04-long-term-memory.md             ← 04-long-term-memory-deep-dive.md
├── narrative/                             「企業を理解する」メッセージングの経緯
│   └── 01-understanding-the-enterprise.md ← 08-understanding-the-enterprise.md
├── reference/                             随時参照する資料
│   ├── 01-key-sources.md                  ← 05-key-sources.md
│   ├── 02-glossary.md                     ← 06-glossary.md
│   └── 03-open-questions.md               ← 07-open-questions.md
└── appendix/                              本編ではない補足
    └── 01-anthropic-comparison.md         ← 09-anthropic-comparison.md
```

フォルダ内は01始まりで独立採番する。これにより、今後どのフォルダに
追記があっても他フォルダのリナンバーは発生しない。

## README再設計

`00-README.md` を以下の構成に全面改訂する。

1. **前提と情報源の性質** — 既存内容を維持
2. **教材の全体像（4層まとめ）** — 旧 `08-understanding-the-enterprise.md` の
   8.0節（企業を理解する手段の4層まとめ表）をここに移設。表内リンクは新パスに更新
3. **教材マップ** — `core/` `narrative/` `reference/` `appendix/` 各フォルダの役割を
   1行ずつ説明する表
4. **読み方の推奨順序（読者タイプ別）** — 技術重視／メッセージング重視／用語引き／
   Anthropic比較興味あり、の4パターンを提示。全ファイル（05・09相当を含む）を
   もれなく参照する
5. **更新履歴** — 既存エントリを維持し、今回の再編成をエントリとして追記

`narrative/01-understanding-the-enterprise.md` からは8.0節を削除し、
「全体像は [00-README.md](../00-README.md) の4層まとめを参照」という
一文リンクに置き換える。

## 移動・リンク更新の方針

- `git mv` でファイルを移動し、リネームとして履歴を保持する
- フォルダを跨ぐ移動により、相互参照している相対リンクを全て新パスに張り替える
  （例: `05-key-sources.md` → `../reference/01-key-sources.md`）
- 移動・置換後、全 `.md` ファイルの `[text](path.md)` 形式のリンクを機械的に
  洗い出し、リンク先ファイルが実在するかを確認する。壊れたリンクがあれば修正する

## スコープ外

- 各ファイルの本文内容・見出し構成・mermaid図・既存の更新履歴エントリは、
  パスの張り替え以外変更しない
- 内容の要約・統合・書き直しは行わない（今回はあくまで構造の再編成）

## 完了条件

- 新しいフォルダ構成にすべてのファイルが `git mv` 済みである
- `00-README.md` が新構成・4層まとめ・読者別ガイドを反映して全面改訂されている
- 全ファイルの相互リンクが新パスで解決する（リンク切れがない）
- `narrative/01-understanding-the-enterprise.md` の8.0節がREADMEに移設され、
  元の場所には参照リンクのみが残っている
