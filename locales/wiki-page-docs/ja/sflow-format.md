---
title: .sflow ファイル形式
description: FloWorks 交換標準の技術仕様、内部構造、および使用ガイド
---

# 📄 `.sflow` ファイル形式

`.sflow` 形式は **FloWorks** のネイティブな交換および永続化標準です。グラフのトポロジー、ノードパラメータ、処理済みデータ、付箋を含む完全なワークフローを単一ファイルにパッケージ化でき、実験の共有、アーカイブ、または決定論的な再現を容易にします。

---

## 📦 `.sflow` ファイルとは？

`.sflow` ファイルは、本質的には**名前を変更した ZIP ファイル**です。拡張子を `.zip` に変更することで、任意のファイルマネージャーやコマンドラインツールでその内容を確認できます。

その最小限の内部構造は以下で構成されます：

| コンポーネント | 説明 |
|-----------|-------------|
| `diagram.json` | メインマニフェスト：ノード、接続、ビュー、付箋、およびシリアライゼーションメタデータを定義します。 |
| `data/` | 各ノードのデータを `.npy` 形式（NumPy バイナリ配列）で格納するフォルダーです。 |
| `metadata.json` *(オプション)* | 補足情報：作者、FloWorks バージョン、説明、およびタグ。 |

=== "🌳 ビジュアル構造"
    ```text
    my-flow.sflow
    ├─ diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (オプション、ScriptNode の永続化用)
    ```

---

## 🧩 `diagram.json` – フローの中心

この JSON ファイルは、完全なトポロジー、キャンバス上の要素の位置、および保存時のビュー状態を記述します。

### 最小限の例
```json
{
  "nodes": [
    {
      "id": "n1",
      "type": "OscilloscopeNode",
      "pos": [150, 200],
      "params": { "channel": "primary", "simulation": false }
    },
    {
      "id": "n2",
      "type": "GraphExporterNode",
      "pos": [450, 200],
      "params": { "theme": "dark", "export_format": "png" }
    }
  ],
  "connections": [
    {
      "from": "n1",
      "to": "n2",
      "from_port": "out",
      "to_port": "data_in"
    }
  ],
  "viewport": { "x": 0, "y": 0, "scale": 1.0 },
  "stickers": [
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "閾値を確認", "user_modified": true }
  ]
}
```

### 主要フィールド
| フィールド | 型 | 説明 |
|-------|------|-------------|
| `nodes` | `Array` | `{id, type, pos, params}` オブジェクトのリスト。`type` は `node_registry.py` と一致する必要があります。 |
| `connections` | `Array` | `{from, to, from_port, to_port}` 接続のリスト。ポートはインデックスではなく文字列です。 |
| `viewport` | `Object` | `(x, y, scale)`。キャンバスの正確な位置とズームを復元します。 |
| `stickers` | `Array` | 座標が 96 dpi に正規化されたシリアライズ済み付箋。 |

!!! tip "ノードのシリアライゼーション"
    各ノードの固有パラメータは `SerializableMixin` 経由で管理されます。`SERIALISABLE = [...]` で宣言された属性のみが保存されます。システムに登録されていないノードは、ロード中に自動的にスキップされます。

---

## 💾 `data/` – 処理済みデータおよび NumPy 配列

フローが実行されると、ノードはこのフォルダー内の `.npy` ファイルに結果を保存できます。

- ファイル名は通常、ノードの `id` または内部参照と一致します。
- 配列は NumPy バイナリ形式で保存され、**元の次元性（1D、2D、3D など）を厳密に保持**します。エンジンは決して `flatten()` を適用しません。
- `diagram.json` では、`__npy__:` プレフィックスを使用してデータを参照します：
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- ノードがデータを生成しないか、永続化しないように構成されている場合、対応するファイルは省略できます。

??? note "外部互換性"
    `.npy` ファイルは Python エコシステムで普遍的です。FloWorks 外部では以下のように読み取れます：
    ```python
    import numpy as np
    data = np.load("data/node_1.npy")
    print(data.shape)
    ```

