---
title: .sflow 파일 형식
description: FloWorks 교환 표준의 기술 사양, 내부 구조 및 사용 가이드
---

# 📄 `.sflow` 파일 형식

`.sflow` 형식은 **FloWorks**의 기본 교환 및 지속성 표준입니다. 그래프 토폴로지, 노드 매개변수, 처리된 데이터 및 스티키 노트를 포함하는 단일 파일에 전체 워크플로를 패키징할 수 있어, 공유, 아카이빙 또는 결정론적 실험 재현을 용이하게 합니다.

---

## 📦 `.sflow` 파일이란?

`.sflow` 파일은 본질적으로 **이름이 변경된 ZIP 파일**입니다. 확장자를 `.zip`으로 변경하면 모든 파일 관리자 또는 명령줄 도구로 내용을 검사할 수 있습니다.

최소 내부 구조는 다음과 같습니다:

| 구성 요소 | 설명 |
|-----------|-------------|
| `diagram.json` | 기본 매니페스트: 노드, 연결, 뷰, 스티키 노트 및 직렬화 메타데이터를 정의합니다. |
| `data/` | 각 노드의 데이터가 `.npy` 형식(NumPy 이진 배열)으로 있는 폴더입니다. |
| `metadata.json` *(선택 사항)* | 보충 정보: 작성자, FloWorks 버전, 설명 및 태그. |

=== "🌳 시각적 구조"
    ```text
    mi-flujo.sflow
    ├── diagram.json
    ├── metadata.json
    └── data/
        ├── node_1.npy
        ├── node_2.npy
        └── script_state.npy (선택 사항, ScriptNode persist용)
    ```

---

## 🧩 `diagram.json` – 플로우의 핵심

이 JSON 파일은 전체 토폴로지, 캔버스의 요소 위치 및 저장 시점의 뷰 상태를 설명합니다.

