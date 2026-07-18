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

## 教材の構成

| ファイル | 内容 |
|---|---|
| [01-overview.md](01-overview.md) | Crystal Intelligenceとは何か／事業主体（SB OAI Japan）の位置付け |
| [02-technical-architecture.md](02-technical-architecture.md) | 技術アーキテクチャ（基盤モデル、RAGと長期記憶、マルチエージェント、FDE） |
| [03-frontier-platform.md](03-frontier-platform.md) | 基盤プラットフォーム「Frontier」の体系的な解説（3本柱、対象顧客、Crystal Intelligenceとの関係） |
| [04-long-term-memory-deep-dive.md](04-long-term-memory-deep-dive.md) | 長期記憶（Business Context）の深掘り：Task/Organizational Memory、Learning Loop、メタデータ形式の非公開領域、批判的視点 |
| [05-key-sources.md](05-key-sources.md) | Crystal Intelligence関連の一次情報源・参考リンク一覧とそれぞれの読み方 |
| [06-glossary.md](06-glossary.md) | 用語集（Frontier、Business Context、Task/Organizational Memory、o1シリーズ、FDE、RAG等） |
| [07-open-questions.md](07-open-questions.md) | 未公開・不明な技術領域と、深掘りする際の調査観点 |
| [08-understanding-the-enterprise.md](08-understanding-the-enterprise.md) | 「企業を理解する」手段の4層まとめ（メッセージング／基盤技術／人的支援／非公開領域）、特許取得の事実、発信イベントのタイムライン |
| [09-anthropic-comparison.md](09-anthropic-comparison.md) | 補足：Anthropic陣営の等価プロダクト比較（Claude Fable 5、Frontierに相当するManaged Agentsプラットフォーム） |

## 読み方の推奨順序

1. まず [01-overview.md](01-overview.md) で全体像（誰が何を提供しているか）を掴む
2. [02-technical-architecture.md](02-technical-architecture.md) で技術構成要素を分解して理解する
3. [03-frontier-platform.md](03-frontier-platform.md) で基盤部分（Frontier）を深掘りする
4. [04-long-term-memory-deep-dive.md](04-long-term-memory-deep-dive.md) で「長期記憶」の中身をさらに深掘りする
5. [08-understanding-the-enterprise.md](08-understanding-the-enterprise.md) で「企業を理解する」というメッセージングの発信経緯を確認する
6. [06-glossary.md](06-glossary.md) を辞書的に参照しながら1〜5を読み直す
7. さらに深く知りたい場合は [07-open-questions.md](07-open-questions.md) を起点に追加調査する

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
