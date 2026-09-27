# 🧪 FloWorks コンソール インタラクティブ チュートリアル

FloWorks 実験室へようこそ。このセクションは、より高度なユーザー向けです。これは、キャンバスに関連するすべてのもの、つまりノードとその接続を、逐次的かつ1行ずつ制御するための **Python** ターミナルです。プログラムにリンクされたターミナルであり、より高度なユーザー向けに動作やルーチンを決定できます。

このガイドでは、マウスに触れずにフローダイアグラムを制御および分析する方法を**ステップバイステップ**で説明します。すべての例はインタラクティブコンソールで検証されており、プログラムの実際のデータ構造を反映しています。

---

## 1. 地形知る

コンソールは3つのグローバルオブジェクトを注入します：`app`（メインウィンドウ）、`graph`（シーン/ダイアグラム）、`selected_node`（キャンバス上で現在選択されているノード）。すべてのコマンドはこれら3つから始まります。

### すべてのノードを表示

```python
>>> graph.nodes
```

**出力例：**
```
シーン内のノード：
  [0] 高度信号生成器 (タイプ: SignalSourceNode, カテゴリ: Sources)
  [1] FFT (タイプ: FFTNode, カテゴリ: Processing)
```

括弧内のインデックス（`[0]`、`[1]`）は、ノードにアクセスするための主要な方法です。順序はキャンバス上での作成順です。

#### 代替：ノードを数える、またはタイプでフィルタリング

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### すべての接続を表示

```python
>>> graph.connections
```

**出力例：**
```
シーン内の接続：
  [0] 高度信生成器 (out) → FFT (input)
```

出力には、ソースノード名、出力ポート、矢印、宛先ノード、および入力ポートが表示されます。接続が表示されない場合、フローは実行できません。

#### 代替：1つのノードの接続を表示

```python
>>> selected_node.connectors
```

### 選択されたノードを表示

キャンバス上のノードをクリックしてから、以下を実行します：

```python
>>> selected_node
```

**出力例：**
```
ノード: FFT
  タイプ: FFTNode
  カテゴリ: Processing
  ポート: ['input', 'output', 'magnitude', 'phase']
```

> **💡 注：** ノードが選択されていない場合、`selected_node` は `None` です。ノードを選択すると、サイドパラメータテーブルも自動的に更新されます。

#### 代替：コードでノードを選択

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. マウスを使わずにノードと接続を操作

### 新しいノードを作成

ノードの正確なクラス名（カタログと同じ）を知っている必要があります。引数は `(type, x, y)` です。

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

ノードはキャンバス上の座標 (300, 200) に表示されます。正確な名前がわからない場合は、カテゴリをリストします（セクション7を参照）。

#### 代替：一度に複数のノードを作成

```python
>>> for i, tipo in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(tipo, 100 + i*200, 300)
```

### ノードを手動で接続

構文：`graph.connect_nodes(source, destination, 'output_port', 'input_port')`。ポートは各ノードによって異なります。名前を推測しないでください。

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 注：** 接続する前に必ず `graph.nodes[N].PORTS` を確認してください。FFT ノードには `'input'` と `'magnitude'` があります。ジェネレータには `'output'` があります。

#### 代替：デフォルトポートに接続

正確な入力ポート名がわからない場合、一部のノードは `None` を受け入れ、最初に利用可能なポートを使用します：

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### ノードを削除

```python
>>> graph.remove_node(graph.nodes[2])
```

ノードとそのすべての関連接続を削除します。`graph.nodes` のインデックスは並べ替えられるため、古い参照は保持しないでください。

#### 代替：カテゴリ内のすべてのノードを削除

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### ノードのポートを表示

```python
>>> graph.nodes[1].PORTS
```

**出力例：**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

キーはポート名（文字列）です。値は視覚的位置を持つタプルです。接続にはキーのみが関係します。

#### 代替：ポートを単純なリストとして表示

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. フローを実行して結果を表示

### グラフ全体を実行

```python
>>> graph.execute_flow()
```

このメソッドはダイアグラム（`graph`）に属し、メインウィンドウには属しません。すべてのノードをトポロジカル順序で走査し、各ノードを実行して結果をキャッシュします。何も返しません。データは内部的に保存されます。

#### 代替：特定のブランチの計算を強制

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

これは、指定されたノードの上流ツリー全体を再計算し、結果を直接返します。グローバルキャッシュは変更しません

### ノードのキャッシュデータを表示

