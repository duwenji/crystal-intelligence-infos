# 3. Frontier：Crystal Intelligenceの基盤プラットフォーム

Crystal Intelligence を理解する上で、その土台となる **OpenAI Frontier** 自体は、
Crystal Intelligenceとは別に**OpenAIが独立した製品として一般公開している**エンタープライズAI
プラットフォームです。Crystal Intelligence固有の情報より公開情報が豊富なため、本章で
体系的に整理します。

## 3.1 Frontierとは何か

- **正式名称**: OpenAI Frontier
- **発表日**: 2026年2月5日（OpenAI公式発表）
- **位置付け**: "Enterprise platform for AI agents"（AIエージェントのためのエンタープライズ基盤）
- **一言で言うと**: 企業が安全に本番運用できるAIエージェントを、既存の基幹システム（システム・
  オブ・レコード）と統合しながら「構築・展開・管理」するためのプラットフォーム

**理解のポイント**: FrontierはChatGPTのような「対話インターフェース」でも、
単体の「モデルAPI」でもありません。**複数のAIエージェントを企業システムに統合し、
安全に運用し続けるための基盤（インフラ＋管理レイヤー）**という位置付けです。
出典元のMedium解説では "managed infrastructure for agentic workflows rather than raw models"
（生のモデルというより、エージェント的ワークフローのためのマネージドインフラ）と表現されています。

## 3.2 主要コンポーネント

OpenAI公式・複数の技術解説記事を横断すると、Frontierは大きく3〜4つの機能領域で構成されています。
なお情報源によって「3本柱」（Business Context / Agent Execution / Security & Governance）として
説明する記事と、「Evaluation and Optimization（評価・最適化）」を独立した4つ目の機能として
説明する記事があり、表現には揺れがあります。本教材では実務的な理解のため4つに分けて整理します。

### (1) Business Context（ビジネスコンテキスト）

- CRM、ERP、データウェアハウス、社内アプリなど、企業内の複数システムを連携させる機能領域
- AIエージェントが「人間の従業員と同じ情報」にアクセスできるようにすることが狙い
- 時間の経過とともに情報が蓄積され、**組織としての記憶（institutional memory）**を
  形成していく設計とされている
- **セマンティックレイヤー（semantic layer）**を構築し、すべてのAIエージェントがこれを
  参照する仕組みとされている。エージェントごとに個別のデータパイプラインを持つのではなく、
  「情報がどう流れるか」「意思決定がどこで行われるか」「何が重要な成果指標か」について
  組織横断で統一された理解を共有できる、と説明されている
  → Crystal Intelligence側で語られる「長期記憶」（[02-technical-architecture.md](02-technical-architecture.md) 2.2節）は、
    このBusiness Context機能を土台にしていると考えられる。Task Memory／Organizational Memory／
    Learning Loopといった内部構造や、メタデータ形式に関する非公開領域については
    [04-long-term-memory-deep-dive.md](04-long-term-memory-deep-dive.md) で詳しく扱う

### (2) Agent Execution（エージェント実行）

- モデルの推論能力を実際の業務状況に適用し、タスクを遂行する実行エンジン
- **複数のエージェントが並行して**、複雑なタスクを実際のワークフロー・環境をまたいで
  遂行できるように設計されている
- エージェントはファイル操作やコード実行、各種リソースへのアクセスを、安全な実行環境の中で
  直接行うことができる
- 実行環境として、**ローカル環境・企業のクラウドインフラ・OpenAIがホストするランタイム**の
  3種類にまたがって動作できるとされ、既存の業務フローを根本から作り変えなくても
  導入できることが特徴として挙げられている
  → Crystal Intelligence側の「自律型AIエージェント群の連携」（[02-technical-architecture.md](02-technical-architecture.md) 2.3節）は、
    この実行エンジンの上に構築されるアプリケーション層と位置付けられる

### (3) Evaluation and Optimization（評価・最適化）

- エージェントの成果を継続的に評価し、何がうまくいっていて何がうまくいっていないかを
  可視化する仕組み
- この評価ループを通じて、エージェントは経験を重ねるごとに改善され、
  時間の経過とともに一貫して有用な成果を出し続けられるように設計されている
