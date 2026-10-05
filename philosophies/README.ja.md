# 思想と背景

> Note: 本リポジトリの問題意識、主要な概念、最終判断は筆者（人間）によるものです。一方で、文章の構成、表現の整理、下書きの生成にはAIの支援を受けています。最終的な内容の責任は筆者にあります。

このディレクトリは、ISE（Intent-Synchronized Engineering）、IGE（Intent Guidance Engineering）、ODF（Orthogonal Dual-Framework）の背景にある考え方を整理する文書群である。

ISE、IGE、ODFは、筆者が本稿群で提唱する呼称であり、既存の標準化された開発手法や広く合意された業界用語ではない。

3つのフレームワークの全体像は[概要](../overview.ja.md)、ISE個別の文書は[ISEドキュメント](../ise/README.ja.md)を参照。

## 読む順番

次の順番で読むことを想定している。

1. [ソフトウェアエンジニアリングの歴史と、現場が抱え続ける「終わらない課題」](01_history_and_unending_challenges.ja.md)
2. [ドキュメント・ドリフトとソフトウェア考古学：現場を苦しめてきた情報のモザイク](02_document_drift_and_software_archaeology.ja.md)
3. [AIの登場とVibe Coding、SDD/IDDの限界](03_ai_paradigm_and_sdd_limits.ja.md)
4. [Intent-Synchronized Engineering：意図の同期を中心に置くエンジニアリング](04_intent_synchronized_engineering.ja.md)
5. [工学的フィードバックとしてのレビュー：主観的点検から多面的評価系へ](05_feedback_systems_and_assessment_engineering.ja.md)
6. [AI時代における人間の役割と責任の再設計](06_roles_and_responsibilities_in_ai_era.ja.md)

補論として、次の文書を置く。

- [ドキュメント・ドリフトと意図の劣化：なぜ意図は同期されなければならないのか](../ise/foundations/01_document_drift_and_intent_decay.ja.md)

用語の確認には、次を参照する。

- [用語集](../ise/glossary.ja.md)

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

### 06: AI時代の人間の役割と責任

`06_roles_and_responsibilities_in_ai_era.ja.md` は、AI時代に人間が負う責任の再設計を扱う。

「どの職種が残るか」ではなく「人間がどこに最終責任を負うか」を問う。全自動論とAI拒絶論の二極論を退け、エンジニア（アーキテクト寄り〜クラフトマン寄り）とPM（ビジネス・ディスカバリー寄り〜プロダクトアーキテクト寄り）の役割を連続的なグラデーションとして整理する。そのうえで、ISE、IGE、ODFが各グラデーションの上に立つ人の責任を支える土台であることを示す。
