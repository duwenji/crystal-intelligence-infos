# Crystal Intelligence 学習教材 再編成 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** `crystal-intelligence-infos` をフラットな00〜09連番構成から、役割別フォルダ（core/narrative/reference/appendix）構成に再編成する。内容は書き換えず、ファイル配置とリンクのみを変更する。

**Architecture:** `git mv` でファイルを4フォルダに移動し、フォルダを跨ぐ相対リンクを `sed` で一括張り替えする。旧8.0節（全体まとめ表）は `narrative/` から `00-README.md` に移設し、README全体を新構成に合わせて全面改訂する。最後に全リンクの解決を検証する。

**Tech Stack:** Markdown, git (mv によるリネーム追跡), GNU sed（Git Bash同梱, 動作確認済み: v4.9）

## Global Constraints

- 対象リポジトリ: `c:\Dev\tutorials\crystal-intelligence-infos`（既存のgitリポジトリ、初期状態でクリーン）
- 各ファイルの本文内容・見出し構成・mermaid図・既存の更新履歴エントリは、パスの張り替え以外変更しない
- ファイル移動は `git mv` を用いてリネームとして記録する（新規作成+削除にしない）
- 全ファイルの相互リンクが、全タスク完了時点ですべて解決すること（壊れたリンクを残さない）
- 各タスック完了ごとにコミットする

---

### Task 1: フォルダ作成とファイル移動

**Files:**
- Create (ディレクトリ): `core/`, `narrative/`, `reference/`, `appendix/`
- Move: `01-overview.md` → `core/01-overview.md`
- Move: `02-technical-architecture.md` → `core/02-architecture.md`
- Move: `03-frontier-platform.md` → `core/03-frontier-platform.md`
- Move: `04-long-term-memory-deep-dive.md` → `core/04-long-term-memory.md`
- Move: `08-understanding-the-enterprise.md` → `narrative/01-understanding-the-enterprise.md`
- Move: `05-key-sources.md` → `reference/01-key-sources.md`
- Move: `06-glossary.md` → `reference/02-glossary.md`
- Move: `07-open-questions.md` → `reference/03-open-questions.md`
- Move: `09-anthropic-comparison.md` → `appendix/01-anthropic-comparison.md`

**Interfaces:**
- Produces: 新しいディレクトリレイアウト（Task 2以降が参照する最終ファイルパス）。`00-README.md` はルートに残る（このタスクでは触らない）

- [ ] **Step 1: フォルダを作成しファイルを移動する**

```bash
cd "c:\Dev\tutorials\crystal-intelligence-infos"
mkdir -p core narrative reference appendix
git mv 01-overview.md core/01-overview.md
git mv 02-technical-architecture.md core/02-architecture.md
git mv 03-frontier-platform.md core/03-frontier-platform.md
git mv 04-long-term-memory-deep-dive.md core/04-long-term-memory.md
git mv 08-understanding-the-enterprise.md narrative/01-understanding-the-enterprise.md
git mv 05-key-sources.md reference/01-key-sources.md
git mv 06-glossary.md reference/02-glossary.md
git mv 07-open-questions.md reference/03-open-questions.md
git mv 09-anthropic-comparison.md appendix/01-anthropic-comparison.md
```

- [ ] **Step 2: 移動結果を確認する**

Run: `git status --short`
Expected: 9行すべてが `R  ` (rename) で始まる。例:
```
R  01-overview.md -> core/01-overview.md
R  02-technical-architecture.md -> core/02-architecture.md
R  03-frontier-platform.md -> core/03-frontier-platform.md
R  04-long-term-memory-deep-dive.md -> core/04-long-term-memory.md
R  05-key-sources.md -> reference/01-key-sources.md
R  06-glossary.md -> reference/02-glossary.md
R  07-open-questions.md -> reference/03-open-questions.md
R  08-understanding-the-enterprise.md -> narrative/01-understanding-the-enterprise.md
R  09-anthropic-comparison.md -> appendix/01-anthropic-comparison.md
```
`00-README.md` は一覧に含まれないこと（未変更のまま）。

