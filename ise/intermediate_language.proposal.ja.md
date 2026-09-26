# ISE 中間言語（Intent Specification Language）構想提案

本ドキュメントは、ISE（Intent Synchronized Engineering）において人とコードの間に位置する**「中間言語（擬似言語）」**の構想およびシンタックスの提案書です。

現時点では**提案レベル（Proposal）**であり、シンタックスの方向性と代表的な意味論の例を中心に定義しています。厳密な形式意味論や仕様策定は今後の拡張課題とします。

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
  人間が責任を持つべき対象は、実装の細かい文法ではなく**「何を作りたいのか（要求）」**と**「システムの振る舞い・契約が正しいか（仕様）」**になります。これらを非エンジニアでも直感的に読め、かつAIが厳密にコードへ変換・検証できる表現手段が求められます。

### 1.3 本質的複雑さと偶発的複雑さの分離
ソースコードから中間言語への**逆変換（Code → 中間言語）**を行うことで：
- **本質的複雑さ（設計レベルの複雑さ）**：業務フローや状態遷移、依存関係そのものの複雑化
- **偶発的複雑さ（実装レベルの複雑さ）**：言語特有のボイラープレート、ライブラリの癖、不適切なコーディングによる複雑化

を客観的に分離して評価できます。過去のコミットと現在のコミットを中間言語レベルで比較することで、「設計そのものが破綻・複雑化しているのか、単に実装の書き方の問題なのか」を判断する指標が得られます。

---

## 2. コア設計思想

1. **実行は不要、大まかな処理が書ける軽量言語**  
   コンパイラやインタプリタで直接実行することを目的とせず、人間とAIが意図を共有・検証するためのプロトコルとして機能します。
2. **「実現手段（How）」は抽象化し、「保証する性質（Contract / Invariant）」を明示する**  
   OSスレッドかGoroutineか、UnixドメインソケットかTCPかパイプかといった実現手段はコード側の裁量とします。ただし、「順序保証が必要」「冪等であること」といった障害時や並行性に直結する性質は明示します。
3. **ビジネスに影響する厳密性（ドメイン制約・型）の漸進的明示**  
   Fintechにおける数値精度（丸め規則、桁数）や、ETLにおける文字エンコーディングなど、ビジネス破綻に直結する要素は「実装詳細」として切り捨てず、意味論的型（Semantic Type）や契約として厳密に指定できるようにします。
4. **段階的な抽象度（Layer構造）とプロファイル拡張**  
   人に近い大局的なレイヤーから、障害設計や厳密な契約を扱うエンジニア向けレイヤーまで同一の文法体系で段階的に記述します。また、ドメイン特性（Fintech、ETL、Web等）に応じたボキャブラリの拡張を許容します。
5. **自然言語の埋め込み（Escape Hatch）と外部アダプタ化**  
   画面のレイアウトや定型的なCRUD表示など、疑似コード化する価値の低い末端仕様は自然言語ブロックとしてそのまま埋め込みます。GUIや外部連携プロトコルはコア言語の「外側の概念」として扱います。

---

## 3. シンタックス提案：Rubyライク構文

### 3.1 採用理由
- **非エンジニアに対する可読性**  
  記号（`{}`、`;` 等）のノイズが少なく、`do ... end` による明確なブロック構造と英語に近い語順を持つため、非エンジニアが見ても「暗号」ではなく「構造化された英文」として理解できます。
- **Pythonとの対比**  
  Pythonも可読性は高いですが、厳密なインデント規則（コピー＆ペースト時の崩れに弱い）や特有の記法（`def`, `self`, `@decorator`）があります。中間言語としては、ドメイン概念（名詞・動詞）をブロックで表現しやすいRubyスタイルのDSLが最も親和性が高いと考えます。

### 3.2 基本キーワード（ドラフト）
- `task`: 自律的・並行に実行可能な処理の単位
- `stream`: タスク間をつなぐデータの流れ
- `resource`: DB、一時ディレクトリ（TmpDir）、外部API等の依存先
- `schema` / `field`: データの構造および意味論的型の定義
- `guarantee`: 満たすべき契約・性質（順序性、冪等性、精度等）
- `view`: UIやプレゼンテーションの定義（自然言語埋め込み・外部参照）
- `on_error`: 例外・エラー時の振る舞い定義