- Crystal Intelligence側で言及される「Learning Loop」（人間のフィードバックに基づく
  継続的な振る舞いの調整、[06-glossary.md](06-glossary.md)参照）は、この機能領域に対応すると考えられる

### (4) Security & Governance（セキュリティ・ガバナンス）

- 企業導入で不可欠な、包括的な管理・監査機能
- 明示的な権限設定（エージェントが何にアクセスできるか）
- 実行されたアクションの監査可能性（auditable actions）
- エージェントのオンボーディングからアクセス権限管理までを一元的に統制
- より具体的な特徴として、以下が複数の技術解説記事で言及されている
  - **スコープ付きエージェントID（scoped agent identities）**: エージェント1体ごとに
    アクセス範囲を限定した固有の識別子を割り当てる
  - **監査ログ（auditable action logs）**: エージェントが行った操作を後から追跡できる記録
  - **ロールベースアクセス制御（role-based access controls）**
  - **SOC 2準拠**: 企業のセキュリティ監査基準に対応しているとされる

## 3.3 互換性：OpenAI製に限らない「マルチエージェント基盤」という設計

Frontierの特徴として、**OpenAI自身が作ったエージェントに限定されない**という点が挙げられます。

- OpenAIが構築したエージェント
- 企業が独自に開発したエージェント
- Google、Microsoft、Anthropicなど**第三者ベンダーのエージェント**

これらを横断的に受け入れ、統合管理できる基盤として設計されている、と説明されています。
これは、Frontierが「特定モデルの利用窓口」ではなく、**エージェント運用のための中立的な
基盤レイヤー**を志向していることを示しています。

> **※注意**: Crystal Intelligence自体がこのマルチベンダー互換性をどこまで日本市場向けに
> 提供するかは、SB OAI Japan側の一次情報からは明示されていません。ここではあくまで
> Frontier（OpenAI側のプラットフォーム）の一般的な設計方針として理解してください。

## 3.4 使い方：導入・運用の流れ

Frontierは単体のAPIやアプリではなく基盤プラットフォームであるため、「使い方」は
一般的なSaaSのセットアップというより、**企業導入プロジェクトとしての進め方**に近い形で
説明されています。複数の技術解説記事を横断すると、おおむね以下の流れになります。

1. **Business Contextの接続**: CRM・データウェアハウス・チケット管理ツール・社内アプリなど、
   既存の「システム・オブ・レコード」をFrontierに接続し、セマンティックレイヤーを構築する
   （3.2節(1)参照）
2. **エージェントのオンボーディング**: 人間の新入社員と同じように、エージェントに対しても
   会社のポリシー・データスキーマ・業務ルール・ドメイン知識を学習させる、構造化された
   オンボーディングの仕組みが用意されている。オンボーディング完了後に本番投入される
3. **権限・アイデンティティの設定**: エージェントごとにスコープ付きID・ロールベースの
   アクセス権限を設定し、何に対して何ができるかを明示的に定義する（3.2節(4)参照）
4. **実行環境へのデプロイ**: ローカル環境／企業のクラウドインフラ／OpenAIホスト型ランタイムの
   いずれか（または組み合わせ）でエージェントを稼働させる。既存の業務フローを
   作り変えなくても導入できる設計が特徴とされる
5. **評価・フィードバックループの運用**: 人間の承認・修正・却下といったフィードバックを
   もとに、Evaluation and Optimization機能でエージェントの成果を継続的に改善していく
   （3.2節(3)参照）

**導入支援の枠組み**: OpenAI側には **Enterprise Frontier Program** という導入支援プログラムが
存在し、"The OpenAI Deployment Company" 所属のFDE（Forward Deployed Engineer）がチームと
ペアを組み、アーキテクチャ設計・ガバナンスの運用実装・本番運用までを伴走支援するとされている。
→ Crystal Intelligence側で語られるFDEによる伴走型支援（3.6節の表、[05-key-sources.md](05-key-sources.md)参照）は、
  この「Enterprise Frontier Program」の枠組みが日本市場向けに展開されたもの、または
  それに準じる支援体制と考えられる（※公式に明言された対応関係ではなく、教材側の推測）

## 3.5 対象顧客・導入状況

- 公開時点（2026年2月）では、**Fortune 500規模の大企業**を中心とした限定的な早期顧客への
  提供から開始されている