---

## 🏷️ `metadata.json`（オプション）

実行に影響を与えない記述情報を含み、トレーサビリティとプロジェクト管理に最適です：

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "三相モーターの振動解析",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["engineering", "vibrations", "FFT", "multichannel"]
}
```

---

## 🔄 保存および読込みプロセス

FloWorks は、データ整合性を保証するための堅牢なメカニズムを実装しています：

1. **保存：**
   - グラフを走査し、`SerializableMixin` を介してノードをシリアライズします。
   - 配列を `data/` に抽出し、`__npy__:` を使用して JSON で参照します。
   - すべてを `.sflow` 拡張子の ZIP にパッケージ化します。
2. **安全な読み込み：**
   - 現在のダイアグラムの**一時的なメモリ内バックアップ**を作成します。
   - 新しい `.sflow` を抽出して解析します。
   - エラーが発生した場合（無効な JSON、欠落ノード、破損した `.npy`）、**バックアップが自動的に復元され**、作業が失われることはありません。
3. **特別な ScriptNode：**
   - `script`、`params`、`dynamic_inputs`、`dynamic_outputs`、`persist`、`python_path` を保存します。
   - 読み込み時にコードを再コンパイルし、動的ポートを再構築し、`persist` 状態を自動的に復元します。
4. **StickyNotes と DPI：**
   - 保存時に座標とサイズが **96 dpi** に正規化されます。
   - 読み込み時に現在のモニターの DPI にスケーリングされ、異なる解像度間での視覚的一貫性が保証されます。

---

## 🛠️ 外部使用と自動化

`.sflow` 形式は、透明でプログラム可能になるように設計されています。外部スクリプトから読み取りまたは生成できます：

=== "🐍 Python（読み取り）"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("my-flow.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        data_n1 = np.load(z.open("data/node_1.npy"))
        print(f"ノード数: {len(graph['nodes'])}")
        print(f"データ: {data_n1.shape}")
    ```

=== "📤 Python（基本作成）"
    ```python
    import zipfile
    import json
    import numpy as np

    graph = {
        "nodes": [{"id": "gen", "type": "GeneratorNode", "pos": [100, 100], "params": {}}],
        "connections": [],
        "viewport": {"x": 0, "y": 0, "scale": 1.0}
    }

    with zipfile.ZipFile("new.sflow", "w", zipfile.ZIP_DEFLATED) as z:
        z.writestr("diagram.json", json.dumps(graph, indent=2))
        z.writestr("data/gen.npy", np.array([1.0, 2.0, 3.0]))
    ```

---

## 🔮 互換性と将来の拡張性

`.sflow` 形式は、**拡張可能かつ後方互換な設計**の原則に従います：

- ✅ **新しいセクション：** 将来のバージョンでは、`thumbnails/`、`logs/`、または `plugins/` などのフォルダーを追加しても、古いローダーを破壊しません。
- ✅ **オプションフィールド：** パーサーは `diagram.json` 内の不明なキーを無視するため、実験的なメタデータを追加できます。
- ✅ **バージョン管理：** `metadata.json` 内の `floworks_version` フィールドにより、形式が進化した場合にアプリケーションが自動移行を適用できま。

!!! warning "黄金律"
    アプリケーションが開いている間は、手動で `diagram.json` を変更しないでください。システムはトポロジー、配列、およびビュー状態の間の一貫性に依存しています。常にネイティブの保存/読み込みフローを使用してください。

---

## 📚 関連リソース
- [🗺️ コードマップとアーキテクチャ](architecture-ii.md) → `file_io.py` と `SerializableMixin` が形式を管理する方法。
- [📦 ポタブルビルドガイド](guia-ejecutable-portable.md) → リソースのパッケージングと安全なパス。
- [🧩 ノードリファレンス](node-reference.md) → ノードタイプごとのシリアライゼーション契約。
