---
title: FloWorks
description: 信号処理、科学計測、自動化のための汎用ビジュアルラボラトリー。
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## 信号、計測、AI のめの汎用ビジュアルラボラトリー

科学処理 • DSP • VISA/SCPI • 自動化 • 機械学習

![FloWorks スクリーンショット](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[FloWorks 入門ガイド](getting-started.md){ .md-button }
[メインインターフェースの解剖](interface-anatomy.md){ .md-button .md-button--primary }
[理念](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## FloWorks とは
FloWorks は、**オープンソースのビジュアルラボラトリー**（Python + PySide6）です。コードを一行ずつ書くのではなく、ブロック（ノード）を接続してシステムを構築できます。

信号発生器、数学フィルタ、ハードウェアコントローラ（VISA/SCPI）、人工知能モデルを仮想ケーブルで接続するデジタルキャンバスを想像してください。すべては**データフロー**に基づいています。あるブロックの出力を別のブロックの入力に接続することで、情報を処したり、機器を自動化したり、結果をリアルタイムで分析したりできます。

学生、研究者、エンジニア、そして従来のプログラミングの壁なしに、直感的に複雑なシステムを実験、学習、プロトタイピングしたいすべての人を対象としています。

### ミッション
実験ワークフローを単一のビジュアルでオープンかつアクセスしやすいツールに集約すること。ユーザーが*実験と発見*に集中できよう、ソフトウェアの複雑さやライセンスコストと戦う必要のない環境を目指します。

### ビジョン
実験的なアイデアとその実行の間にある唯一の障壁が、実験者の好奇心である世界。FloWorks は、グローバルコミュニティによって、グローバルコミュニティのために構築される科学および工学のリファレンスプラットフォームを目指し、プロプライエタリツールの壁を取り払います。

### 原則
* **完全な自由（MIT ライセンス）：** 知識とツールは、すべての人に対して自由かつアクセス可能であるべきです。
* **無限の拡張性：** 必要なブロックがない場合、誰でも Python を使って作成し、エコシステムに統合できます。
* **ビジュアル透明性：** プロセスの各ステップは、グラフィカルに検査、デバッグ、理解できます。
* **リアルワールドとの接続：** これは単なるシミュレーションではありません。キャンバスから直接、実際の科学計測機器を制御できます。

クローズドまたは高度に専門化されたツールとは異なり、FloWorks は、各コンポーネントが再利用可能かつ接続可能なノードである、拡張可能なモジュール式エコシステムとして設計されています。

---

## 主要機能

<div class="grid cards" markdown>

-   **:material-puzzle-outline: 拡張可能なノードエコシステム**

    レイヤー別に整理された技術カタログ：ソース、処理、制御、ハードウェア、スクリプト。

    動的レジストリ、宣言的シリアライゼーション、および迅速な開発のための明確な契約。

    [:material-arrow-right: ノードリファレンス](node-reference.md)

-   **:material-connection: VISA/SCPI 統合**

    オシロスコープ、LCR メーター、ジェネレーターとの直接接続。

    マルチチャンネル対応、`PyVISA-py` による統合シミュレーション、ポータブルモードでのファイアウォール管理。

    [:material-arrow-right: 計測機器](instrumentation.md)

-   **:material-package-variant-closed: ポータブル `.sflow` フォーマット**

    JSON グラフ、`.npy` 配列、メタデータを含む自己完結型 ZIP 標準。

    実験の完全な再現性と自動 DPI 正規化。

    [:material-arrow-right: .sflow フォーマット](sflow-format.md)

-   **:material-translate: 高度な国際化**

    アプリを再起動せずにホット言語切り替え。

    階層的 JSON 翻訳と設定の永続化。

    [:material-arrow-right: i18n ガイド](translation-guide.md)

-   **:material-tools: SDK と迅速な開発**

    ベーステンプレート（`template_node.py`）、シリアライゼーションミックスイン、およびステップバイステップガイド。

    プラグインとコミュニティ拡張に対応したアーキテクチャ。

    [:material-arrow-right: ノードを作成](adding-a-new-node.md)

</div>

---

## 応用分野

| 分野 | 応用 |
|------|------|
| 🎓 **教育** | 物理学、電子工学、数学、STEM ラボ |
| ⚙️ **エンジニアリング** | DSP、制御、計測、計量 |
| 🤖 **AI** | ML、最適化、ハイブリッドパイプライン |
| 🔬 **研究** | 自動化とデータ取得 |
| 🔌 **ハードウェア** | VISA/SCPI、シミュレーション、ハイブリッドシステム |

---

!!! tip "FloWorks を初めてお使いですか？"

    **FloWorks 入門ガイド** のセクションから始め、次に **インターフェースの解剖** で GUI アーキテクチャを理解し、最後に **全般アーキテクチャ** を探索してデータフローとトロジカルエンジンの構造を理解してください。

---

!!! info "Open Core モデル"

    FloWorks は **MIT ライセンス** の下で **Free/Open Core** モデルを採用しています。

    コアはフリーでオープンなままですが、将来のエンタープライズ、カリキュラム、またはマーケットプレイス拡張はオプションとなります。

---

<div markdown="1" style="text-align: center;">

## FloWorks

ビジュアル処理 • 計測 • 科学 • AI

<small>MkDocs Material で構築されたドキュメント</small>

</div>