- [ ] **Step 3: コミットする**

```bash
git commit -m "$(cat <<'EOF'
Move chapters into role-based folders (core/narrative/reference/appendix)

Rename-only commit; content and links are unchanged in this step and
will be fixed in follow-up commits.

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 2: core/ 配下のリンクを新パスに張り替える

**Files:**
- Modify: `core/01-overview.md`
- Modify: `core/02-architecture.md`
- Modify: `core/03-frontier-platform.md`
- Modify: `core/04-long-term-memory.md`

**Interfaces:**
- Consumes: Task 1で確定した新ファイルパス一覧
- Produces: core/配下4ファイルのリンクが全て新パスで解決する状態

- [ ] **Step 1: core/01-overview.md のリンクを張り替える**

```bash
cd "c:\Dev\tutorials\crystal-intelligence-infos"
sed -i \
  -e 's#](07-open-questions\.md)#](../reference/03-open-questions.md)#g' \
  -e 's#](05-key-sources\.md)#](../reference/01-key-sources.md)#g' \
  -e 's#](02-technical-architecture\.md)#](02-architecture.md)#g' \
  core/01-overview.md
```

- [ ] **Step 2: core/02-architecture.md のリンクを張り替える**

```bash
sed -i \
  -e 's#](08-understanding-the-enterprise\.md)#](../narrative/01-understanding-the-enterprise.md)#g' \
  core/02-architecture.md
```

- [ ] **Step 3: core/03-frontier-platform.md のリンクを張り替える**

```bash
sed -i \
  -e 's#](02-technical-architecture\.md)#](02-architecture.md)#g' \
  -e 's#](04-long-term-memory-deep-dive\.md)#](04-long-term-memory.md)#g' \
  -e 's#](06-glossary\.md)#](../reference/02-glossary.md)#g' \
  -e 's#](05-key-sources\.md)#](../reference/01-key-sources.md)#g' \
  -e 's#](09-anthropic-comparison\.md)#](../appendix/01-anthropic-comparison.md)#g' \
  core/03-frontier-platform.md
```

- [ ] **Step 4: core/04-long-term-memory.md のリンクを張り替える**

```bash
sed -i \
  -e 's#](02-technical-architecture\.md)#](02-architecture.md)#g' \
  -e 's#](08-understanding-the-enterprise\.md)#](../narrative/01-understanding-the-enterprise.md)#g' \
  -e 's#](05-key-sources\.md)#](../reference/01-key-sources.md)#g' \
  core/04-long-term-memory.md