---

## 4. レイヤーモデル（段階的詳細化）

数字が小さいほど人間に近く、大きいほどエンジニア・障害設計・ビジネス厳密性寄りになります。

| レイヤー | 想定利用者 | 記述する内容 | 意図的に除外する内容 |
| :--- | :--- | :--- | :--- |
| **Layer 0** | PO、非エンジニア、設計初期 | 処理の大まかな流れ、主たるTaskとStream、大枠の入出力 | エラー処理、リトライ、詳細プロトコル、厳密な型・精度 |
| **Layer 1** | PO、リードエンジニア | 結果種別（HTTP status、終了コード、画面遷移）、大まかなフォールバック | 障害ドメイン分離、詳細なトランザクション境界、バイナリ形式 |
| **Layer 2〜3** | エンジニア間、AI実装生成 | 契約・保証（Ordered, Idempotent, Atomic等）、意味論的型（Decimal精度、Encoding）、リソース・トランザクション境界 | 具体的な実現ライブラリ、OSスレッド/goroutine等の低レベル手段 |

---

## 5. 意味論（セマンティクス）の体系

OSや特殊な組み込みを除き、通常のWeb・ネイティブアプリ・業務システムで共通して必要な意味論を**コア概念**として定義し、GUIやドメイン固有の厳密性は**コアの外側（プロファイル／外部アダプタ）**として整理します。

### 5.1 コア概念（すべての通常開発で共通）
1. **データ構造と意味論的型（Schema / Semantic Types）**:
   単なるメモリ表現（32bit/64bit）ではなく、ビジネス上の正確性を保証する型定義。
   - 数値精度: `decimal(precision, scale)`、`currency(:JPY)`、丸め規則（`rounding: :half_up` 等）
   - 文字列・エンコーディング: `encoding(:shift_jis)`、`format(:email)`
2. **実行単位と並行性（Task）**:
   ある目的を持った自律的実行単位。内部の並行性（マルチスレッド、Goroutine、イベントループ等）は実装に委ねられます。
3. **データ伝送と結合（Stream / Channel）**:
   Task間で受け渡されるデータの系列。パイプ、ソケット、チャネル、メッセージキュー等のいずれで実現されてもよいデータ伝送路。
4. **外部境界と永続化（Resource）**:
   DB、一時ディレクトリ（TmpDir）、外部API等。ライフサイクルやトランザクション境界を扱います。
5. **契約と不変条件（Guarantee / Traits）**:
   手段は問わないが、システムが満たすべき性質を宣言します。
   - **Stream**: `ordered`（順序保証）、`unordered`（並列可）、`at_least_once`（重複許容）、`lossless`（背圧等で欠落防止）
   - **Task**: `idempotent`（冪等性）、`atomic`（原子性）、`isolated`（障害隔離）

### 5.2 コアの外側に置く追加概念（Presentation / Domain Profiles）
- **GUI・画面表現（View）**:
  画面のマークアップ、レイアウト、スタイルなどは中間言語コアに含めず、**自然言語の指示ブロック**や**HTML/CSS、デザインツール（Figma等）への参照**として扱います。中間言語側の関心は「Viewに何を渡し、Viewからどんなイベントが返るか」という入出力境界のみです。
- **ドメイン別プロファイル（Profiles）**:
  - **Fintech Profile**: 通貨、丸め、勘定元帳（Ledger）、監査ログ（AuditTrail）
  - **ETL/MasterData Profile**: 固定長電文（fixed_width）、文字コード変換、差分更新（Upsert）、隔離（Quarantine）

### 5.3 あえて「捨てる」要素（偶発的複雑さの排除）
- メモリ管理・参照・ポインタ・GCの意識
- ループ制御構文（`for`, `while`, `break`）：コレクション操作やStreamの消費に一元化
- 低レベルな例外クラス構造（`try/catch`）：結果種別とフォールバックの宣言に集約
- 言語都合のクラス継承やメタプログラミング

---

## 6. 記述例

### 6.1 例1：バッチ処理（URLからZipダウンロード・展開・DB保存）

