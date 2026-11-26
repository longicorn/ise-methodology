# goose-sdd と Nagi からの実践知

## 目的

この文書は、goose-sdd の実験結果と Nagi の開発・運用から見えてきた知見を、ISE Document Engineering へ吸い上げるための整理である。

ISE Document Engineering は、抽象理論だけでは成立しにくい。

実際に AI にドキュメントを読ませ、書かせ、コードと同期させ、Reverse Sync で Reality から文書を再構成しようとすると、どの情報が混ざりやすいか、どの文書名が解決しづらいか、どの分類が破綻しやすいかが見えてくる。

goose-sdd と Nagi は、そのための実践的な一次資料である。

## goose-sdd の位置づけ

goose-sdd は、ISE 以前の実践的な出発点だった。

中心にあった考え方は、`docs/sdd/` を Living Spec として扱い、ドキュメントとコードの Drift を Forward Sync と Reverse Sync によって管理することである。

goose-sdd では、主に次の構造を持っていた。

- system
  - プロダクト全体の背景、概念、アーキテクチャ、ルール、用語
- feature
  - 個別機能の要求、設計、テスト、実装方針
- implement
  - 小規模・高速な実装用の簡易ドキュメント
- analyze
  - 既存コードから As-Is を逆生成するための文書
- tool
  - 質問や補助操作

この構造は、SDD を実践する上で有効だった。

特に、仕様を先に明示してコード生成に使う Forward Sync と、既存コードから文書を回復する Reverse Sync の両方を試せたことは重要だった。

## goose-sdd で有効だったこと

goose-sdd から得られた有効な知見は多い。

### Living Spec は有効だった

ドキュメントを一度書いて終わりにするのではなく、コードとともに更新される Living Spec として扱うことは有効だった。

AI 支援開発では、文書が古いまま残ると、AI が古い前提を強く信じてしまう危険がある。Living Spec として更新し続ける方が、AI と人間の両方にとって安全である。

### Forward Sync は有効だった

仕様や設計を先に書き、それをもとに AI に実装させる流れは有効だった。

曖昧な依頼から直接コードを書くより、要求、設計、制約、テスト観点を文書化した上で実装する方が、意図の取りこぼしが少ない。

### Reverse Sync は有効だった

既存コードから As-Is を整理し、ドキュメントへ戻す流れも有効だった。

これは特に、既にコードが存在するプロジェクトや、実装が先行した場面で意味がある。Reality を観測し、文書側へ戻すことで、理解負債を減らせる。

### Negotiation Loop の必要性が見えた

Forward Sync と Reverse Sync を使うと、ドキュメントとコードのどちらが正しいかを単純には決められない場面が出てくる。

コードが仕様からズレている場合、それはバグかもしれないし、仕様が古いだけかもしれない。ここでは、人間が判断して Intent を更新する必要がある。

このことから、Drift、Analyze、Review、Convergence という Negotiation Loop の必要性が見えた。

## goose-sdd で見えた限界

goose-sdd は有効だったが、その構造には限界もあった。

### system / feature / implement / analyze は固定分類として重い

system、feature、implement、analyze という分類はわかりやすいが、ISE の抽象モデルとしては固定的すぎる。

たとえば analyze は system や feature と並ぶ階層なのか、それとも Reality を Knowledge に変換する別方向の作業なのかが曖昧になる。

また、implement は小規模開発の便利な運用形態ではあるが、ISE の本質的な分類というより、ある利用モードに近い。

この経験から、階層（layer）ではなく作業モード（mode）として捉える方が自然になった。

### SSoT と言うだけでは情報の種類を分けきれない

`docs/sdd/` を SSoT と呼ぶことは実務上わかりやすかった。

しかし、そこに含まれる情報は一種類ではない。

- 採用済み Intent
- 現実から復元した As-Is
- 人間由来のメモ
- 設計案
- 未確定事項
- 判断理由
- テスト観点
- 実装メモ

これらを同じ SSoT の中に置くと、何が正式な意図で、何が観測事実で、何が候補なのかが曖昧になる。

ISE では、すべての要素が独立して変化し続けるため静的な SSoT を前提とせず、「その時点で採用された Intent のスナップショット（Living Spec）」として扱う必要がある。

