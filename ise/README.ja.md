# ISEドキュメント

> Note: 本リポジトリの問題意識、主要な概念、最終判断は筆者（人間）によるものです。一方で、文章の構成、表現の整理、下書きの生成にはAIの支援を受けています。最終的な内容の責任は筆者にあります。

このディレクトリは、Intent-Synchronized Engineering（ISE: 意図同期型エンジニアリング）に関する考え方を整理するための文書群です。

ISEは、筆者が本稿群で提唱する呼称であり、既存の標準化された開発手法や広く合意された業界用語ではありません。中心にあるのは、コード、テスト、仕様、設計、Issue、ADR、運用記録などの成果物を、共通の意図へ継続して同期させるという考え方です。

ここでいう意図そのものは、人の頭の中にのみ存在します。成果物は意図を外部化した表現物であり、外部化の過程で情報が抜け落ちることがあります。ISEは、この表現物を関係者の合意によって決着させ、実装（Reality）との間で同期させ続けます。

## 読む順番

ISEの背景と提唱の流れは、[思想と背景のREADME](../philosophies/README.ja.md)に記載した読む順番に従う想定です。

ISE固有の文書として、以下を用意しています。

- [ドキュメント・ドリフトと意図の劣化：なぜ意図は同期されなければならないのか](foundations/01_document_drift_and_intent_decay.ja.md)（`philosophies/04` の補論）
- [用語集](glossary.ja.md)

## 各文書の位置づけ

思想と背景（01〜06）の各文書の位置づけは、[思想と背景のREADME](../philosophies/README.ja.md)を参照してください。

### foundations: 理論的背景

`foundations/01_document_drift_and_intent_decay.ja.md` は、04の補論です。

なぜ意図は失われるのか。なぜドキュメント・ドリフトは起きるのか。なぜ文字による意図同期は強力でありながら難しいのか。なぜAIは文字化が苦手な人を排除せず、むしろ補助しうるのかを論じます。

この文書では、意図の外部化、文字中心性、記憶の再構成、意図保持の減衰、共通基盤、認知負荷、成果物間の時間差を扱います。

## ISEの責任範囲

ISEは、主に実装側の意図同期を扱います。

```text
ISE:
  accepted intent
    -> spec / design / code / test / docs / operation への同期
```

ISEは、曖昧な要求から意図を作ること、価値判断を決めること、納期を保証すること、マネジメント側の曖昧さを吸収することを主たる責務とはしません。

曖昧な要求、観測、痛み、不満、運用信号、市場信号から実装可能な意図（intent）やIssueを作る領域は、IGEが担います。ISEとIGEを混ぜずに接続する全体構造は、ODFが扱います。

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

## 関連文書

本フレームワーク群（Environment ⇄ Intent ⇄ Reality）を構成するIGEやODF、およびODF/IGE/ISEの間やそれぞれの内部など、あらゆる同期・変換の流れを有向グラフとして捉える共通概念モデルであるlane-modelについては、次の文書があります。

- [3つのフレームワークの全体像（Environment ⇄ Intent ⇄ Reality）](../overview.ja.md)
- [IGE概要](../ige/overview.ja.md)
- [ODF概要](../odf/overview.ja.md)
- [lane-modelの思想と概念](../lane-model/01_philosophy_and_concepts.ja.md)

## 今後の予定

現時点で、今後追加する価値が高い文書は次のとおりです。

- ISE/IGE/ODFとスクラムの関係
- 具体例、ケーススタディ

特にスクラムについては、短い周期でプロダクト意図、ステークホルダーの期待、バックログの優先順位、チームの作業リズムを同期しようとした実践として扱えます。一方で、要求探索、価値判断、実装同期、成果物同期を明確に分離しなかったため、IGE/ODFの観点から再整理する余地があります。
