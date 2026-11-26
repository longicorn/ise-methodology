# ISE Document Engineering の問題提起

## 目的

この文書は、既存のドキュメント実践を踏まえた上で、ISE として何を問題にし、どのような文書体系を考えるべきかを整理する。

ISE Document Engineering の中心は、単に「よい設計書を書く方法」ではない。

中心にある問いは次である。

> ISE におけるドキュメントは、Intent と Reality の同期状態を、人間と AI の両方が操作可能にするための artifact であるべきではないか。

この立場では、ドキュメントは工程の副産物ではなく、同期、交渉、昇格、検証、再構成を支える操作対象である。

## ISE におけるドキュメントの再定義

ISE では、開発の本質を固定的な工程順序ではなく、Intent と Reality の継続的な同期として捉える。

- Intent
  - システムをどうあるべきと考えるか
- Reality
  - 実際のコード、テスト、設定、動作、運用状態
- Synchronization
  - Intent と Reality のズレを検出し、交渉し、再び整合させること

この前提に立つと、ドキュメントは「説明資料」では足りない。

ドキュメントは、Intent を外部化し、Reality を観測し、両者の差分を扱い、人間が判断した結果を残すための artifact である。

したがって、ISE におけるドキュメントは次の性質を持つべきである。

- Intent を保持する
- Reality を観測可能にする
- Drift を検出可能にする
- 判断と合意を記録する
- Reverse Sync の結果を受け止める
- AI に渡す context surface として機能する
- 人間が採用判断を下せる形で情報を分離する
- Eventual Consistency を前提に一時的な不整合を扱える

## 工程成果物から同期 artifact へ

従来の文書体系では、要求書、設計書、テスト仕様書、運用手順書のように、工程や用途ごとに文書が分かれることが多い。

ISE では、この分け方を完全には捨てない。しかし一次分類としては不十分である。

なぜなら、ISE で重要なのは「どの工程で作られた文書か」ではなく、「同期構造の中で何を担っているか」だからである。

たとえば同じ design という名前の文書でも、次のように意味が異なる。

- これから実装するための To-Be design
- 実装後に Reality から復元された As-Is design
- 現実と意図の差分を示す drift analysis
- 採用前の design candidate
- 人間が決定した adopted design

これらを同じ設計書として扱うと、文書はすぐに曖昧になる。

ISE Document Engineering では、文書を工程成果物ではなく、同期状態を持つ artifact として扱う必要がある。

## Intent / Reality / Knowledge / Decision の分離

ISE における文書体系では、少なくとも次の四種類を混ぜないことが重要である。

### Intent Document

Intent Document は、どうあるべきかを表す。

ここには要求、設計意図、制約、品質方針、採用済み判断などが含まれる。

重要なのは、Intent Document は単なる希望や推測ではなく、その時点で採用された意図を表すことである。未確定の案やコードから推測された意図候補は、ここに直接混ぜるべきではない。

### Reality Document

Reality Document は、実際にどうなっているかを表す。

これはコード、テスト、設定、運用状態、実際の振る舞いなどに基づく。Reality Document の価値は、主観的な理想ではなく、観測された事実に接地している点にある。

ただし、Reality Document は Intent ではない。実装されているから正しい、とは限らない。

### Knowledge Document

Knowledge Document は、Reality や人間由来の情報を整理し、理解可能な形に再構成したものである。

analyze モード（mode）における gather、mosaic、synthesize、elevate は、この領域と強く関係する。

Knowledge Document は、単なる事実一覧でも、正式な仕様でもない。事実、背景、仮説、確度、出所を整理し、判断可能な状態へ近づけるための中間 artifact である。

### Decision Document

Decision Document は、なぜその選択をしたのかを表す。

ここには採用理由、棄却案、トレードオフ、リスクの引受け、判断主体、判断時点などが含まれる。

コードには How が残りやすい。テストには What が残りやすい。しかし Why や Why not は残りにくい。Decision Document は、この失われやすい情報を同期構造の中に戻すために必要である。

## As-Is / Pre To-Be / To-Be の分離

ISE Document Engineering で特に重要なのは、As-Is、Pre To-Be、To-Be を分けることである。

### As-Is

As-Is は、現実の再構成である。

コードや設定、テスト、実行結果などから観測できる事実を中心に、現在のシステムがどうなっているかを整理する。

As-Is は重要だが、それ自体は正しさを意味しない。As-Is は Reality の記述であって、採用済み Intent ではない。

### Pre To-Be

Pre To-Be は、Reality や人間由来の断片情報から導かれた改善候補、意図候補、問題提起である。

ここには次のようなものが含まれる。

- pain point
- drift signal
- intent candidate
- design candidate
- risk candidate
- confidence
- provenance

Pre To-Be は、ISE と IGE の境界にある重要な受け皿である。

Reality から何かが見えても、それをすぐ正式な Intent にしてはいけない。Pre To-Be として保持し、必要に応じて IGE や人間の判断へ渡す。

なお、Reverse Sync で観測事実と推定・不明点をまとめて作る暫定仕様の作業場を **PreSpec（Pre-Spec）** と呼ぶ。PreSpec も Pre To-Be も、そのままでは正式なドキュメント（採用済み To-Be）にはできない不完全な中間状態であり、PreSpec が現状理解の足場であるのに対し、Pre To-Be はそこから浮かび上がった意図候補・変更候補を受け止める段階に位置づく。

### To-Be

To-Be は、正式に採用された Intent である。

