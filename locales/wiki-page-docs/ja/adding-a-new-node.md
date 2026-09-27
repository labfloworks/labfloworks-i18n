---
title: FloWorks に新しいノードを追加するガイド
description: FloWorks フローエンジンにカスタムノードを作成、登録、統合するためのステップバイステップチュートリアル。
---

# 📘 開発者ガイド：FloWorks に新しいノードを追加する方法

このガイドでは、FloWorks に新しいノードタイプを作成するための完全なプロセスを説明します。フローエンジン、ユーザーインターフェース、ビジュアルテーマ、および国際化システムと正しく統合されることを保証します。

---

## 📋 目次
- [📘 開発者ガイド：FloWorks に新しいノードを追加する方法](#-開発者ガイドfloworks-に新しいノードを追加する方法)
  - [📋 目次](#-目次)
  - [1. アーキテクチャの概要](#1-アーキテクチャの概要)
  - [2. `template_node.py` テンプレートの使用](#2-template_nodepy-テンプレートの使用)
  - [3. ステップバイステップ：カスタムノードの作成](#3-ステップバイステップカスタムノードの作成)
    - [3.1. テンプレートのコピーと名前変更](#31-テンプレートのコピーと名前変更)
    - [3.2. ポートとラベルの定義](#32-ポートとラベルの定義)
    - [3.3. 処理ロジックの実装](#33-処理ロジックの実装)
    - [3.4. 外観のカスタマイズ（オプション）](#34-外観のカスタマイズオプション)
    - [3.5. 設定可能なパラメータの追加（オプション）](#35-設定可能なパラメータの追加オプション)
    - [3.6. ノードのシリアライズ可能化（設定の保存 / 読み込み）](#36-ノードのシリアライズ可能化設定の保存--読み込み)
  - [4. システムへの統合](#4-システムへの統合)
  - [5. 国際化（i18n）](#5-国際化i18n)
  - [6. ビジュアルテーマ](#6-ビジュアルテーマ)
  - [7. チェックリストとトラブルシューティング](#7-チェックリストとトラブルシューティング)
    - [✅ チェックリスト](#-チェックリスト)
    - [🐛 よくある問題](#-よくある問題)
  - [8. まとめ](#8-まとめ)

---

## 1. アーキテクチャの概要

FloWorks は PySide6 上に構築されており、信号処理フローを表す接続可能なノードモデルを使用しています。

---

## 2. `template_node.py` テンプレートの使用

新しいノードの作成を容易にするため、`nodes/template_node.py` ファイルが提供されています。このテンプレートには以下が含まれます：
- 完全な国際化サポート（`languageChanged` への接続、`update_language` メソッド）。
- 完全なテーマサポート（`update_theme` メソッド）。
- 3セクション形式の HTML による組み込みヘルプ。
- 複数の設定可能な入出力ポートの管理。
- `get_output_for_port` による複数出力。
- `get_display_signal` によるプロットでの可視化。
- 翻訳可能なコンテキストメニュー。

新しいノードを開発する際は、常にこのテンプレートから開始することをお勧めします。

---

## 3. ステップバイステップ：カスタムノードの作成

### 3.1. テンプレートのコピーと名前変更
1. `nodes/template_node.py` を新しいノードの名でコピーします。例：`nodes/my_node.py`。
2. クラス名を `TemplateNode` から説明的な名前に変更します。例：`MyNodeNode`。
3. 必要に応じてインポートを調整します。

### 3.2. ポートとラベルの定義
!!! warning "重要：名前の一致"
    `PORTS`、`PORT_LABELS` のポート名、および `execute_program` が返す辞書のキーは、**完全に同じ**である必要があります（大文字/小文字を含む）。テンプレートには、より堅牢にするためのエイリアスマッピング（`'data_in'` → 最初の左ポート）が含まれるようになりました。

ファイルの先頭にある `PORTS` 辞書を編集します。各エントリの形式は次のとおりです：
```python
"port_name": ("side", fraction)
```
- **可能な辺：** `"left"`、`"right"`、`"top"`、`"bottom"`。
- **fraction：** 辺に沿った位置を示す `0.0` から `1.0` の間の値。

**1 つの入力と 2 つの出力を持つノードの例：**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
`PORT_LABELS` 辞書には、各ポートの横に表示されるテキストが含まれます。固定テキストではなく翻訳キーを使用することをお勧めします（国際化のセクションを参照）。

### 3.3. 処理ロジックの実装
重要なメソッドは `execute_program(self, input_data)` です。このメソッドは、ノードがデータを受信したときにフローエンジンによって呼び出されます。

**`input_data` は次の可能性があります：**
- 入力がない場合は `None`。
- 時間信号の場合はタプル `(x, y)`。
- 1 次元配列。
- 複数入力ノードの場合は辞書 `{port_name: data}`。

**戻り値：**
- 単一出力ノードの場合は、データを直接返します（例：タプル `(x, y)`）。
- 複数出力ノードの場合は、`PORTS` で定義された出力ポート名と一致するキーを持つ辞書を返します。

```python
def execute_program(self, input_data):
    # input_data を処理して結果を生成
    magnitude_result = (freq, mag)
    phase_result = (freq, phase)
    return {
        "magnitude": magnitude_result,
        "phase": phase_result
    }
```

!!! tip "汎用ポート名に関する注意"
    フローエンジンは、実際のポート名の代わりに `'data_in'` のようなキーを持つ辞書を渡す場合があります（特にユーザーが円を正確にクリックしなかった場合）。テンプレートにはすでにこのケースを処理するコードが含まれています：
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    これにより、不正確な接続エラーによってノードが失敗するのを防ぎます。

テンプレートにはすでにコメント付きの例が含まれています。また、`get_output_for_port(self, port_name)` も実装されているため、エンジンは各出力をルーティングできます：
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. 外観のカスタマイズ（オプション）
`paint()` メソッドは、背景、タイトル、状態、および追加のテキストを描画します。変更できるもの：
- 色（`update_theme` で自動的に更新されます）。
- 状態テキスト（`self._status` 属性を使用）。
- 概要情報（例：マグニチュードピーク）。

テンプレートには基本的な例が示されています。

### 3.5. 設定可能なパラメータの追加（オプション）
ノードにユーザーが調整できるパラメータが必要な場合（例：ウィンドウサイズ、カットオフ周波数）、以下を行えます：
1. `__init__` に属性を追加します（例：`self.window_size = 512`）。
2. 設定ダイアログを作成します（`QDialog` を継承）。
3. `open_config_dialog()` でダイアログを接続します（テンプレートに既に存在するメソッド）。
4. ダイアログからパラメータを更新し、`self.update()` を呼び出します。

### 3.6. ノードのシリアライズ可能化（設定の保存 / 読み込み）
ノードがコピー/ペースト、元に戻す/やり直し、またはファイルメニューの保存/開くコマンドを使用する際にパラメータを保存および回復できるようにするには、シリアライゼーションミックスインを継承し、その属性を宣言する必要があります。

1. ファイルにミックスインをインポートします：
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. クラスの継承を変更し、`QGraphicsObject` の前に含めます：
    ```python
    class MyNodeNode(SerializableMixin, QGraphicsObject):
    ```
3. クラスレベルで `SERIALISABLE` リストを定義します。永続化する属性名を含めます。単純な型（`int`、`float`、`str`、`bool`）、リスト、辞書、または NumPy 配列のみがサポートされます（後者は `.sflow` 内の `.npy` ファイルとして自動的に保存されます）。
    ```python
    class MyNodeNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frequency', 'amplitude', 'configuration']
    ```
4. これらの属性が `__init__` で初期化されていることを確認します：
    ```python
    self.frequency = 1000.0
    self.amplitude = 1.0
    self.configuration = {'type': 'sine', 'phase': 0}
    ```

これにより、`serialize`/`deserialize` メソッドを書く必要はありません。ミックスインが値の保存と回復を自動的に処理します。

ノードの読み込み時に追加のロジックが必要な場合（例えば、ハードウェア機器の再接続）は、まず親メソッドを呼び出して `deserialize` をオーバーライドできます：
```python
def deserialize(self, data):
    super().deserialize(data)   # SERIALISABLE 属性を復元
    self._start_device()
```

---

## 4. システムへの統合

ノードファイルを作成したら、`nodes` フォルダに配置するだけで、インターフェースに表示され、システムの他の部分と連携します。

---

## 5. 国際化（i18n）

すべての表示テキストは、`tr("key", default="...")` を介して翻訳可能である必要があります。テンプレートにはすでにこれが実装されています。`locales/` 内の JSON ファイルに対応するキーを追加する必要があります。

**推奨構造：**
```json
{
   "nodes": {
     "my_node": {
       "title": "My Node",
       "tooltip": "Tooltip description",
       "ports": {
         "input": "Input",
         "output1": "Output 1",
         "output2": "Output 2"
      },
       "status": {
         "no_data": "No data",
         "ready": "Ready"
      },
       "menu": {
         "show_output": "Show output",
         "configure": "Configure..."
      },
       "help_title": "Help - My Node",
       "help_html": "<h3>🎛️ Filter Node</h3>
<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_my_node": "My Node"
  }
}
```

HTML ヘルプは、すべてのノードで共の 3 セクション形式（特定の説明 + "システムの考え方" + "ショートカットとヒント"）に従います。テンプレートにはすでに `get_help_text()` にその構造が含まれています。

---

## 6. ビジュアルテーマ

`update_theme(self, theme)` メソッドは、現在のテーマで定義された色を含む辞書を受け取ります。テンプレートは自動的に以下を更新します：
- ノードの背景（`node_normal_bg`）
- 枠線（`node_selected_border`）
- タイトルとテキストの色（`node_normal_text`）
- ポートの色（`port_circle`、`port_outline`、`port_inline`、`port_text`）

`MainWindow`（または `ThemeUpdater`）で、テーマが変更されたときに各ノードに対して `node.update_theme()` が呼び出されることを確認してください。

---

## 7. チェックリストとトラブルシューティング

### ✅ チェックリスト
- [ ] ノードがツールバーから正しく作成されます。
- [ ] ポートが予想される位置に表示され、接続のために検出可能で（`Ctrl+クリック`）。
- [ ] 入力データを受信すると、`execute_program` が呼び出され、信号が処理されます。
- [ ] 出力が接続されたノードに正しく伝播します。
- [ ] コンテキストメニューで表示チャンネルを変更できます（複数出力がある場合）。
- [ ] ノードをクリックすると、選択された信号がプロットウィジェットにプロットされます。
- [ ] ダブルクリックで適切な形式のヘルプが開きます。
- [ ] 言語が正しく変更されます（タイトルテキスト、ポート、メニュー）。
- [ ] テーマが正しく変更されます（ノードとポートの色）。
- [ ] コピー/ペーストがエラーなく動作します。

!!! tip "正確なポート接続"
    ノードを接続する際は、対象のポートの円を正確にクリックしてください。ノードの本体をクリックすると、システムは汎用名（`'data_in'`）を使用します。テンプレートは現在これらの名前を許容しますが、複数出力の正しいルーティングを保証すために、円に直接接続するのが良い実践です。

### 🐛 よくある問題

| 症状 | 考えられる原因 | 解決策 |
|---------|---------------|----------|
| 接続矢印がポートにアンカーされない。 | ポートの円に `setData(0, port_name)` がない、または `get_port_scene_pos` が実装されていない。 | `_create_ports` で `circle.setData(0, port_name)` が実行されていること、および `get_port_scene_pos` がその名前を使用していることを確認してください。 |
| 出力が接続されたノードに届かない。 | `execute_program` が辞書を返さない（複数出力の場合）、または `get_output_for_port` が実装されていない。 | `execute_program` が `{port_name: data}` を返し、`get_output_for_port` が対応する値を返すことを確認してください。 |
| ノードをクリックしても何もプロットされない。 | `get_display_signal` が有効な `(x, y)` タプルを返さない、または `display_channel` が既存の出力と一致しない。 | `get_display_signal` が選択されたチャンネルを使用し、データが NumPy 配列であることを確認してください。 |
| 言語を変更してもテキストが更新されない。 | `languageChanged` 信号が接続されていない、または `update_language` が要素を更新しない。 | `__init__` での接続を確認してください：`language_manager.languageChanged.connect(self.update_language)`。 |
| テーマが適用されない。 | ノードの作成時またはテーマ変更時に `update_theme` が呼び出されない。 | `MainWindow` で、ノード作成に `node.update_theme(self.theme_manager.current_theme())` を呼び出してください。 |
| 矢印がノードの中心を指す。 | 円ではなく本体がクリックされた、または名前が `PORTS` と一致しない。 | 円を直接クリックしてください。`get_port_scene_pos` にエイリアスマッピングがあることを確認してください。 |
| インポート時に `NameError: name 'self' is not defined`。 | インスタンス属性が `__init__` の外部で宣言された。 | `self.my_parameter` などのすべての属性は `__init__` 内で定義する必要があります。 |
| コピー/`.sflow` を開くときにパラメータが失われる。 | ノードが `SerializableMixin` を継承していない、または `SERIALISABLE` を定義していない。 | このガイドの手順 3.6 を実装してください。 |

---

## 8. まとめ

このガイドに従い、`template_node.py` テンプレートを使用することで、効率的かつシステムの他の部分と一貫性を保ちながら、FloWorks に新しいノードを追加できます。常に i18n とテーマとの互換性を維持し、プロフェッショナルなユーザー体験を提供してください。

ぜひ、独自のノードで貢献してください！