### Reverse SDD は As-Is と As-Intended を混同しやすい

コードから文書を逆生成すると、現在の構造はかなり整理できる。

しかし、そこから「本来こう意図されていた」とまで言うのは危険である。

コードから得られるのは主に As-Is である。As-Intended は、部分的に推測できるとしても不完全である。

この経験は、analyze モード（mode）における gather、mosaic、synthesize、elevate の分離につながる。

### 文書名と配置は操作性に直結する

goose-sdd では、`00_product_background.md` や `01_requirement.md` のように、順序付きのファイル名を使っていた。

これは人間には読みやすい。一方で、`mode / scope / artifact` のように対象を解決する場合、ファイル名や配置は単なる見た目ではなく、resolver が解釈する target surface になる。

このため、文書名を「人間に自然」かつ「AI と CLI が解決可能」な形に再設計する必要が出てきた。

## Nagi の位置づけ

Nagi は、goose-sdd の知見を引き継ぎつつ、ISE の reference implementation に近い位置を目指す実装である。

ただし、Nagi は ISE の単純な従属実装ではない。Nagi の開発を通じて見つかった実践上の問題が、ISE の理論側へ逆流することもある。

この関係は重要である。

ISE が理論で、Nagi が実装という一方向ではない。

Nagi は、ISE Document Engineering を鍛える実験場でもある。

## Nagi で見えてきたこと

### layer ではなく作業モード（mode）が自然

goose-sdd の system / feature / implement / analyze は、ある程度 layer 的に見える。

しかし実践を通じて、作業を上下関係や工程順序ではなく、**作業モード（mode）** として捉える方が自然になった（なお、ここでの `mode` は操作単位としての作業モードであり、[Lane Model](../lane-model/01_philosophy_and_concepts.ja.md) における Node 間の変換辺である Lane を束ねた入口に対応する）。

mode は、対象をどう変化させるかの方向を表す。

- spec
  - Intent を明示化する
- code
  - Reality を変更する
- analyze
  - Reality を Knowledge として再構成する

なお、健全性の評価や問題のトリアージ、差し戻し判断は **Gate** の責務として位置づけられ、修復作業自体は差し戻しを受けた `spec` や `code` モードの再同期として行われる。

この見方により、ドキュメントも mode ごとの artifact として整理できる。

### analyze モードは文書体系の中心に近い

analyze モードは、ISE Document Engineering にとって特に重要である。

analyze は単なるコード解析ではない。Reality から Knowledge を作り、必要に応じて PreSpec や Pre To-Be を生む mode である。

そのため、次の区別が必要になる。

- gather
  - code-backed facts を集める
- mosaic
  - 人間由来の断片情報や仮説を整理する
- synthesize
  - gather と mosaic を統合し、理解可能な As-Is にする
- elevate
  - より高い確度の Knowledge や To-Be 候補へ接続する

この構造は、Reverse Sync の危険を抑えるためにも必要である。

### Pre To-Be の受け皿が必要

Nagi の開発を通じて、As-Is と To-Be の間に Pre To-Be が必要であることが見えてきた。

Reality から改善候補や意図候補が見つかることはある。しかし、それはまだ正式な Intent ではない。

Pre To-Be は、この中間状態を受け止めるための領域である。

ここには、次のような情報が入る。

- pain point
- drift signal
- intent candidate
- design candidate
- confidence
- provenance
- unresolved question

将来的に IGE / ODF が成熟すれば、Pre To-Be は IGE へ渡す payload にもなりうる。

### consultant / assistant は直接の作業モードではないが文書と深く関係する

実装や運用の中では、相談、設計会話、レビュー補助、理解支援の機能が必要になる。

これは synchronization を直接前進させる作業モードではないため、spec、code、analyze と同列には置かない方がよい。

一方で、consultant / assistant は文書体系と強く関係する。

なぜなら、相談機能は適切な文書を読み、圧縮し、人間が判断しやすい形で提示する必要があるからである。

つまり、consultant / assistant は direct change mode ではないが、document access pattern を設計する上では重要な support interface である。

