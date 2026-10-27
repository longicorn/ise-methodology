# ISE Logic Wireframe Notation (LWN) 構想提案

本ドキュメントは、ISE（Intent Synchronized Engineering）において人とコード（実装）の間に位置する **Logic Wireframe Notation (LWN / ロジック・ワイヤーフレーム記法)** の構想提案書です。

「仕様記述言語」というジャンルの下に Z や TLA+、Alloy などの具体的な言語が存在するように、LWNは **「意図と実装の間で人間が理解しやすくするため、いい加減さ（ラフさ）を許容したロジック抽象化体系（メタモデル）」** の総称です。特定のプログラミング言語の構文に過度に固定化せず、また単なる自然言語ドキュメント（Markdownメモ）に退行することもなく、 **実装詳細を剥ぎ取った普遍的な処理の抽象化（プリミティブとワークフローパターン）** を定義します。

現時点では **提案レベル（Proposal）** であり、ロジック抽象化のモデルと代表的な意味論の例を中心に定義しています。具体的な各種シンタックスへの投影（プロジェクション）や厳密な形式意味論は今後の拡張課題とします。

---

## 1. 背景と目的

### 1.1 従来の課題：詳細仕様書のジレンマ
- **二重保守コストと乖離（ドキュメント腐敗）**  
  自然言語による詳細仕様書とソースコードを二重管理することは維持コストが極めて高く、開発が進むにつれて仕様書と実装の乖離が不可避でした。
- **共有の欠落**  
  一方で、詳細仕様書を排してコードのみに依存すると、多様なステークホルダー（非エンジニア、PO、他チームのエンジニア）間での共通認識が失われ、設計意図がブラックボックス化します。

### 1.2 未来の開発環境への適応
- **AIコーディング時代の到来**  
  今後AIによる実装自動化が進むと、人間が手作業で詳細なコード（Rust、Go、TypeScriptなど）を日常的に読み書きしない層が主流になると予想されます。
- **人間が検証すべき最後の砦**  
  人間が責任を持つべき対象は、実装の細かい文法ではなく **「何を作りたいのか（要求）」** と **「システムの振る舞い・契約が正しいか（仕様）」** になります。これらを非エンジニアでも直感的に読め、かつAIが厳密にコードへ変換・検証できる表現手段が求められます。

### 1.3 本質的複雑さと偶発的複雑さの分離
ソースコードからLWNへの **逆変換（Code → LWN）** を行うことで：
- **本質的複雑さ（設計レベルの複雑さ）**：業務フローや状態遷移、依存関係そのものの複雑化
- **偶発的複雑さ（実装レベルの複雑さ）**：言語特有のボイラープレート、ライブラリの癖、不適切なコーディングによる複雑化

を客観的に分離して評価できます。過去のコミットと現在のコミットをLWNレベルで比較することで、「設計そのものが破綻・複雑化しているのか、単に実装の書き方の問題なのか」を判断する指標が得られます。

---

## 2. コア設計思想

1. **実行は不要、大まかな処理が書ける軽量記法（ワイヤーフレーム性）**  
   コンパイラやインタプリタで直接実行することを目的とせず、人間とAIが意図と実装の骨組みを共有・合意するためのプロトコルとして機能します。理解しやすさを最優先とし、細部の省略や「いい加減さ（ラフさ）」を許容します。
2. **「実現手段（How）」は抽象化し、「保証する性質（Contract / Invariant）」を明示する**  
   OSスレッドかGoroutineか、UnixドメインソケットかTCPかパイプかといった実現手段はコード側の裁量とします。ただし、「順序保証が必要」「冪等であること」といった障害時や並行性に直結する性質は明示します。
3. **ビジネスに影響する厳密性（ドメイン制約・型）の漸進的明示**  
   Fintechにおける数値精度（丸め規則、桁数）や、ETLにおける文字エンコーディングなど、ビジネス破綻に直結する要素は「実装詳細」として切り捨てず、意味論的型（Semantic Type）や契約として厳密に指定できるようにします。
4. **視点と抽象化の光学モデル（ZoomとLens）とプロファイル拡張**  
   固定的な階層構造（Layer）ではなく、カメラの「Zoom（詳細度）」と「Lens（抽象化の切り口）」の二軸で表現をコントロールします。大局的な「Overview」から契約を明示する「Deep」までの距離調整（Zoom）と、魚眼・望遠・マクロのように目的に応じて強調・捨象する関心事を切り替える仕組み（Lens）により、必要な箇所を必要な切り口で記述します。また、ドメイン特性（Fintech、ETL、Web等）に応じたボキャブラリの拡張を許容します。