#### Layer 0（大局フロー）
```ruby
task :download_and_store do
  description "指定URLからアーカイブを取得し、展開してDBに格納する"

  stream :zip_archive, from: fetch("https://example.com/data.zip")
  stream :records, from: extract(stream: :zip_archive, to: :tmp_dir)

  store stream: :records, into: :database
end
```

#### Layer 2/3（契約と障害特性の明示）
```ruby
task :download_and_store do
  resource :database, type: :relational
  resource :work_dir, type: :tmp_dir, cleanup: :on_exit

  stream :zip_archive, from: fetch("https://example.com/data.zip") do
    guarantee :lossless
  end

  stream :records, from: extract(stream: :zip_archive, to: :work_dir) do
    guarantee :ordered
  end

  task :save_record do
    guarantee :idempotent # 重複投入されても問題ない設計を要求

    store stream: :records, into: :database do
      on_conflict :update_existing
    end
  end

  on_error :network_failure do
    retry limit: 3, backoff: :exponential
    notify_channel "バッチ失敗: ダウンロード不能"
  end
end
```

---

### 6.2 例2：Webアプリケーション（ユーザー一覧画面）

自然言語エスケープを活用し、画面仕様の細かいマークアップは自然言語に委ねる例です。

```ruby
task :show_user_list do
  description "管理用ユーザー一覧の検索・表示"

  input :http_request
  output :http_response

  stream :users, from: :database do
    guarantee :ordered_by, field: :created_at, direction: :desc
  end

  # UI・画面仕様は自然言語で記述（コア外の概念）
  view do
    """
    - id, ユーザー名, email, 登録日 をテーブル形式で表示する。
    - ページネーションは50件単位。
    - 検索条件に該当するユーザーが0件の場合は「該当するユーザーはいません」とメッセージ表示する。
    """
  end

  on_error :database_timeout do
    status 503
    message "現在データベースが混み合っています。時間を置いて再度お試しください。"
  end
end
```

---

### 6.3 例3：Fintech（口座間送金：数値精度とトランザクション）

ビジネス直結の数値型（Floatの禁止）と整合性保証の例です。

```ruby
task :transfer_funds do
  description "二つの口座間での送金処理（残高引き落としと入金）"

  schema do
    field :from_account_id, type: :string
    field :to_account_id,   type: :string
    field :amount,          type: :decimal, precision: 18, scale: 2, min: 0.01 do
      rounding :truncate # 丸めは切り捨てを明示（Float厳禁）
    end
    field :currency,        type: :currency_code # ISO 4217 (例: JPY, USD)
  end

  transaction do
    isolation :serializable
    guarantee :atomic

    step :withdraw do
      task :debit, account: :from_account_id, amount: :amount
    end

    step :deposit do
      task :credit, account: :to_account_id, amount: :amount
    end
  end

  on_error :insufficient_funds do
    status 422
    message "残高が不足しています。"
  end
end
```

---

### 6.4 例4：マスターデータ統合・ETL（電文文字コードとUPSERT戦略）

外部連携特有のエンコーディングやETL処理方法の例です。

```ruby
task :sync_bank_master do
  description "全銀フォーマットのマスターデータSFTP取り込み"

  stream :raw_file, from: sftp_source("/inbound/bank_master.dat") do
    format :fixed_width
    encoding :shift_jis # 文字化けや桁ずれを防ぐための必須契約
  end

  stream :validated_records, from: transform(:raw_file) do
    on_invalid_record :quarantine # 不正レコードは隔離キューへ退避
  end

  store stream: :validated_records, into: :master_database do
    strategy :upsert, key: :bank_code
  end
end
```

---

## 7. 今後の検討課題

1. **形式意味論の段階的整理**  
   契約（`guarantee`）の種類や組み合わせの妥当性ルール（例: `at_least_once` なストリームを受け取るタスクは `idempotent` であるべき、等）の静的検証ルールの整理。
2. **AIプロンプト／スキルの策定**  
   中間言語 ⇄ 実装コード（Go, TypeScript等）の相互変換プロンプトおよび評価ルールのプロトタイプ検証。
3. **コミット間での差分分析（Diffing）検証**  
   Gitコミット間で中間言語を逆生成・比較し、設計の劣化や偶発的複雑さの検知が行えるかのPoC実験。