- 報道されている早期顧客の例: Intuit、State Farm、Thermo Fisher、Uber、HP、Oracle（いずれも米国企業）
- さらにBBVA、Cisco、T-Mobileなど数十社がパイロットプログラムに参加していると報じられている
- 具体的な成果事例として、以下のような数字が報道・技術解説記事で紹介されている
  - ある大手製造業では、生産最適化業務の所要期間が**6週間から1日**に短縮
  - ある大手投資会社では、営業担当者の稼働時間の**90%以上**を他業務に振り向けられるようになった
  - ある大手エネルギー企業では、生産量が**最大5%**増加
  - （※これらは二次情報源の要約であり、算出方法や前提条件などの詳細は公開情報からは確認できない）
- 日本市場においては、SB OAI Japan・ソフトバンクが**社内での先行検証**を進めた上で、
  Crystal Intelligenceとしての展開を加速する計画とされている
  （出典: SoftBank公式プレスリリース、2026年2月6日付）

## 3.6 Frontier と Crystal Intelligence の関係の整理

| 観点 | Frontier | Crystal Intelligence |
|---|---|---|
| 提供元 | OpenAI（グローバル） | SB OAI Japan（ソフトバンク×OpenAIの合弁） |
| 位置付け | 汎用エンタープライズAIエージェント基盤 | Frontierを土台にした日本企業向けパッケージソリューション |
| 対象市場 | グローバル（米国Fortune 500企業から展開） | 日本国内企業 |
| 提供内容 | Business Context / Agent Execution / Evaluation and Optimization / Security & Governance | 上記に加え、業務プロセスに合わせたエージェントライブラリ、実装・運用支援 |
| 導入支援 | Enterprise Frontier Programを通じた、一般的なエンタープライズ導入支援（3.4節参照） | FDE（Forward Deployed Engineer）による伴走型支援（[05-key-sources.md](05-key-sources.md)参照） |

**まとめると**: Frontierは「土台となる基盤製品」、Crystal Intelligenceは
「その基盤の上に、日本企業向けの実装・運用支援・業務特化エージェントを組み合わせた
パッケージソリューション」という関係です。[01-overview.md](01-overview.md) の図解も、
この関係を踏まえて読み返すと理解が深まります。

## 3.7 Frontierに関する情報源

| 情報源 | URL | 性質 |
|---|---|---|
| OpenAI公式トップページ | https://openai.com/business/frontier/ | 一次・公式（製品ページ） |
| OpenAI公式発表記事 | https://openai.com/index/introducing-openai-frontier/ | 一次・公式（発表記事） |
| SoftBank公式プレスリリース（英語） | https://www.softbank.jp/en/corp/news/press/sbkk/2026/20260206_01/ | 一次・公式（Crystal Intelligenceとの関係） |
| CNBC報道 | https://www.cnbc.com/2026/02/05/open-ai-frontier-enterprise-customers.html | 二次・報道（早期顧客・市場文脈） |
| Fortune報道 | https://fortune.com/2026/02/05/openai-frontier-ai-agent-platform-enterprises-challenges-saas-salesforce-workday/ | 二次・報道（業界インパクトの解説） |
| TechCrunch報道 | https://techcrunch.com/2026/02/05/openai-launches-a-way-for-enterprises-to-build-and-manage-ai-agents/ | 二次・報道（導入・運用フローの解説） |
| DataCamp技術解説 | https://www.datacamp.com/blog/openai-frontier | 二次・技術解説（仕組み・機能の整理） |

> **読み方の注意**: OpenAI公式ページ（openai.com/business/frontier/ 等）は、
> 本節を執筆した2026年7月時点でも引き続きアクセス制限（HTTP 403）により
> ツールから直接本文取得ができませんでした。上記の内容は、Web検索を通じて集約した
> 複数の二次情報源（報道・技術解説記事）の要約、およびSoftBank公式サイトからの
> 間接引用を統合したものです。一次情報を直接確認したい場合は、必ずURLに自身でアクセスし、
> 最新の記載内容を確認してください。

---

次は、Frontierの記憶機構（Business Context）についてさらに深掘りします。
→ [04-long-term-memory-deep-dive.md](04-long-term-memory-deep-dive.md)