To-Be になるには、単に AI がそう推測しただけでは足りない。人間または責任主体による採用判断が必要である。

ISE において Living Spec と呼べるものは、静的な単一の SSoT を前提にするのではなく、この採用済み To-Be を中心に構成されるべきである。

## Reverse Sync が文書体系に要求するもの

Reverse Sync は、Reality から Intent 側へ情報を戻す動きである。

これは ISE の重要な特徴だが、同時に危険もある。

コードからは As-Is を復元できる。しかし As-Intended を完全に復元することはできない。実装されている構造から、なぜそうしたのか、何を捨てたのか、どのリスクを飲んだのかを完全に知ることはできない。

そのため Reverse Sync の結果は、採用済み Intent と区別する必要がある。

Reverse Sync で得られるものは、主に次のいずれかである。

- code-backed fact
- observed behavior
- inferred structure
- intent candidate
- drift signal
- pain point
- undocumented decision candidate

これらをそのまま仕様に混ぜると、Reality がいつの間にか Intent に昇格してしまう。

ISE Document Engineering では、Reverse Sync の結果に provenance、confidence、昇格状態を持たせる必要がある。

## 作業モード（mode）とドキュメント

ドキュメント体系を考える上では、作業を工程順序ではなく **作業モード（mode）** として捉えることが重要である。ここでの `mode` は操作単位としての作業の方向（change vector）を表し、[Lane Model](../lane-model/01_philosophy_and_concepts.ja.md) における Node 間の変換辺（Lane）を文脈ごとに束ねた入口に対応する。

- spec
  - Intent を明示化し Reality に接続する
- code
  - Reality を変更し Intent に近づける
- analyze
  - Reality を Knowledge として再構成する

この見方では、ドキュメントは spec モードだけのものではない。

spec モードには Intent を表す文書がある。analyze モードには As-Is や PreSpec、Pre To-Be を表す文書がある。code モードにも、局所的な実装判断や AI への意図伝達を支える文書がありうる。また、健全性評価やトリアージ（health / triage）に関する記録は **Gate** の評価・差し戻し判断に結びつき、修復作業（remedy）は差し戻しを受けた spec や code モードの再同期として扱われる。

つまり、ドキュメント分類は `mode / scope / artifact` の三つ組で考えるのが自然である。

```text
mode     = 何をするか（作業モード）
scope    = どこを対象にするか
artifact = その scope の中でどの成果物や観点を扱うか
```

この構造により、従来の system / feature / implement / analyze のような固定分類よりも、ISE の同期モデルに近い整理ができる。

## AI context surface としてのドキュメント

AI 支援開発では、ドキュメントは人間が読むためだけのものではない。

AI に渡す context surface としても機能する。

この観点では、文書には次の性質が必要になる。

- AI が必要な範囲だけを読める
- artifact の意味が文脈から解決できる
- scope が論理的に指定できる
- 古い情報と採用済み情報を区別できる
- 推測、事実、判断を混ぜない
- どの文書を優先すべきかが決まっている
- 人間が AI の出力を検証できる

つまり、ファイル名やディレクトリ構造は単なる整理ではない。

それらは AI が対象を解決するための操作面（target surface）の一部である。

## SSoT は絶対真理ではない（ISE は SSoT を前提としない）

ISE は、静的な単一の SSoT（Single Source of Truth）を前提としない。仕様、コード、運用上の現実、そして関わる人間を含め、すべての Node が時間とともに独立して変化し続けるためである。

あえて SSoT に相当するものを位置づけるとしても、それは唯一絶対の真実ではなく、その時点で交渉が決着した採用済み Intent のスナップショット（Living Spec）である。

この理解は重要である。

どこかに唯一絶対の SSoT があるとみなすと、Reality からの反証や実装中に見つかった新しい意図を受け止めにくくなる。一方で、採用済み Intent を単なるメモにしてしまうと、実装や検証を制約できなくなる。

ISE における採用済み Intent（Living Spec）は、次の性質を持つべきである。

- 採用済み Intent を表す
- Reality と同期される
- Drift によって更新対象になりうる
- 変更には判断と記録が必要である
- 未採用の候補とは区別される

このため、採用済み Intent の周囲には、候補、観測、差分、判断、棄却案を置くための文書構造が必要になる。

## ISE Document Engineering の中心命題

以上を踏まえると、ISE Document Engineering の中心命題は次のように置ける。

> ISE におけるドキュメントは、工程成果物ではなく、Intent / Reality / Knowledge / Decision を同期・交渉・昇格させるための操作可能な artifact である。

この命題から、次の設計原則が導かれる。

- 文書は工程順ではなく同期責務で分類する
- Intent と Reality と Knowledge と Decision を混ぜない
- As-Is と To-Be を混ぜない
- Pre To-Be を正式な Intent と混ぜない
- Reverse Sync の結果には provenance と confidence を持たせる
- 人間の採用判断を通ったものだけを Living Spec（採用済み Intent）に昇格する
- 棄却案、未確定事項、制約、判断理由を一級の記録対象にする
- ファイル名とディレクトリ構造は AI の解決可能性を考慮して設計する
- ドキュメントは human-readable であると同時に agent-readable であるべきである
- Eventual Consistency を前提に、一時的な Drift を表現できる構造にする

ISE Document Engineering は、この原則をもとに、具体的な実装やツールの document model、作業モード（mode）、artifact namespace、resolver、Reverse Sync、consultant support interface へ接続されるべきである。