5. **自然言語の埋め込み（Escape Hatch）と外部アダプタ化**  
   画面のレイアウトや定型的なCRUD表示など、疑似コード化する価値の低い末端仕様は自然言語ブロックとしてそのまま埋め込みます。GUIや外部連携プロトコルはコア記法の「外側の概念」として扱います。

---

## 3. 機能としての構成要素（Building Blocks / 実体）

LWNでは、ワイヤーフレーム上に配置する「名詞・実体」となる構成要素を、実装技術に依存しない普遍的な基本要素（Building Blocks）として定義します。実体そのものは片手で数えられるほどシンプルに保ちます。

### 3.1 コア・プリミティブ
1. **Task（処理・実行単位）**:
   - 独立した1つの責務・処理単位。実装が OSスレッド、プロセス（fork）、Goroutine、Serverless（AWS Lambda等）、Web Worker、純粋関数、非同期ジョブのいずれであっても抽象化されます。
2. **Stream（データ伝送路と結合）**:
   - タスク間を結ぶデータのパイプライン。実装が Unixパイプ、TCP/UNIXソケット、Goのchannel、Redis Pub/Sub、Kafka、SQSメッセージキューのいずれであっても抽象化されます。
3. **Resource（境界と永続化）**:
   - 外部との接点や永続化の境界。RDBMS、NoSQL、S3/オブジェクトストレージ、外部Web API、一時ファイル（TmpDir）などを抽象化します。
4. **State / Place（状態・保持場所）**:
   - エンティティが留まるライフサイクルの局面（Draft, Published, Reserved等）や、ペトリネットにおけるトークンの蓄積場所（バッファ・キュー）。
5. **Event / Message（シグナル・電文）**:
   - Streamを流れるデータの実体、または状態遷移の契機となる通知シグナル。
6. **Guarantee（契約・不変条件）**:
   - 「順序保証（ordered）」「冪等性（idempotent）」「原子性（atomic）」など、実現手段は問わないがシステムが満たすべき性質・契約。