### artifact namespace と resolver が必要

ツールの操作面（target surface）としては、`mode / scope / artifact` を考える。

```text
<tool> <mode> <scope>[:artifact]
```

この形にすると、artifact は全 mode 共通の名前ではなく、mode ごとの名前空間を持つ方が自然である。

たとえば `design` という語があっても、spec モードの design と analyze モードの design-like reconstruction は同じ意味ではない。

そのため、artifact は mode ごとに解釈されるべきであり、必要に応じて namespace や resolver によって解決先を補助する必要がある。

これは ISE Document Engineering にとって重要である。

文書の分類は、単なるディレクトリ整理ではなく、操作可能な target surface の設計だからである。

## Nagi のドキュメントモデルへ戻すべき知見

goose-sdd と Nagi の経験から、Nagi のドキュメントモデルには次の知見を反映すべきである。

### 文書は同期責務で分類する

文書を「要求書」「設計書」「テスト仕様書」といった名前だけで分類すると、As-Is と To-Be、事実と判断、候補と採用済み Intent が混ざりやすい。

Nagi では、文書がどの同期責務を担うかを明確にする必要がある。

### 採用済み Intent と候補を分ける

AI が生成した提案や、Reality から推測された intent candidate は、そのまま正式な Intent ではない。

採用済み Intent に昇格するには、人間または責任主体の判断が必要である。

### Reality 由来の文書には provenance と confidence が必要

コードや設定から得た事実は比較的強い根拠を持つ。

一方、人間由来の断片情報や、AI による推測は確度が異なる。

Nagi の文書体系では、情報の出所と確度を表現できる必要がある。

### Decision を一級 artifact として扱う

判断理由、棄却案、トレードオフ、リスクの引受けは、後からコードだけを読んでも復元しにくい。

Nagi では decision を補助情報ではなく、一級の artifact として扱うべきである。

### ファイル名は人間と AI の両方に向けて設計する

ファイル名や配置は、人間の読みやすさだけで決めるべきではない。

AI が必要な文書を発見し、scope と artifact を解決し、古い情報と採用済み情報を区別できることが重要である。

## ISE Document Engineering へのフィードバックループ

ISE Document Engineering は、次のフィードバックループで育てるのが自然である。

1. 既存のドキュメント実践を整理する
2. ISE としての問題提起を立てる
3. Nagi で仮の文書構造を実装する
4. 実際の開発運用で壊れる点を観測する
5. goose-sdd / Nagi の実践知として記録する
6. ISE Document Engineering の理論へ戻す
7. Nagi の document model、lane-model、resolver へ再反映する

これは、ISE そのものの考え方と一致している。

理論から実装へ進む Forward Sync だけでなく、実装から理論へ戻る Reverse Sync が必要である。

## 今後まとめるべき論点

今後、ISE Document Engineering としてさらに整理すべき論点は次の通りである。

- artifact の標準分類
- mode ごとの document namespace
- scope と実ファイルパスの対応
- 採用済み Intent（Living Spec）への昇格ルール
- Pre To-Be の保存形式
- provenance / confidence の表現
- decision と ADR の関係
- human-readable と agent-readable の両立
- consultant / assistant が読むべき context set
- Drift を検出するための文書構造
- Nagi における resolver の設計
- プロジェクト固有拡張と標準 artifact の境界

これらは一度に決める必要はない。

Nagi の実装を進めながら、実際に必要になったものを ISE Document Engineering へ戻し、原則化していくのがよい。

## まとめ

goose-sdd は、Living Spec、Forward Sync、Reverse Sync、Negotiation Loop の有効性を示した。

同時に、system / feature / implement / analyze の固定分類や、SSoT という単純な見方には限界があることも見えた。

Nagi は、その知見を mode（作業モード）、target surface、artifact namespace、analyze モード、Pre To-Be へ再構成しようとしている。

ISE Document Engineering は、この実践知を吸い上げ、既存ドキュメント実践と ISE の同期モデルをつなぐ分野として位置づけられる。

この意味で、goose-sdd と Nagi は単なる実装ではない。

ISE Document Engineering を現実に接地させるための観測装置であり、理論を鍛えるための実験場である。