### 최소 예제
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
    { "x": 600, "y": 100, "width": 200, "height": 150, "text": "Revisar umbral", "user_modified": true }
  ]
}
```

### 주요 필드
| 필드 | 유형 | 설명 |
|-------|------|-------------|
| `nodes` | `Array` | `{id, type, pos, params}` 객체 목록입니다. `type`은 `node_registry.py`와 일치해야 합니다. |
| `connections` | `Array` | `{from, to, from_port, to_port}` 연결 목록입니다. 포트는 인덱스가 아닌 문자열입니다. |
| `viewport` | `Object` | `(x, y, scale)`로 캔버스의 정확한 위치와 줌을 복원합니다. |
| `stickers` | `Array` | 96 dpi로 정규화된 좌표를 가진 직렬화된 스티키 노트입니다. |

!!! tip "노드 직렬화"
    각 노드의 특정 매개변수는 `SerializableMixin`을 통해 관리됩니다. `SERIALISABLE = [...]`에 선언된 속성만 저장됩니다. 시스템에 등록되지 않은 노드는 로드 중 자동으로 건너뜁니다.

---

## 💾 `data/` – 처리된 데이터 및 NumPy 배열

플로우가 실행되면 노드가 이 폴더 내의 `.npy` 파일에 결과를 저장할 수 있습니다.

- 파일 이름은 일반적으로 노드의 `id` 또는 내부 참조와 일치합니다.
- 배열은 NumPy 이진 형식으로 저장되며, **원래 차원을 엄격하게 보존**합니다(1D, 2D, 3D 등). 엔진은 절대 `flatten()`을 적용하지 않습니다.
- `diagram.json`에서 데이터는 `__npy__:` 접두사로 참조됩니다:
  ```json
  "params": { "cached_output": "__npy__:node_2.npy" }
  ```
- 노드가 데이터를 생성하지 않거나 지속하지 않도록 구성된 경우 해당 파일을 생략할 수 있습니다.

??? note "외부 호환성"
    `.npy` 파일은 Python 생태계에서 보편적입니다. FloWorks 외부에서 다음과 같이 읽을 수 있습니다:
    ```python
    import numpy as np
    datos = np.load("data/node_1.npy")
    print(datos.shape)
    ```

---

## 🏷️ `metadata.json` (선택 사항)

실행에 영향을 주지 않는 설명 정보를 포함하며, 추적성 및 프로젝트 관리에 이상적입니다:

```json
{
  "floworks_version": "2.1.0",
  "author": "María Gómez",
  "description": "Análisis de vibraciones en motor trifásico",
  "created": "2026-05-10T10:30:00Z",
  "tags": ["ingeniería", "vibraciones", "FFT", "multicanal"]
}
```

---

## 🔄 저장 및 로드 프로세스

FloWorks는 데이터 무결성을 보장하기 위해 강력한 메커니즘을 구현합니다:

1. **저장:**
   - 그래프를 순회하고 `SerializableMixin`을 통해 노드를 직렬화합니다.
   - 배열은 `data/`로 추출되고 JSON에서 `__npy__:` 접두사로 참조됩니다.
   - 모든 것이 `.sflow` 확장자를 가진 ZIP으로 패키징됩니다.
2. **안전한 로드:**
   - 현재 다이어그램의 **메모리 내 임시 백업**이 생성됩니다.
   - 새 `.sflow`를 추출하고 파싱합니다.
   - 오류가 발생하면(잘못된 JSON, 누락된 노드, `.npy` 손상) **백업이 자동으로 복원**되어 작업 손실이 없습니다.
3. **특수 ScriptNode:**
   - `script`, `params`, `dynamic_inputs`, `dynamic_outputs`, `persist` 및 `python_path`를 저장합니다.
   - 로드 시 코드를 재컴파일하고, 동적 포트를 재구성하며, `persist` 상태를 자동으로 복원합니다.
4. **StickyNotes 및 DPI:**
   - 저장 시 좌표와 크기가 **96 dpi**로 정규화됩니다.
   - 로드 시 현재 모니터의 DPI로 스케일되어 다양한 해상도 간 시각적 일관성을 보장합니다.

---

## 🛠️ 외부 사용 및 자동화

`.sflow` 형식은 투명하고 프로그래밍 가능하록 설계되었습니다. 외부 스크립트에서 읽거나 생성할 수 있습니다:

=== "🐍 Python (읽기)"
    ```python
    import zipfile
    import json
    import numpy as np

    with zipfile.ZipFile("mi-flujo.sflow") as z:
        graph = json.loads(z.read("diagram.json"))
        if "metadata.json" in z.namelist():
            meta = json.loads(z.read("metadata.json"))

        datos_n1 = np.load(z.open("data/node_1.npy"))
        print(f"Nodos: {len(graph['nodes'])}")
        print(f"Datos: {datos_n1.shape}")
    ```

=== "📤 Python (기본 생성)"
    ```python
    import zipfile
    import json
    import numpy as np

    graph = {
        "nodes": [{"id": "gen", "type": "GeneratorNode", "pos": [100, 100], "params": {}}],
        "connections": [],
        "viewport": {"x": 0, "y": 0, "scale": 1.0}
    }

    with zipfile.ZipFile("nuevo.sflow", "w", zipfile.ZIP_DEFLATED) as z:
        z.writestr("diagram.json", json.dumps(graph, indent=2))
        z.writestr("data/gen.npy", np.array([1.0, 2.0, 3.0]))
    ```

---

## 🔮 호환성 및 미래 확장성

`.sflow` 형식은 **확장 가능하고 하위 호환되는 디자인** 원칙을 따릅니다:

- ✅ **새 섹션:** 향후 버전에서 `thumbnails/`, `logs/` 또는 `plugins/`와 같은 폴더를 추가할 수 있으며 이전 로더를 손상시키지 않습니다.
- ✅ **선택적 필드:** 파서는 `diagram.json`의 알 수 없는 키를 무시하므로 실험적 메타데이터를 추가할 수 있습니다.
- ✅ **버전 관리:** `metadata.json`의 `floworks_version` 필드를 통해 형식이 발전할 경우 애플리케이션이 자동 마이그레이션을 적용할 수 있습니다.

!!! warning "황금률"
    애플리케이션이 열려 있는 동안에는 `diagram.json`을 수동으로 수정하지 마십시오. 시스템은 토폴로지, 배열 및 뷰 상태 간의 일관성에 의존합니다. 항상 네이티브 저장/로드 흐름을 사용하십시오.

---

## 📚 관련 리소스
- [🗺️ 코드 맵 및 아키텍처](architecture-ii.md) → `file_io.py` 및 `SerializableMixin`이 형식을 관리하는 방법.
- [📦 포터블 실행 파일 가이드](guia-ejecutable-portable.md) → 리소스를 위한 패키징 및 안전한 경로.
- [🧩 노드 참조](node-reference.md) → 노드 유형별 직렬화 계약.