特定のノードの処理済みデータにアクセスする必要がある場合、2つの方法があります：

#### 直接方式（オブジェクト別）
```python
>>> graph.node_values[graph.nodes[1]]
```

**出力例：**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ 警告：** シーン内のノードの順序が変更された場合（例：ノードの削除や追加）、またはオブジェクトインスタンスが書に保存されたキーと完全に一致しない場合、この方法は `KeyError` で失敗する可能性があります。

#### 代替方式（位置別）
```python
>>> list(graph.node_values.values())[1]
```

この方法は、オブジェクトの正確なアイデンティティに依存しないため、**より安定しています**。値の順序は、最後の `graph.execute_flow()` 中にノードが実行されたシーケンスに従います。インデックス `[1]` は、そのシーケンスの2番目のノードに対応します。

> **💡 注：** 実行順序での各ノードのインデックスを確認したい場合は、以下を使用できます：
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ 注意：** `graph.node_values` は、必ずしも直接配列を返すわけではありません。プロセッサノード（FFT、フィルターなど）の場合、各キーが出力ポートである**辞書**を返します。ソースノードの場合、タプル `(x, y)` を返します。

#### 代替：1行ですべてのノードのデータを表示
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### ソースノードの Y 軸にアクセス

ジェネレータノード（SignalSourceNode、FileInputNode など）は、実行時にタプル `(time, signal)` を返します。Y 軸のみを取得するには：

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**出力例：**
```
Scalar NumPy (float64): 1.0
```

#### 代替：X 軸（時間）を取得

```python
>>> x = result[0]
>>> x[:5]
```

### Python の代入：極めて重要な詳細

Python では、代入（`=`）は**文**であり、式ではありません。コンソールは `x, y = ...` の後に何も印刷しません。なぜなら、戻り値がないからです。

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

機能したことを確認するには、次の行で変数を評価します：

```python
>>> x
>>> y.shape
```

または、`;` を使用して同じ行に式を連鎖させます：

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

または、明示的に `print()` を使用します：

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. メインプロットに描画

### プロットをクリア

```python
>>> app.plot_widget.clear_plot()
```

#### 代替：クリアしてすぐに再描画

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### コンソールから任意の信号を描画

NumPy で配列を作成し、ノードを経由せずにプロッティングウィジェットに直接送信できます。

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### 代替：正弦波の和を描画

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### ソースノードの結果を描画

ソースノードは `(x, y)` を返すため、直接アンパックできます：

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### FFT ノードの結果を描画（複数ポート）

複数の出力を持つノード（FFT、時周数分析など）は、単純なタプルを返しません。各キーが出力ポートである `dict` を返します。

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**出力例：**
```
dict_keys(['output', 'magnitude', 'phase'])
```