### 3.2 ロジック抽象化パターン・総合カタログ
[Workflow Patterns](http://www.workflowpatterns.com/) や分散システム、OSモデルから抽出された、LWNで表現可能な普遍的処理パターンの一覧です。

#### 1. 制御フロー・並行性（Control-Flow & Concurrency）
| パターン | 抽象化の意図 | 実装技術の例（How） |
| :--- | :--- | :--- |
| **Sequence（逐次実行）** | 前の処理が終わってから次へ進む | 関数の直列呼び出し、await |
| **Parallel Split / Fork（並行分岐）** | 1つの入力を契機に、複数の処理を同時に走らせる | Goroutine, pthread, Promise.all |
| **Synchronization / Join（同期合流）** | 並行して走るすべての処理の完了を待つ | `sync.WaitGroup`, `JoinHandle` |
| **Exclusive Choice / XOR-Split（排他分岐）** | 条件に合致する1つのパスだけを選択する | if/else, switch/match |
| **Multi-Choice / OR-Split（多重条件分岐）** | 満たした条件すべてのパスを並行して実行する | 複数イベントの発行 |
| **Discriminator / First-Win（先着合流）** | 複数並行処理のうち、最初に完了した1つを採用し他を破棄 | `Promise.race`, `select` 構文 |
| **Multiple Instances（多重並行インスタンス）** | 配列やストリームの要素数分、動的にタスクを並行起動 | Parallel map, SQS+Worker群 |
| **Mutual Exclusion（排他制御）** | 複数タスクが同時に同一領域に触れないようロック | Mutex, 分散ロック（Redis/Redlock） |
| **Backpressure / Flow Control（背圧・流量制御）** | 下流の処理能力に合わせて上流の供給を制御する | リアクティブストリーム、バッファ上限 |

#### 2. データフロー・変換（Data Interaction & Transformation）
| パターン | 抽象化の意図 | 実装技術の例（How） |
| :--- | :--- | :--- |
| **Pipe & Filter（直列変換）** | 入力データを加工して次のタスクへ流す | Unixパイプ、イテレータチェーン |
| **Fan-Out / Publish-Subscribe（同報配信）** | 1つのデータを複製して複数の購読先へ届ける | SNS, Redis Pub/Sub, Kafka topic |
| **Fan-In / Aggregator（集約合流）** | 複数ソースのデータを特定のキーや条件で1つにまとめる | MapReduceのReduce, Batching |
| **Filter / Quarantine（フィルタと隔離）** | 不正・不要データを弾き、不正データは隔離所へ送る | バリデーション、Dead Letter Queue (DLQ) |
| **Enricher（データ補加）** | 主データに別リソースから引いた情報を付与する | DB lookup結合、外部API問い合わせ |
| **Content-Based Routing（内容ベースルーティング）**| データの中身（種別・金額等）を見て宛先を変える | ルーティングエンジン、メッセージ振り分け |

#### 3. リソース・権限・セキュリティ（Identity, Access & Context）
| パターン | 抽象化の意図 | 実装技術の例（How） |
| :--- | :--- | :--- |
| **Principal / Actor（実行主体の定義）** | その処理を実行している主体（ユーザー、ボット、システム） | Unix User/UID, Service Account, JWT sub |
| **Role-Based Execution / RBAC（役割付与）** | 特定のロール（管理者、一般客、監査者）でのみ実行許可 | IAM Role, アプリ内ロールガード |
| **Capability / Scope Check（権限・スコープ検査）** | 特定の操作権限（`read:users`, `write:payment`）の所持確認 | OAuth2 scope, Linux Capability |
| **Context Propagation（認証・相関の伝播）** | 分散タスク間でユーザー情報やトレースIDを引き回す | HTTP Header (Bearer/TraceContext), context.Context |
| **Impersonation / Delegation（代理・権限借用）** | 一時的に別の主体（特権等）として振る舞う | `sudo`, `su`, AWS AssumeRole |
| **Four-Eyes Principle / Separation of Duties（相互承認）**| 「作成者」と「承認者」が同一であってはならない制約 | ワークフロー承認ルール、マルチシグ |

#### 4. ファイル・I/O・ストレージ（Filesystem, Storage & State）
| パターン | 抽象化の意図 | 実装技術の例（How） |
| :--- | :--- | :--- |
| **Ephemeral Workspace（作業用一時領域）** | タスク終了時に自動破棄されるクリーンな作業ディレクトリ | `/tmp`, `mktemp`, Docker一時ボリューム |
| **Atomic Swap / Replace（原子的置換）** | 作成途中の不完全な状態を見せず、一瞬で差し替える | `rename` システムコール, Symlink切り替え |
| **Virtual Path Mapping（論理パス解決）** | 物理パスではなく論理識別子（`:inbound`, `:artifacts`）で指定 | マウントポイント、S3バケットマッピング |
| **Streaming Spooling（大容量スプール）** | メモリに乗り切らないデータを安全にディスクへ退避 | ディスクバッファ、Multipart upload |
| **Snapshot / Freeze（スナップショット）** | ある時点のデータ状態を不変（Immutable）に固定する | ファイルコピー, DBスナップショット, Gitコミット |

#### 5. 時間・スケジューリング（Temporal & Rate Limiting）
| パターン | 抽象化の意図 | 実装技術の例（How） |
| :--- | :--- | :--- |
| **Cron / Recurring Schedule（定周期起動）** | 指定日時やインターバルで自律起動する | cron, CloudWatch Events, systemd timer |
| **Deadline / Timeout（時間制限）** | 一定時間内に応答がなければ中断する | HTTP timeout, `context.WithTimeout` |
| **Delay / Backoff（遅延実行・待機）** | 一定時間待ってから次へ進む / 指数関数的待機 | sleep, Exponential Backoff |
| **Rate Limiter / Throttling（流量制限）** | 単位時間あたりの実行回数を制限（API保護等） | Token Bucket, Leaky Bucket |
| **Debounce / Throttle（イベント集約）** | 短時間の連続イベントを最後の1回（または最初の1回）に間引く | UIイベント制御、バッチトリガー |

#### 6. 例外・障害回復・補償（Exception Handling & Compensation）
| パターン | 抽象化の意図 | 実装技術の例（How） |
| :--- | :--- | :--- |
| **Retry with Policy（方針付き再試行）** | 一時的障害とみなし、規定回数・間隔でやり直す | リトライループ、ポリシー定義 |
| **Fallback / Degrade（縮退・代替動作）** | 主処理の失敗時に、最低限の代替値・代替画面を返す | キャッシュ返却、デフォルト値 |
| **Circuit Breaker（遮断・保護）** | 連続失敗時に以降の呼び出しを即座に拒絶し共倒れを防ぐ | Netflix Hystrix, envoy circuit breaker |
| **Compensation / Saga（補償巻き戻し）** | 途中で失敗した際、過去に成功したタスクの取り消し処理を実行 | 「送金失敗時に課金取り消しAPIを叩く」 |
| **Quarantine / DLQ（隔離退避）** | 処理不能な毒メッセージ（Poison Pill）を横に除けて処理継続 | Dead Letter Queue, エラーログ退避 |
| **Fail-Stop / Crash（フェイルファスト）** | 矛盾が起きた場合、被害を広げず即座に安全に停止する | Panic, exit(1), プロセス再起動 |

#### 7. 関数・実行単位・スコープ（Modularity, Purity & Lifecycle）
| パターン | 抽象化の意図 | 実装技術の例（How） |
| :--- | :--- | :--- |
| **Pure Transform（純粋関数的変換）** | 副作用がなく、同じ入力なら必ず同じ出力を返す | 純粋関数（キャッシュ・並列化可能） |
| **Effectful Step（副作用を伴う手続き）** | DB書き込みやAPI送信など外界へ変化を与える処理 | 手続き呼び出し、コマンド |
| **Resource Acquisition / RAII（スコープ保護）** | 開始時にリソースを確保し、成否にかかわらず確実に解放 | `defer`, `try-with-resources`, `using` |
| **Idempotent Step（冪等ステップ）** | 何度重複して実行されても結果が変わらない保証 | `UPSERT`, 冪等性キー（Idempotency-Key） |

---

## 4. 切り口としてのレンズ（Lenses / 目的別の抽象化と捨象ルール）

LWNでは、配置した構成要素（Task, Stream, Resource等）を固定的な1枚の図やコードで網羅しようとせず、 **「Zoom（距離・詳細度）」** と **「Lens（関心事別の光学フィルター）」** を通して目的別に投影（プロジェクション）します。

```
【機能（実体・名詞）】────────┐
  - Task, Stream, Resource    │  掛け合わせ
  - State, Event, Guarantee   ├──────────────▶  目的別のワイヤーフレーム
【切り口（レンズ・動詞）】────┘                （必要な関心事だけが強調され、
  - Petri Net / EIP / State /                    他は意図的にボケる）
    Sequence / Timing / Security...
```

### 4.1 ズーム（Semantic Zooming）：視点の距離と詳細度
全体を一律の深さで書き切る必要はなく、関心のある重要箇所だけを部分的にズームイン（Semantic Zooming）します。

| ズーム視点 | 想定読者・対話シーン | 見えるもの（描くべき関心事） | あえて捨てるもの（いい加減さの許容） |
| :--- | :--- | :--- | :--- |
| **Overview**<br>（広角・引きの視点） | PO、非エンジニア、設計初期のブレスト | 処理の大局的な流れ、主たるTaskとStream、大枠の入出力 | 例外処理、リトライ、詳細プロトコル、厳密な型や精度 |
| **Focus**<br>（中距離・境界の視点） | PO、リードエンジニア、仕様合意 | 主要な分岐、結果種別（成功/失敗、画面遷移）、大まかなフォールバック、外部境界 | 障害分離の詳細、細かなトランザクション境界、バイナリ仕様 |
| **Deep**<br>（接写・契約の視点） | エンジニア間、AI実装生成・検証 | 契約と不変条件（`ordered`, `idempotent`, `atomic` 等）、意味論的型（精度、文字コード）、リソース・トランザクション境界 | 具体的なライブラリやOSスレッド等の低レベルな実現手段 |

---

### 4.2 専門レンズ（Specialized Lenses）：理論・業界標準に基づく射影

計算機科学や業界で培われた確立されたモデル（Petri Net, EIP, UML等）を「レンズ」として採用し、目的ごとに強調・捨象するルールを定義します。

```
┌────────────────────────────────────────────────────────────────────────┐
│                   LWN コアモデル（Task, Stream, Resource, Guarantee）   │
└────────────────────────────────────┬───────────────────────────────────┘
                                     │ 目的別に投影（Lens）
    ┌──────────────┬─────────────────┼─────────────────┬────────────────┐
    ▼              ▼                 ▼                 ▼                ▼
【State &      【Interaction &   【Pipeline &      【Lifecycle &    【Timing &
 Concurrency】  Sequence】       Messaging】       State Machine】  Temporal】
(Petri Net)    (UML Sequence)    (EIP / Stream)    (UML State)      (UML Timing)
 競合・同期     呼び出し・境界     伝送・ルーティング 状態遷移・不変条件  期限・時間制約
```

#### Lens 1: 並行・競合・同期レンズ（Petri Net / CPN 的視点）
> **視点の目的**: 複数のタスクが**「何を待ち合わせ」「どこでリソースを取り合い」「いつ発火するか」**に集中する。内部の細かい業務ロジックは完全にボカす。

| パターン | Petri Net / CPN の概念 | 抽象化の意図 |
| :--- | :--- | :--- |
| **Token Consumption & Firing（発火条件）** | Transition / Input Place | 必要なデータや前提条件（トークン）がすべて揃った瞬間に自律的に処理が起動する。 |
| **Synchronization / Rendezvous（待ち合わせ）** | Multi-input Transition | 異なる並行パス（例: 決済完了と在庫引当完了）の完了を待ち合わせて合流する。 |
| **Conflict / Race for Token（競合・排他）** | Conflict（複数Transitionが1つのTokenを奪い合う） | 1つの在庫枠や同一アカウントの残高を、複数タスクが同時に奪い合う際の排他制御（Mutex/FIFO）。 |
| **Colored Token Routing（属性による発火）** | Colored Token（属性値を持つトークン） | 単なるシグナルではなく、「VIP会員」「高額決済」などのデータ属性によって発火先が変化する。 |
| **Bounded Capacity / Backpressure（容量制限・背圧）**| Capacity of Place | バッファの上限に達したら、上流の発火を物理的に停止させる。 |

#### Lens 2: 相互作用・シーケンスレンズ（UML Sequence / CSP 的視点）
> **視点の目的**: システム境界を越えた**「誰と誰が、どの順番で、同期/非同期どちらで対話しているか」**に集中する。

| パターン | UML / CSP の概念 | 抽象化の意図 |
| :--- | :--- | :--- |
| **Synchronous Call-Return（同期要求・応答）** | Synchronous Message | 相手の応答が返るまで自身の処理をブロック（待機）する。 |
| **Fire-and-Forget / Asynchronous（非同期送信）** | Asynchronous Message | 相手へメッセージを放り投げ、応答を待たずに即座に次へ進む。 |
| **Context & Identity Propagation（文脈伝播）** | Execution Specification | 呼び出し階層を跨いで、Trace ID、認証トークン（JWT）、相関ID（Correlation ID）を引き回す。 |
| **Delegation / Proxy（委譲・代理）** | Combined Fragment / Alt | 自身では処理せず、後続の別タスクへ権限や処理をそのまま横流しする。 |

#### Lens 3: パイプライン・メッセージ伝送レンズ（EIP: Enterprise Integration Patterns 的視点）
> **視点の目的**: データがパイプラインを流れる過程で**「どう分割・結合・変換・ルーティングされるか」**に集中する。

| パターン | EIP の概念 | 抽象化の意図 |
| :--- | :--- | :--- |
| **Content-Based Router（内容ルーティング）** | Content-Based Router | 電文の中身（ヘッダや値）を見て、次へ流すStreamを動的に切り替える。 |
| **Splitter（分割）** | Splitter | 1つの巨大データ（CSV、バルク配列等）を個別のレコード要素に分解して下流へ流す。 |
| **Aggregator（集約）** | Aggregator | バラバラに流れてくる個別データを、キーや時間枠、個数（1000件毎）で束ねて1つの塊にする。 |
| **Message Translator / Normalizer（正規化）** | Message Translator | 外部システムの固有フォーマットを内部の共通データモデルに変換する。 |
| **Content Enricher（データ補加）** | Content Enricher | データが流れる途中で、不足している情報をDBや別APIから引いて継ぎ足す。 |
| **Idempotent Receiver（重複排除・冪等受信）** | Idempotent Receiver | 同一メッセージが再送されてきても、重複実行を無視して安全に処理をスキップする。 |

#### Lens 4: ライフサイクル・状態マシンレンズ（UML State Machine 的視点）
> **視点の目的**: エンティティ（注文、口座、タスク等）が**「今どの状態にあり、どのイベントでどの状態へ遷移できるか、何が禁止されているか」**に集中する。

| パターン | State Machine の概念 | 抽象化の意図 |
| :--- | :--- | :--- |
| **State & Transition（状態と遷移）** | State / Transition | `Draft` → `Published`、`Pending` → `Settled` などの正当な状態推移。 |
| **Guard Condition（遷移ガード）** | Guard `[condition]` | 「残高が送金額以上であること」「承認フラグが立っていること」等、遷移を許可する前提契約。 |
| **Entry / Exit Actions（境界副作用）** | Entry/Exit Action | その状態に入った瞬間（通知メール送信等）、抜けた瞬間（タイマー破棄等）に発動する処理。 |
| **Invariant Guarantee（状態不変条件）** | State Invariant | 「いかなる状態推移の途中であっても、預金合計と貸方合計は常に一致しなければならない」という絶対制約。 |

#### Lens 5: 時間・タイミングレンズ（UML Timing Diagram 的視点）
> **視点の目的**: 処理の論理的な正しさではなく、**「時間的制約、遅延、タイムアウト、実行間隔」**に集中する。

| パターン | UML Timing の概念 | 抽象化の意図 |
| :--- | :--- | :--- |
| **Duration Constraint / Timeout（時間上限）** | Duration Constraint | 呼び出しから3秒以内に完了しなければ中断・フォールバックへ倒す。 |
| **Delay / Cooldown（待機・冷却期間）** | Time Metric | 次の処理まで意図的に待つ、または再試行までの指数バックオフ。 |
| **Rate Limit / Windowing（流量枠）** | State Lifetime | 「1秒あたり最大100リクエスト」といった時間窓による制限。 |
| **Heartbeat / Deadline Missed（死活と失効）** | Periodic Event | 定期シグナルが途絶えたら異常とみなし、リカバリーを発動する。 |

#### 横断レンズ: セキュリティ・権限レンズ（Resource & Security 的視点）
> **視点の目的**: システム内で**「誰がどの権限で実行し、職務がどう分掌されているか」**に集中する。

| パターン | セキュリティモデルの概念 | 抽象化の意図 |
| :--- | :--- | :--- |
| **Actor & Subject（主体）** | Principal / Subject | 実行者が誰か（User, Worker, System, Service Account）。 |
| **Capability / Scope Guard（権限壁）** | OAuth Scope, Capability | 実行に必要な権限の宣言（`read:audit`, `write:transfer` 等）。 |
| **Separation of Duties（職務分掌・相互承認）** | Four-Eyes Principle | 同一エンティティに対して「申請者」と「承認者」が同じ主体であってはならない制約。 |

---

### 4.3 光学メタファーとの対応
日常的なカメラのアナロジーでも直感的に捉えられます：
- **魚眼レンズ（Fisheye）**: 全体俯瞰・外部境界（歪みを許容してすべてを1枚に収める）
- **望遠レンズ（Telephoto）**: 主要シナリオ・ハッピーパス（背景の例外やインフラをボカして主役だけを追う）
- **マクロレンズ（Macro）**: 契約・完全性（数値精度、排他制御、冪等性など微小な制約に肉薄する）
- **偏光（PL）フィルター**: 信頼性・障害設計（正常系を半透明にし、障害回復・補償トランザクションを透視する）

---

### 4.4 Zoom × Lens による合意形成の円滑化
1. **「何を無視して良いか」の共通了解ができる**  
   「いまはパイプラインレンズで見ているから、排他制御は書かれていなくて当然」「魚眼レンズだから、型定義がラフで構わない」という合意が成立し、記述コストを最小化できます。
2. **単一の意図モデルから多様なプロジェクションが可能になる**  
   AIやビューアツールと組み合わせることで、「非エンジニア向けにOverview＋望遠でハッピーパスを提示する」「SRE・インフラ向けにDeep＋偏光フィルターで障害耐性を抽出する」といった多角的な出力が可能になります。

---

## 5. 具象構文と投影（Concrete Syntax & Projections）

LWNは「機能（Building Blocks）」と「切り口（Lenses）」からなるメタモデルであるため、特定の表記法に唯一束縛されるものではありません。目的や読者の文脈に応じて、複数の **具象構文（Concrete Syntax）やビジュアル表現への投影（Projection）** が可能です。

### 5.1 具象構文のバリエーション
1. **Rubyライク構文（Reference Syntax）**:
   - 自然な英文の語順（SVO）を崩さずにブロック構造を扱えるため、可読性と構造化のバランスに優れる。エンジニアやアーキテクト間の詳細設計・合意向け。
2. **Markdownアウトライン構文（Lightweight Outline）**:
   - ハイフン（`-`）やインデントを用いた箇条書きテキスト。非エンジニアがチャットやメモ帳で自然言語のまま思考速度で骨組みを書き下すのに最適。
3. **シナリオ／BDD構文（Scenario / Gherkin-style）**:
   - `Given`（前提・Guard）、`When`（Task実行・Event）、`Then`（状態遷移・Guarantee）を用いた振る舞い指向の記法。POやQAとの受入基準・ユースケース合意に加え、**「意図（Intent）とテストケースの間」を取り持つテスト用ワイヤーフレーム**としても機能します。
4. **ダイアグラム投影（Visual Projection）**:
   - MermaidやUML、Petri Net図ツールへの自動レンダリング。アーキテクチャ全体像の可視化やプレゼンテーション用。

### 5.2 コアの外側に置く追加概念（Presentation / Domain Profiles）
- **GUI・画面表現（View）**:
  画面のマークアップ、レイアウト、スタイルなどはLWNのコア記法に含めず、 **自然言語の指示ブロック** や **HTML/CSS、デザインツール（Figma等）への参照** として扱います。LWN側の関心は「Viewに何を渡し、Viewからどんなイベントが返るか」という入出力境界のみです。
- **ドメイン別プロファイル（Profiles）**:
  - **Fintech Profile**: 通貨、丸め、勘定元帳（Ledger）、監査ログ（AuditTrail）
  - **ETL/MasterData Profile**: 固定長電文（fixed_width）、文字コード変換、差分更新（Upsert）、隔離（Quarantine）

### 5.3 あえて「捨てる」要素（偶発的複雑さの排除）
- メモリ管理・参照・ポインタ・GCの意識
- ループ制御構文（`for`, `while`, `break`）：コレクション操作やStreamの消費、Workflow Patterns（Multiple Instances）に一元化
- 低レベルな例外クラス構造（`try/catch`）：結果種別とフォールバック・補償（Compensation）の宣言に集約
- 言語都合のクラス継承やメタプログラミング

---

## 6. 記述例（参考：Rubyスタイル具象構文による表現例）

> [!NOTE]
> 以下に示すRubyスタイルのDSL構文は、LWNのモデルと意味論を具体的に書き下すための **参照構文の一例（Reference Syntax）** です。
> 「すべての関心事を1つの図やコードに詰め込まない」という原則に基づき、**「どのLens（切り口）を通して記述しているか」** を明示してワイヤーフレーム化しています。

---

### 6.1 例1：ETLデータパイプライン（Extract → Transform → Load）
大容量CSVを取得・加工し、データベースへバルク保存する処理です。

#### 1. Zoom: Overview（広角・大局フロー）
処理の全体像と主要なデータストリームの接続だけをラフに合意します。
```ruby
workflow :user_data_etl do
  description "外部CSVダウンロード、加工、DB保存パイプライン"

  stream :csv_lines, from: task(:extract, source: "https://example.com/users.csv")
  stream :records,   from: task(:transform, input: :csv_lines)

  task :load, input: :records, into: :user_database
end
```

#### 2. Lens: Pipeline（EIP視点：分割・集約・隔離）
データがパイプラインを流れる際の「分割（Splitter）」「集約（Aggregator）」「不正データの隔離（Quarantine）」に焦点を当てます。
```ruby
task :transform, lens: :pipeline do
  input stream: :csv_lines

  # 1行ずつ分解して処理（Splitter）
  split_each :csv_lines do |line|
    on_invalid_format do
      quarantine to: :error_dlq # 不正電文はDLQへ退避
    end

    transform line, into: :user_record
  end

  # 1,000件ごとに束ねて下流へ放出（Aggregator / Batching）
  aggregate :user_record, every: 1_000, into: stream(:records)
end
```

#### 3. Lens: Concurrency & Backpressure（Petri Net視点：並行・同期・背圧）
タスク間の並行稼働と、下流が詰まった際の「流量制限（背圧）」に焦点を当てます。
```ruby
workflow :user_data_etl, lens: :concurrency do
  # タスクの並行起動（Parallel Execution）
  parallel do
    run task: :extract
    run task: :transform
    run task: :load
  end

  # ストリームのバッファ上限と背圧制御（Petri Net: Bounded Capacity）
  stream :csv_lines do
    capacity 5_000 # 最大5000行までメモリ保持
    on_overflow :backpressure # 溢れたら上流のExtractを物理的に一時停止
  end
end
```

---

### 6.2 例2：Fintech 口座間送金（契約・ライフサイクル・補償）
2つの口座間での資金移動における、権限、数値精度、不変条件、および分散トランザクションの巻き戻しです。

#### 1. Lens: Security & Lifecycle（権限・状態遷移・不変条件）
誰が実行できるか、遷移の前提条件（Guard）、および絶対に破綻してはならない制約（Invariant）に焦点を当てます。
```ruby
task :transfer_funds, lens: :lifecycle do
  as_actor :authorized_user
  require_scope "payment:transfer"

  schema do
    field :from_account_id, type: :account_id
    field :to_account_id,   type: :account_id
    field :amount,          type: :decimal, precision: 18, scale: 2, min: 1 do
      rounding :truncate # 丸めは切り捨て（Float厳禁）
    end
  end

  # 状態不変条件（State Invariant）
  invariant "送金前後で二口座の残高総計は完全に一致すること"

  state_machine :transfer_status do
    state :draft, initial: true
    state :pending
    state :settled

    transition from: :draft, to: :pending do
      # 遷移ガード（Guard Condition）
      guard "from_account の利用可能残高 >= amount"
    end

    transition from: :pending, to: :settled
  end
end
```

#### 2. Lens: Exception & Compensation（Saga視点：補償巻き戻し）
分散環境において、片方の処理が失敗した場合の自動取り消し（Sagaパターン）に焦点を当てます。
```ruby
task :execute_transfer, lens: :compensation do
  saga :transfer_flow do
    # ステップ1：引き落とし
    step :debit do
      action { account(:from_account_id).withdraw(amount) }
      # 失敗時の補償トランザクション（引き落としたお金を返金）
      compensate { account(:from_account_id).refund(amount) }
    end

    # ステップ2：入金
    step :credit do
      action { account(:to_account_id).deposit(amount) }
      # 入金が失敗した場合は、自動的に step: :debit の compensate が発動する
    end
  end
end
```

---

### 6.3 例3：Web API & 非同期イベント（相互作用とタイミング）
注文受付APIにおける、同期処理（即時応答）と非同期処理（メール通知）、および時間制約・保護です。

#### 1. Lens: Interaction & Sequence（同期/非同期の相互作用）
クライアントとサーバー、および後続ワーカー間の対話形式に焦点を当てます。
```ruby
task :order_checkout, lens: :interaction do
  input :http_request
  output :http_response

  # 同期呼び出し（Call-Return / 応答をブロックして待つ）
  order = sync_call task(:create_order, params: :http_request)
  payment = sync_call task(:charge_credit_card, order: order)

  # 即時レスポンスを返却
  reply_to_client status: 201, body: order

  # 後続処理は非同期放流（Fire-and-Forget / 待たずにバックグラウンドへ）
  fire_and_forget event(:order_placed, payload: { order_id: order.id }) do
    propagate_context :trace_id # 分散トレースIDを後続へ伝播
  end
end
```

#### 2. Lens: Timing & Reliability（時間制約・保護）
単位時間あたりの流量枠、タイムアウト、および再試行に焦点を当てます。
```ruby
task :charge_credit_card, lens: :timing do
  # 流量制限（Rate Limit）
  rate_limit max: 100, per: 1.minute

  # タイムアウト制約（Duration Constraint）
  timeout 3.seconds do
    on_timeout :abort_and_notify
  end

  # 一時障害への再試行ポリシー（Retry with Backoff）
  retry_policy do
    max_attempts 3
    backoff :exponential, initial: 500.ms, multiplier: 2.0
  end
end
```

---

## 7. 今後の検討課題

1. **形式意味論の段階的整理**  
   契約（`guarantee`）の種類や組み合わせの妥当性ルール（例: `at_least_once` なストリームを受け取るタスクは `idempotent` であるべき、等）の静的検証ルールの整理。
2. **AIプロンプト／スキルの策定**  
   LWN ⇄ 実装コード（Go, TypeScript等）の相互変換プロンプトおよび評価ルールのプロトタイプ検証。
3. **コミット間での差分分析（Diffing）検証**  
   Gitコミット間でLWNを逆生成・比較し、設計の劣化や偶発的複雑さの検知が行えるかのPoC実験。

---

## 8. 参考文献・理論的背景（References）

LWNのロジック抽象化モデル、レンズ体系、および構文設計は、以下の学術研究・業界標準パターンを理論的背景としています。

1. **Workflow Patterns**
   - W.M.P. van der Aalst, A.H.M. ter Hofstede, B. Kiepuszewski, and A.P. Barros. *"Workflow Patterns."* Distributed and Parallel Databases, 14(1):5–51, 2003.
   - 公式サイト: [http://www.workflowpatterns.com/](http://www.workflowpatterns.com/)
   - *（制御フロー、データ、リソース、例外処理の普遍的処理パターンの基礎）*

2. **Enterprise Integration Patterns (EIP)**
   - Gregor Hohpe and Bobby Woolf. *"Enterprise Integration Patterns: Designing, Building, and Deploying Messaging Solutions."* Addison-Wesley, 2003.
   - 公式パターンカタログ: [https://www.enterpriseintegrationpatterns.com/patterns/messaging/](https://www.enterpriseintegrationpatterns.com/patterns/messaging/)
   - *（パイプライン、ルーター、スプリッター、アグリゲーター、メッセージチャネルの基礎）*

3. **Petri Nets & Colored Petri Nets (CPN)**
   - Carl Adam Petri. *"Kommunikation mit Automaten."* PhD thesis, University of Bonn, 1962.
   - Kurt Jensen. *"Coloured Petri Nets: Basic Concepts, Analysis Methods and Practical Use."* Springer, 1997.
   - *（自律発火、同期・待ち合わせ、トークン競合、容量制限・背圧制御の数理モデル）*

4. **UML Dynamic Models (OMG UML Specification)**
   - Object Management Group (OMG). *"Unified Modeling Language (UML) Specification."*
   - *（Sequence Diagram: 相互作用・呼び出し順、State Machine Diagram: 状態遷移・ガード・不変条件、Timing Diagram: 時間制約・タイムアウト）*

5. **Communicating Sequential Processes (CSP) & Actor Model**
   - C.A.R. Hoare. *"Communicating Sequential Processes."* Communications of the ACM, 21(8):666–677, 1978.
   - Carl Hewitt, Peter Bishop, and Richard Steiger. *"A Universal Modular ACTOR Formalism for Artificial Intelligence."* IJCAI, 1973.
   - *（タスク間での非同期/同期メッセージパッシングと文脈伝播の基礎）*

6. **プログラミング言語構文の認知科学・人間工学研究**
   - Andreas Stefik and Susanna Siebert. *"An Empirical Investigation into Programming Language Syntax."* ACM Transactions on Computing Education (TOCE), 13(4):1–40, 2013.
   - *（C系記号構文の認知負荷と、Ruby等の自然言語に近いキーワード・構造化構文の優位性を実証した研究）*
