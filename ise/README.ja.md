# ISEドキュメント

> Note: 本リポジトリの問題意識、主要な概念、最終判断は筆者（人間）によるものです。一方で、文章の構成、表現の整理、下書きの生成にはAIの支援を受けています。最終的な内容の責任は筆者にあります。

このディレクトリは、Intent-Synchronized Engineering（ISE: 意図同期型エンジニアリング）に関する考え方を整理するための文書群である。

ISEは、筆者が本稿群で提唱する呼称であり、既存の標準化された開発手法や広く合意された業界用語ではない。中心にあるのは、コード、テスト、仕様、設計、Issue、ADR、運用記録などの成果物を、共通の意図へ継続的に同期するという考え方である。

## 読む順番

まずは、次の順番で読むことを想定している。

1. [ソフトウェアエンジニアリングの歴史と、現場が抱え続ける「終わらない課題」](01_history_and_unending_challenges.ja.md)
2. [ドキュメント・ドリフトとソフトウェア考古学：現場を苦しめてきた情報のモザイク](02_document_drift_and_software_archaeology.ja.md)
3. [AIの登場とVibe Coding、SDD/IDDの限界](03_ai_paradigm_and_sdd_limits.ja.md)
4. [Intent-Synchronized Engineering：意図の同期を中心に置くエンジニアリング](04_intent_synchronized_engineering.ja.md)
5. [工学的フィードバックとしてのレビュー：主観的点検から多面的評価系へ](05_feedback_systems_and_assessment_engineering.ja.md)

補論として、次の文書を置く。

- [ドキュメント・ドリフトと意図の劣化：なぜ意図は同期されなければならないのか](foundations/01_document_drift_and_intent_decay.ja.md)

用語の確認には、次を参照する。

- [用語集](glossary.ja.md)

## 各文書の位置づけ

### 01: 歴史と既存方法論

`01_history_and_unending_challenges.ja.md` は、ソフトウェアエンジニアリングがどのように考えられてきたのかを整理する。

ウォーターフォール的な順方向開発、ISO/IEC/IEEE 12207、15288、29148、42010、ISO/IEC 25010、SWEBOK、PMBOK、反復型開発、アジャイル、スクラムなどを扱う。

ここではISEを前面に出すのではなく、既存の方法論がどのような問題意識を持ち、どのような限界を残してきたのかを確認する。

### 02: ドリフト、考古学、モザイク

`02_document_drift_and_software_archaeology.ja.md` は、現場で日常的に起きている問題を整理する。

ドキュメント・ドリフト、ソフトウェア考古学、情報のモザイク、Vibe Management、技術的負債、理解負債を扱う。

ここでの中心は、開発者がコードを書く前に、まず「今このシステムは何なのか」「どの情報が現在も正しいのか」「なぜこうなっているのか」を復元しなければならないという問題である。

### 03: AI時代の既存手法の限界

`03_ai_paradigm_and_sdd_limits.ja.md` は、AIの登場によって何が変わり、何がまだ解けていないのかを整理する。

Vibe Coding、SDD、IDD、Harness Engineering、AI-DLC / AI-SDLC などを扱う。AIはコード生成を高速化するが、意図の定義、検証、同期、維持を自動的に解決するわけではない。

ここでは、AI時代の問題が「コード生成」ではなく、「意図と成果物をどう同期するか」へ移っていることを示す。

### 04: ISEの提唱

`04_intent_synchronized_engineering.ja.md` は、ISEの中心文書である。

ISEは、IntentとRealityを、複数の成果物を通じて継続的に同期するための実装側フレームワークとして定義される。

ここでは、SSoTを絶対真理ではなく交渉の決着として扱うこと、順方向だけでなく逆方向の同期を扱うこと、作成主体を人間かAIかで固定しないこと、認知負荷の管理、Intent Recovery、AIを補装具として使うこと、IGE/ODFが必要になる理由を整理する。

### 05: 工学的フィードバックとしてのレビュー

`05_feedback_systems_and_assessment_engineering.ja.md` は、ISEにおける評価系（Assessment）とフィードバック制御を扱う。

レビューを人の主観的点検や承認ゲートから、制御工学における「フィードバック制御系」として再定義する。Fagan Inspection、Perspective-Based Reading（PBR）、IEEE 1028、Microsoftの実証研究などの知見に基づき、レビューの目的と手段を直交化（Defect / Validity / Complexity）すること、AI時代における代替と人間の役割分担、閉ループ制御としてのエンジニアリングの成立を論じる。

### foundations: 理論的背景

`foundations/01_document_drift_and_intent_decay.ja.md` は、04の補論である。

なぜ意図は失われるのか。なぜドキュメント・ドリフトは起きるのか。なぜ文字による意図同期は強力だが難しいのか。なぜAIは文字化が苦手な人を排除せず、むしろ補助しうるのか。

この文書では、意図の外部化、文字中心性、記憶の再構成、意図保持の減衰、共通基盤、認知負荷、成果物間の時間差を扱う。

## ISEの責任範囲

ISEは、主に実装側の意図同期を扱う。

```text
ISE:
  accepted intent
    -> spec / design / code / test / docs / operation への同期
```

ISEは、曖昧な要求から意図を作ること、価値判断を決めること、納期を保証すること、マネジメント側の曖昧さを吸収することを主責務にしない。

曖昧な要求、観測、痛み、不満、運用信号、市場信号から実装可能なintentやissueを作る領域は、IGEが扱うべきである。ISEとIGEを混ぜずに接続する全体構造は、ODFが扱う。

```text
IGE:
  vague signal / pain / request
    -> intent / issue / implementable direction

ISE:
  accepted intent
    -> spec / design / code / test / docs / operation

ODF:
  IGEとISEを混ぜず、限定されたインターフェースで接続する
```

## 今後の予定

現時点で、今後追加する価値が高い文書は次の通りである。

- IGEの導入文書
- ODFの導入文書
- lane-modelの概要
- ISE/IGE/ODFとスクラムの関係
- 具体例、ケーススタディ

特にスクラムについては、短い周期でプロダクト意図、ステークホルダー期待、バックログ優先順位、チームの作業リズムを同期しようとした実践として扱える。一方で、要求探索、価値判断、実装同期、成果物同期を明確に分離しなかったため、IGE/ODFの観点から再整理する余地がある。