ノードに汎用ポートがない場合、`'output'` は `None` になる可能性があることに注意してください。有用な出力は `'magnitude'` と `'phase'` で、これらはタプル `(frequencies, values)` です：

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### 代替：振幅の代わりに位相を描画

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### 代替：2つの信号を重ね合わせる

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # 別のノード
>>> app.plot_widget.plot_waveform(x2, y2)  # 重ね合わせ
```

> **💡 注：** dict で `x, y = result` を試みると、Python は `ValueError: too many values to unpack` を発生させます。アンパックする前に、必ず `type(result)` と `result.keys()` で確認してください。

---

## 5. その場でアプリケーションを変更

### サイドパラメータテーブルを更新

コードでパラメータを変更し、サイドテーブルにその変更を反映させたい場合：

```python
>>> app.workspace_table.populate()
```

このメソッドは引数を受け取りません。選択されたノードの現在の値でテーブルを更新します。

#### 代替：別のノードの選択を強制し、更新

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### ツールバーからノードを追加

```python
>>> app.add_node('SignalSourceNode')
```

これは、ツールバーの "+" ボタンを押すのと同等で。ノードはキャンバス上のデフォルト位置に配置されます。

### ウィンドウタイトルを変更

`setWindowTitle` は Qt のネイティブメソッドです。機能しますが、アプリケーションに `update_title()` を呼び出して自動的に上書きするタイマーまたはイベントがある可能性があることに注意してください。

```python
>>> app.setWindowTitle('私の信号ラボ')
>>> app.windowTitle()
```

アプリケーションが内部状態（プロジェクト名、ファイルなど）から計算する「公式」タイトルを復元するには：

```python
>>> app.update_title()
```

#### 代替：プロジェクト名を含むタイトル

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. コンソールでのナビゲーションと生産性

コンソールは単なる `print()` ではありません。履歴、自動補完、複数行ブロックがあります。

| キー / コマンド           | 操作                                                           |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | 実行されたコマンド履歴を移動                                   |
| `Tab`                     | 名前空間内の変数、属性、メソッドを自動補完                     |
| `Ctrl + L`                | コンソール全体をクリア（テキストを削除、Python 状態は保持）   |
| `if`, `for`, `def`, `class` | プロンプトが `>>>` から `...` に変わり、複数行ブロックに対応  |
| `Ctrl+C`（選択時）        | コンソールのテキストをコピー                                   |
| `Ctrl+A`                  | すべての内容を選択                                             |

> **💡 注：** 自動補完は `rlcompleter` を使用し、注入された名前空間全体（`app`、`graph`、`selected_node`）およびセッションで定義された変数を認識します。

---

## 7. 高度なレシピ

### ノードの内部パラメータを変更

ノードパラメータはフラットな属性ではありません。`params` 辞書内にネストされており、`'preset'`、`'formula'`、`'advanced'` などのサブセクションがあります。`node.amplitude = 3.0` は決して行わないでください。これはオブジェクトに新しい属性を作成しますが、実際のパラメータは変更しません。

#### ケース A：プリセットを変更（正弦波、方形波など）

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### ケース B：カスタム数式を使用

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

式では `t` を時間変数として使用します。`vars` の値は、数式で参照できるシンボルです。`vars` を省略すると、ノードはデフォルト値を使用し、数式は変更を反映しない可能性があります。

#### ケース C：高度なパラメータを変更（サンプリングレート、期間）

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### ケース D：非ジェネレータノードのパラメータを変更（例：FFT）

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 注：** `getattr(obj, '_generate_signal', lambda: None)()` は安全なパターンです：メソッドが存在する場合（ジェネレータノード）は呼び出し、存在しない場合は何もせず、エラーも発生しません。プロセッサノードの場合、`graph.execute_flow()` だけで十分です。

### 利用可能なすべてのノードカテゴリを一覧表示

インポートはカタログを読み込みますが、自動的には表示しません。Python では、成功したインポートは何も印刷しないことを覚えておいてください。オブジェクトを評価する必要があります。

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

読みやすいサマリーを表示するには：

```python
>>> for cat, nodes in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(nodes)} ノード")
```

#### 代替：カテゴリ別にノード名を一覧表示

```python
>>> {cat: [n.__name__ for n in nodes] for cat, nodes in NODE_CATEGORIES.items()}
```

### 任意のメソッドのヘルプを表示

```python
>>> help(graph.connect_nodes)
```

ドックストリンがコンソールに直接表示されます。これは、ソースコードを開かずにメソッドが期待する引数を発見するのに役立ちます。

#### 代替：フィルタリングされた属性を表示

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. 問題が発生した場合はどうすればよいですか？

- **コンソールの赤いエラー：** 完全なトレースバックが表示されます。アプリケーションは閉じません。コマンドを修正して再試行できます。
- **インターフェースがフリーズする：** 無限ループを記述した可能性があります。コンソールは別のスレッドで実行されますが、ループが GUI スレッドに影響する場合は、アプリケーションを再起動してください。
- **予期しない `None`：** ノードがデータの代わりに `None` を返す場合は、上流に接続されていること（`graph.connections`）、およびフローが実行されたこと（`graph.execute_flow()`）を確認してください。
- **`ValueError: too many values to unpack`：** dict をタプルであるかのようにアンパックしようとしています。最初に `result.keys()` を使用してください。
- **`ValueError: not enough values to unpack`：** 2つの値を期待していますが、ノードは1つ（dict）または3つ（スペクトログラム）を返します。アンパックする前に `type(result)` で確認してください。
- **`AttributeError`：** オブジェクトにその属性がありません。`dir(obj)` または `[a for a in dir(obj) if 'word' in a.lower()]` を使用して、正しい名前を見つけてください。
- **実行しても何も起こらない：** チェーンに少なくとも1つのソースノードが接続されており、`graph.execute_flow()` が呼び出されたことを確認してください。プロセッサノードは単独ではデータを生成しません。
- **プロットが変わらない：** パラメータを変更した後に `graph.execute_flow()` を呼び出したことを確認してください。`params` を変更するけでは、自動的には再計算されません。

---

© 2026 FloWorks — 信号ラボ