```

- [ ] **Step 5: 旧スタイルのリンクが残っていないか確認する**

Run:
```bash
grep -noE '\]\((0[1-9]-[a-z-]+|00-README)\.md\)' core/*.md
```
Expected: 出力なし（旧ファイル名を指すリンクが1件も残っていない）

- [ ] **Step 6: 新パスのリンクが解決するか確認する**

```bash
for f in core/*.md; do
  dir=$(dirname "$f")
  grep -oE '\]\([^)]+\.md\)' "$f" | sed -E 's/^\]\(//; s/\)$//' | while read -r link; do
    [ -f "$dir/$link" ] || echo "BROKEN in $f: $link"
  done
done
```
Expected: 出力なし

- [ ] **Step 7: コミットする**

```bash
git add core/
git commit -m "$(cat <<'EOF'
Fix cross-file links in core/ after folder move

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 3: reference/ 配下のリンクを新パスに張り替える

**Files:**
- Modify: `reference/01-key-sources.md`
- Modify: `reference/02-glossary.md`
- Modify: `reference/03-open-questions.md`

**Interfaces:**
- Consumes: Task 1で確定した新ファイルパス一覧
- Produces: reference/配下3ファイルのリンクが全て新パスで解決する状態

- [ ] **Step 1: reference/01-key-sources.md のリンクを張り替える**

```bash
cd "c:\Dev\tutorials\crystal-intelligence-infos"
sed -i \
  -e 's#](03-frontier-platform\.md)#](../core/03-frontier-platform.md)#g' \
  -e 's#](04-long-term-memory-deep-dive\.md)#](../core/04-long-term-memory.md)#g' \
  -e 's#](06-glossary\.md)#](02-glossary.md)#g' \
  reference/01-key-sources.md
```

- [ ] **Step 2: reference/02-glossary.md のリンクを張り替える**

```bash
sed -i \
  -e 's#](03-frontier-platform\.md)#](../core/03-frontier-platform.md)#g' \
  -e 's#](04-long-term-memory-deep-dive\.md)#](../core/04-long-term-memory.md)#g' \
  -e 's#](09-anthropic-comparison\.md)#](../appendix/01-anthropic-comparison.md)#g' \
  -e 's#](07-open-questions\.md)#](03-open-questions.md)#g' \
  reference/02-glossary.md
```

- [ ] **Step 3: reference/03-open-questions.md のリンクを張り替える**

```bash
sed -i \
  -e 's#](03-frontier-platform\.md)#](../core/03-frontier-platform.md)#g' \
  -e 's#](04-long-term-memory-deep-dive\.md)#](../core/04-long-term-memory.md)#g' \
  -e 's#](00-README\.md)#](../00-README.md)#g' \
  reference/03-open-questions.md
```

- [ ] **Step 4: 旧スタイルのリンクが残っていないか確認する**

```bash
grep -noE '\]\((0[1-9]-[a-z-]+|00-README)\.md\)' reference/*.md
```
Expected: 出力なし

- [ ] **Step 5: 新パスのリンクが解決するか確認する**

```bash
for f in reference/*.md; do
  dir=$(dirname "$f")
  grep -oE '\]\([^)]+\.md\)' "$f" | sed -E 's/^\]\(//; s/\)$//' | while read -r link; do
    [ -f "$dir/$link" ] || echo "BROKEN in $f: $link"
  done
done
```
Expected: 出力なし

- [ ] **Step 6: コミットする**

```bash
git add reference/
git commit -m "$(cat <<'EOF'
Fix cross-file links in reference/ after folder move

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 4: narrative/01 から8.0節を移設し、残りのリンクを張り替える

**Files:**
- Modify: `narrative/01-understanding-the-enterprise.md`

**Interfaces:**
- Consumes: Task 1で確定した新ファイルパス一覧
- Produces: `narrative/01-understanding-the-enterprise.md` から8.0節（4層まとめ表）が除去され、`00-README.md`への参照一文に置き換わった状態。この節のテキストはTask 6で `00-README.md` に転記する

- [ ] **Step 1: 8.0節を参照一文に置き換える**

`narrative/01-understanding-the-enterprise.md` を編集し、以下の `old_string` を `new_string` に置き換える（Editツール使用）。

old_string:
```
## 8.0 企業を理解する手段の全体像（4層まとめ）

これまでの調査（02章・03章・04章・本章）で判明した「企業を理解する手段」を、
レイヤーごとに横断整理すると以下の4層になります。個別の詳細は各節・各章を参照してください。

| レイヤー | 手段 | 中身 | 出典・詳細 |
|---|---|---|---|
| ①メッセージング（Crystal Intelligence側の公式説明） | 長期記憶 | 企業専用の「クリスタル」に蓄積された情報をもとに問題解決。特許取得済み | 8.1(1)節、8.2節 |
| ①同上 | 基幹システムのソースコード解析 | 長年運用されたコードを全解析し、機能だけでなく「意図」まで理解 | 8.1(2)節 |
| ①同上 | 社内情報への継続アクセス | 全社会議に参加し、議事録・メール・資料・仕様書を横断参照 | 8.1(3)節 |
| ②基盤技術（Frontierの機能） | Business Context | CRM・ERP・データウェアハウス等を接続し、人間と同じ情報にエージェントがアクセス | [03章](03-frontier-platform.md) 3.2節 |
| ②同上 | Task Memory | 単一タスク実行中の中間推論・ツール結果・人間フィードバックを保持（短期） | [04章](04-long-term-memory-deep-dive.md) 4.1節 |
| ②同上 | Organizational Memory | タスク・エージェント横断の共有知識ベース（業務ルール・過去の意思決定記録） | [04章](04-long-term-memory-deep-dive.md) 4.1節 |
| ②同上 | Learning Loop | 人間フィードバック（承認/修正/却下）に基づき、企業データ境界内で継続的に振る舞いを調整 | [04章](04-long-term-memory-deep-dive.md) 4.1節 |
| ③人的手段（実装フェーズ） | FDE（Forward Deployed Engineer） | OpenAI・ソフトバンクのエンジニアが伴走し、業務整理・データ統合・セキュリティ設計・実装を支援 | [02章](02-technical-architecture.md) 2.4節 |
| ③同上 | 企業ごとのファインチューニング | 安全な環境で企業固有データを追加学習させ、専用エージェントを構築 | 8.5節ログミー出典 |
| ④非公開・未確認の手段 | ナレッジグラフ | 公式資料に言及なし。第三者専門家は「本来は必要」と提言するのみで採用有無は不明 | [04章](04-long-term-memory-deep-dive.md) 4.7節 |
| ④同上 | メタデータ形式・検索アルゴリズム | 非公開 | [04章](04-long-term-memory-deep-dive.md) 4.5節 |

**まとめると**: ①は顧客企業向けの「機能・効用レベルの説明」、②はそれを支えるFrontier側の
技術コンセプト、③は導入時の人的支援体制であり、この3層が組み合わさって
「企業を理解するAI」というメッセージングを構成しています。④に該当する実装の中身
（データ構造・アルゴリズム）は、本教材作成時点では依然として非公開です。

## 8.1 「企業を理解する」の3つの技術的根拠
```

new_string:
```
教材全体を横断する「企業を理解する手段の4層まとめ」は
[00-README.md](../00-README.md) の「教材の全体像」に移設しました。

## 8.1 「企業を理解する」の3つの技術的根拠
```

- [ ] **Step 2: 削除できたことを確認する**

Run: `grep -n "8.0" narrative/01-understanding-the-enterprise.md`
Expected: 出力なし（8.0見出しが残っていない）

- [ ] **Step 3: 残りのリンクを新パスに張り替える**

```bash
cd "c:\Dev\tutorials\crystal-intelligence-infos"
sed -i \
  -e 's#](03-frontier-platform\.md)#](../core/03-frontier-platform.md)#g' \
  -e 's#](04-long-term-memory-deep-dive\.md)#](../core/04-long-term-memory.md)#g' \
  -e 's#](02-technical-architecture\.md)#](../core/02-architecture.md)#g' \
  -e 's#](00-README\.md)#](../00-README.md)#g' \
  narrative/01-understanding-the-enterprise.md
```

- [ ] **Step 4: 旧スタイルのリンクが残っていないか確認する**

```bash
grep -noE '\]\((0[1-9]-[a-z-]+|00-README)\.md\)' narrative/*.md
```
Expected: 出力なし

- [ ] **Step 5: 新パスのリンクが解決するか確認する**

```bash
for f in narrative/*.md; do
  dir=$(dirname "$f")
  grep -oE '\]\([^)]+\.md\)' "$f" | sed -E 's/^\]\(//; s/\)$//' | while read -r link; do
    [ -f "$dir/$link" ] || echo "BROKEN in $f: $link"
  done
done
```
Expected: 出力なし

- [ ] **Step 6: コミットする**

```bash
git add narrative/
git commit -m "$(cat <<'EOF'
Move 4-layer summary out of narrative/01 and fix its remaining links

Section 8.0 duplicated a whole-book synthesis inside a chapter titled
around messaging timeline; it moves to 00-README.md in Task 6 where its
scope actually matches the document.

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 5: appendix/ 配下のリンクを新パスに張り替える

**Files:**
- Modify: `appendix/01-anthropic-comparison.md`

**Interfaces:**
- Consumes: Task 1で確定した新ファイルパス一覧
- Produces: `appendix/01-anthropic-comparison.md` のリンクが全て新パスで解決する状態

- [ ] **Step 1: リンクを張り替える**

```bash
cd "c:\Dev\tutorials\crystal-intelligence-infos"
sed -i \
  -e 's#](03-frontier-platform\.md)#](../core/03-frontier-platform.md)#g' \
  -e 's#](06-glossary\.md)#](../reference/02-glossary.md)#g' \
  -e 's#](07-open-questions\.md)#](../reference/03-open-questions.md)#g' \
  appendix/01-anthropic-comparison.md
```

- [ ] **Step 2: 旧スタイルのリンクが残っていないか確認する**

```bash
grep -noE '\]\((0[1-9]-[a-z-]+|00-README)\.md\)' appendix/*.md
```
Expected: 出力なし

- [ ] **Step 3: 新パスのリンクが解決するか確認する**

```bash
for f in appendix/*.md; do
  dir=$(dirname "$f")
  grep -oE '\]\([^)]+\.md\)' "$f" | sed -E 's/^\]\(//; s/\)$//' | while read -r link; do
    [ -f "$dir/$link" ] || echo "BROKEN in $f: $link"
  done
done
```
Expected: 出力なし

- [ ] **Step 4: コミットする**

```bash
git add appendix/
git commit -m "$(cat <<'EOF'
Fix cross-file links in appendix/ after folder move

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 6: 00-README.md を新構成に合わせて全面改訂する

**Files:**
- Modify: `00-README.md`

**Interfaces:**
- Consumes: Task 4で `narrative/01-understanding-the-enterprise.md` から除去した8.0節のテキスト（このタスクでREADMEに転記する）。Task 1〜5で確定した全ファイルの最終パス
- Produces: 新しい教材の入口となる `00-README.md`

- [ ] **Step 1: 00-README.md を以下の内容で上書きする**

`00-README.md` の内容全体を次のとおり置き換える（Writeツール使用。既存の「前提と情報源の性質」節と「更新履歴」の既存9件は文言をそのまま維持し、「教材の全体像」「教材マップ」「読み方の推奨順序」を新設、更新履歴に1件追記）。

```markdown
# Crystal Intelligence 学習教材

このフォルダは、SB OAI Japan（ソフトバンク × OpenAI の合弁会社）が提供する
エンタープライズ向けAIサービス「クリスタル・インテリジェンス（Crystal Intelligence）」について、
公式発表・技術解説をもとに体系的に理解するための教材です。

## 前提と情報源の性質

Crystal Intelligence は一般消費者向けプロダクトではなく、企業向けに個別導入される
エンタープライズAIサービスです。そのため、一般的なOSSプロジェクトのように
公開APIリファレンスやGitHubリポジトリは存在せず、情報は以下の3系統に限定されます。

1. **SB OAI Japan / OpenAI の公式発表・プレスリリース**（一次情報、事実ベース）
2. **ソフトバンクのTech Blog / Insightsページ**（アーキテクチャやユースケースの解説、二次情報）
3. **データ管理・AI領域の専門家による技術分析記事**（第三者による技術的な解釈・批評、三次情報。
   基盤であるOpenAI Frontierの機能を深掘りする際に多く登場する）

この教材では、これらの情報を整理し、不足している部分（公開APIやOSS実装、詳細なメタデータ形式など）は
「非公開・現時点では不明」として明示します。**推測や一般論で技術仕様を補完している箇所は
「※推定」と明記**し、一次情報と混同しないようにしています。

## 教材の全体像（4層まとめ）

これまでの調査（[core/02](core/02-architecture.md)・[core/03](core/03-frontier-platform.md)・
[core/04](core/04-long-term-memory.md)・[narrative/01](narrative/01-understanding-the-enterprise.md)）で
判明した「企業を理解する手段」を、レイヤーごとに横断整理すると以下の4層になります。
個別の詳細は各節・各章を参照してください。

| レイヤー | 手段 | 中身 | 出典・詳細 |
|---|---|---|---|
| ①メッセージング（Crystal Intelligence側の公式説明） | 長期記憶 | 企業専用の「クリスタル」に蓄積された情報をもとに問題解決。特許取得済み | [narrative/01](narrative/01-understanding-the-enterprise.md) 8.1(1)節、8.2節 |
| ①同上 | 基幹システムのソースコード解析 | 長年運用されたコードを全解析し、機能だけでなく「意図」まで理解 | [narrative/01](narrative/01-understanding-the-enterprise.md) 8.1(2)節 |
| ①同上 | 社内情報への継続アクセス | 全社会議に参加し、議事録・メール・資料・仕様書を横断参照 | [narrative/01](narrative/01-understanding-the-enterprise.md) 8.1(3)節 |
| ②基盤技術（Frontierの機能） | Business Context | CRM・ERP・データウェアハウス等を接続し、人間と同じ情報にエージェントがアクセス | [core/03](core/03-frontier-platform.md) 3.2節 |
| ②同上 | Task Memory | 単一タスク実行中の中間推論・ツール結果・人間フィードバックを保持（短期） | [core/04](core/04-long-term-memory.md) 4.1節 |
| ②同上 | Organizational Memory | タスク・エージェント横断の共有知識ベース（業務ルール・過去の意思決定記録） | [core/04](core/04-long-term-memory.md) 4.1節 |
| ②同上 | Learning Loop | 人間フィードバック（承認/修正/却下）に基づき、企業データ境界内で継続的に振る舞いを調整 | [core/04](core/04-long-term-memory.md) 4.1節 |
| ③人的手段（実装フェーズ） | FDE（Forward Deployed Engineer） | OpenAI・ソフトバンクのエンジニアが伴走し、業務整理・データ統合・セキュリティ設計・実装を支援 | [core/02](core/02-architecture.md) 2.4節 |
| ③同上 | 企業ごとのファインチューニング | 安全な環境で企業固有データを追加学習させ、専用エージェントを構築 | [narrative/01](narrative/01-understanding-the-enterprise.md) 8.5節ログミー出典 |
| ④非公開・未確認の手段 | ナレッジグラフ | 公式資料に言及なし。第三者専門家は「本来は必要」と提言するのみで採用有無は不明 | [core/04](core/04-long-term-memory.md) 4.7節 |
| ④同上 | メタデータ形式・検索アルゴリズム | 非公開 | [core/04](core/04-long-term-memory.md) 4.5節 |

**まとめると**: ①は顧客企業向けの「機能・効用レベルの説明」、②はそれを支えるFrontier側の
技術コンセプト、③は導入時の人的支援体制であり、この3層が組み合わさって
「企業を理解するAI」というメッセージングを構成しています。④に該当する実装の中身
（データ構造・アルゴリズム）は、本教材作成時点では依然として非公開です。

## 教材マップ

| フォルダ | 内容 |
|---|---|
| [core/](core/) | Crystal Intelligence自体の技術説明。[01-overview](core/01-overview.md)（概要）→[02-architecture](core/02-architecture.md)（アーキテクチャ）→[03-frontier-platform](core/03-frontier-platform.md)（基盤Frontier）→[04-long-term-memory](core/04-long-term-memory.md)（長期記憶の深掘り）の順に深掘りする4本 |
| [narrative/](narrative/) | 「企業を理解する」というメッセージングの発信経緯・タイムライン。[01-understanding-the-enterprise](narrative/01-understanding-the-enterprise.md) |
| [reference/](reference/) | 随時参照する資料。[01-key-sources](reference/01-key-sources.md)（情報源一覧）・[02-glossary](reference/02-glossary.md)（用語集）・[03-open-questions](reference/03-open-questions.md)（未解決の調査論点） |
| [appendix/](appendix/) | 本編ではない補足。[01-anthropic-comparison](appendix/01-anthropic-comparison.md)（Anthropic陣営との比較） |

## 読み方の推奨順序（読者タイプ別）

- **技術を理解したい人**: [core/01](core/01-overview.md) → [core/02](core/02-architecture.md) → [core/03](core/03-frontier-platform.md) → [core/04](core/04-long-term-memory.md) → 本ページの「教材の全体像」に戻って全体を俯瞰する
- **「企業を理解する」の裏付けが知りたい人**: 本ページの「教材の全体像」→ [narrative/01](narrative/01-understanding-the-enterprise.md) → [core/04](core/04-long-term-memory.md)
- **用語を都度引きたい人**: [reference/02-glossary](reference/02-glossary.md) を1〜5と並行して参照
- **さらに深く知りたい人**: [reference/03-open-questions](reference/03-open-questions.md) を起点に追加調査する。情報源は [reference/01-key-sources](reference/01-key-sources.md) を参照
- **Anthropicとの違いに興味がある人**: core一式を読んだ後に [appendix/01-anthropic-comparison](appendix/01-anthropic-comparison.md)

## 更新履歴

- 2026-07-18: 初版作成（ユーザー提供情報をもとに構成）
- 2026-07-18: Frontierの体系的な章（03-frontier-platform.md）を追加。OpenAI公式発表・SoftBank公式サイトの一次情報を調査の上で構成し、既存ファイルを03〜05から04〜06に採番し直した
- 2026-07-18: 長期記憶（04-long-term-memory-deep-dive.md）の深掘り章を追加。Task Memory/Organizational Memory/Learning Loopの概念、メタデータ形式が非公開である点、第三者専門家による批判的視点を、複数の技術ブログを調査の上で整理し、既存ファイルを04〜06から05〜07に採番し直した
- 2026-07-18: 「企業を理解する」メッセージングの調査章（08-understanding-the-enterprise.md）を追加。2025年2月の初回発表から2026年7月のSoftBank World 2026までの発信タイムラインと、長期記憶に関する特許取得の事実を整理。SoftBank World 2026メイン基調講演2本ではこの論点の新たな詳細展開を確認できなかった旨も明記した
- 2026-07-18: 04-long-term-memory-deep-dive.md に「ナレッジグラフとの関係」節（4.7）を追加。SB OAI Japan／OpenAI公式資料に「ナレッジグラフ」の語は登場しないこと、第三者専門家（Atlan）が「Frontierが機能するには企業側でナレッジグラフ相当のセマンティクス層が必要」と提言している点を調査の上で整理した
- 2026-07-18: 08-understanding-the-enterprise.md 冒頭に「8.0 企業を理解する手段の全体像（4層まとめ）」を追加。①メッセージング（長期記憶・コード解析・会議参加）、②基盤技術（Business Context/Task Memory/Organizational Memory/Learning Loop）、③人的手段（FDE・ファインチューニング）、④非公開領域（ナレッジグラフ・メタデータ形式）の4層に整理し、各章・各節への参照を一覧化した
- 2026-07-19: 補足章（09-anthropic-comparison.md）を追加。Anthropic公式APIドキュメントを主な情報源に、Claude Fable 5（Anthropic版フロンティアモデル）と、Frontierに相当するプラットフォームであるManaged Agentsの構成要素・比較表を整理した。Crystal Intelligence本編とは異なりAnthropic側は情報源の性質（開発者向け技術文書）が異なる点を明記した
- 2026-07-19: 09-anthropic-comparison.md の9.3.3節（認証・セキュリティモデル）を拡充。Vaultの認証情報がサンドボックスに渡らずegress時にAnthropic側プロキシ（MCPプロキシ／Gitプロキシ／環境変数プレースホルダー置換）が注入する仕組みをmermaidのflowchartで図解し、Frontierの「権限ベースの統制」とManaged Agentsの「秘匿ベースの隔離」という設計思想の違いを比較図として追加した
- 2026-07-19: 06-glossary.md に「Egress（イーグレス）／Ingress（イングレス）」の項を追加。Kubernetes固有の用語ではなく、AWS/GCP/Azure/Kubernetes等で共通して使われる一般的なネットワーキング用語である旨を明記し、境界の内向き/外向き通信をmermaid図で図解した。09-anthropic-comparison.md 9.3.3節から本項への参照リンクも追加した
- 2026-07-19: 09-anthropic-comparison.md 9.3.3節のアーキテクチャ図を改善。プロキシがAnthropic自社運用インフラ上で稼働することを緑枠の外側サブグラフで明示し、「顧客インフラでもサンドボックスでもない」点を視覚化した。あわせて「反証：セルフホスト環境で何が起きるか」の節を新設し、`config.type: "self_hosted"`環境で環境変数クレデンシャルの置換が非対応となる公式ドキュメントの記述（"egress is yours, so there's nowhere to substitute the secret"）を、標準環境との比較mermaid図とともに反証として整理した
- 2026-07-19: 教材全体を役割別フォルダ（core/narrative/reference/appendix）に再編成。フラットな00〜09連番からの脱却により、今後の追記でのファイル全体リナンバーを回避できる構成にした。08章の「8.0 企業を理解する手段の全体像」節はREADME側に「教材の全体像」として移設した
```

- [ ] **Step 2: 全リンクが解決するか確認する**

```bash
cd "c:\Dev\tutorials\crystal-intelligence-infos"
grep -oE '\]\([^)]+\.md[^)]*\)' 00-README.md | sed -E 's/^\]\(//; s/\)$//' | while read -r link; do
  path="${link%%#*}"
  [ -f "$path" ] || echo "BROKEN in 00-README.md: $link"
done
```
Expected: 出力なし

- [ ] **Step 3: コミットする**

```bash
git add 00-README.md
git commit -m "$(cat <<'EOF'
Rewrite 00-README.md for the role-based folder structure

Adds the folder map and per-reader reading order, and absorbs the
4-layer summary relocated from narrative/01 in Task 4.

Co-Authored-By: Claude Sonnet 5 <noreply@anthropic.com>
EOF
)"
```

---

### Task 7: リポジトリ全体のリンク健全性を最終検証する

**Files:**
- (変更なし。検証のみ)

**Interfaces:**
- Consumes: Task 1〜6の全成果物
- Produces: 「壊れたリンクが0件」という検証結果

- [ ] **Step 1: リポジトリ内の全Markdownファイルの相対リンクを検証する**

```bash
cd "c:\Dev\tutorials\crystal-intelligence-infos"
broken_found=0
for f in $(find . -name '*.md' -not -path './.git/*' -not -path './docs/*'); do
  dir=$(dirname "$f")
  links=$(grep -oE '\]\([^)]+\.md[^)]*\)' "$f" | sed -E 's/^\]\(//; s/\)$//')
  for link in $links; do
    path="${link%%#*}"
    if [ ! -f "$dir/$path" ]; then
      echo "BROKEN in $f: $link"
      broken_found=1
    fi
  done
done
echo "done"
```
Expected: `BROKEN` 行が1件も出力されず、最後に `done` とだけ表示される

- [ ] **Step 2: 新しいディレクトリ構成を目視確認する**

```bash
find . -maxdepth 2 -name '*.md' -not -path './docs/*' | sort
```
Expected:
```
./00-README.md
./appendix/01-anthropic-comparison.md
./core/01-overview.md
./core/02-architecture.md
./core/03-frontier-platform.md
./core/04-long-term-memory.md
./narrative/01-understanding-the-enterprise.md
./reference/01-key-sources.md
./reference/02-glossary.md
./reference/03-open-questions.md
```

- [ ] **Step 3: git logでリネームが正しく追跡されているか確認する**

```bash
git log --follow --oneline -- core/01-overview.md
```
Expected: 移動前の `01-overview.md` 時代のコミット（`Add Crystal Intelligence learning materials` など）も含めて履歴が表示される（`--follow` がリネームを追跡できている証拠）

このタスクはコード変更を伴わないため、コミットは不要（Task 1〜6ですべてコミット済み）。
