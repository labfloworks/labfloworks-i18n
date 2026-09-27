---
title: FloWorks에 새 노드 추가 가이드
description: FloWorks의 흐름 엔진, UI, 비주얼 테마 및 국제화 시스템에 사용자 정의 노드를 생성, 등록 및 통합하기 위한 단계별 튜토리얼입니다.
---

# 📘 개발자 가이드: FloWorks에 새 노드를 추가하는 방법

이 가이드는 FloWorks에서 새로운 유형의 노드를 생성하는 전체 프로세스를 설명하며, 흐름 엔진, 사용자 인터페이스, 비주얼 테마 및 국제화 시스템과 올바르게 통합되도록 보장합니다.

---

## 📋 목차
- [📘 개발자 가이드: FloWorks에 새 노드를 추가하는 방법](#-개발자-가이드-floworks에-새-노드를-추가하는-방법)
  - [📋 목차](#-목차)
  - [1. 아키텍처 소개](#1-아키텍처-소개)
  - [2. `template_node.py` 템플릿 사용](#2-template_nodepy-템플릿-사용)
  - [3. 단계별: 사용자 정의 노드 생성](#3-단계별-사용자-정의-노드-생성)
    - [3.1. 템플릿 복사 및 이름 변경](#31-템플릿-복사-및-이름-변경)
    - [3.2. 포트 및 레이블 정의](#32-포트-및-레이블-정의)
    - [3.3. 처리 로직 구현](#33-처리-로직-구현)
    - [3.4. 외관 사용자 정의 (선택 사항)](#34-외관-사용자-정의-선택-사항)
    - [3.5. 구성 가능한 매개변수 추가 (선택 사항)](#35-구성-가능한-매개변수-추가-선택-사항)
    - [3.6. 노드를 직렬화 가능하게 만들기 (설정 저장 / 로드)](#36-노드를-직렬화-가능하게-만들기-설정-저장--로드)
  - [4. 시스템 통합](#4-시스템-통합)
  - [5. 국제화 (i18n)](#5-국제화-i18n)
  - [6. 비주얼 테마](#6-비주얼-테마)
  - [7. 체크리스트 및 문제 해결](#7-크리스트-및-문제-해결)
    - [✅ 체크리스트](#-체크리스트)
    - [🐛 일반적인 문제](#-일반적인-문제)
  - [8. 결론](#8-결론)

---

## 1. 아키텍처 소개

FloWorks는 PySide6를 기반으로 구축되었으며, 신호 처리 흐름을 나타내는 연결 가능한 노드 모델을 사용합니다.

---

## 2. `template_node.py` 템플릿 사용

새 노드 생성을 용이하게 하기 위해 `nodes/template_node.py` 파일이 제공됩니다. 이 템플릿에는 다음이 포함됩니다:
- 국제화에 대한 완전한 지원 (`languageChanged` 연결, `update_language` 메서드).
- 테마에 대한 완전한 지원 (`update_theme` 메서드).
- 세 섹션 형식의 HTML 내장 도움말.
- 여러 구성 가능한 입력/출력 포트 관리.
- `get_output_for_port`를 통한 다중 출력.
- `get_display_signal`를 통한 플롯 시각화.
- 번역 가능한 컨텍스트 메뉴.

새 노드를 개발할 때는 항상 이 템플릿에서 시작하는 것이 좋습니다.

---

## 3. 단계별: 사용자 정의 노드 생성

### 3.1. 템플릿 복사 및 이름 변
1. `nodes/template_node.py`를 새 노드 이름으로 복사합니다(예: `nodes/mi_nodo.py`).
2. 클래스 이름을 `TemplateNode`에서 설명적인 이름으로 변경합니다(예: `MiNodoNode`).
3. 필요한 경우 가져오기를 조정합니다.

### 3.2. 포트 및 레이블 정의
!!! warning "중요: 이름 일치"
    `PORTS`, `PORT_LABELS`의 포트 이름과 `execute_program`이 반환하는 딕셔너리의 키는 **정확히 동일**해야 합니다(대소문자 포함). 이제 템플릿에는 더 은 견고성을 위해 별칭 매핑(`'data_in'` → 첫 번째 왼쪽 포트)이 포함되어 있습니다.

파일 상단의 `PORTS` 딕셔너리를 편집합니다. 각 항목의 형식은 다음과 같습니다:
```python
"포트_이름": ("측면", 비율)
```
- **가능한 측면:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **비율:** `0.0`에서 `1.0` 사이의 값으로, 측면을 따라 위치를 나타냅니다.

**하나의 입력과 두 개의 출력이 있는 노드의 예:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
`PORT_LABELS` 딕셔너리에는 각 포트 옆에 표시될 텍스트가 포함됩니다. 고정 텍스트 대신 번역 키를 사용하는 것이 좋습니다(국제화 섹션 참조).

### 3.3. 처리 로직 구현
핵심 메서드는 `execute_program(self, input_data)`입니다. 이 메서드는 노드가 데이터를 받을 때 흐름 엔진에 의해 호출됩니다.

**`input_data`는 다음일 수 있습니다:**
- 입력 없는 경우 `None`.
- 시간 신호의 경우 튜플 `(x, y)`.
- 1D 배열.
- 다중 입력 노드의 경우 딕셔너리 `{포트_이름: 데이터}`.

**반환 값:**
- 단일 출력 노드의 경우 데이터를 직접 반환합니다(예: 튜플 `(x, y)`).
- 다중 출력 노드의 경우 키가 `PORTS`에 정의된 출력 포트 이름과 일치하는 딕셔너리를 반환합니다.

```python
def execute_program(self, input_data):
    # input_data를 처리하고 결과를 생성합니다
    resultado_magnitud = (freq, mag)
    resultado_fase = (freq, phase)
    return {
        "magnitude": resultado_magnitud,
        "phase": resultado_fase
    }
```

!!! tip "일반 포트 이름에 대한 참고"
    흐름 엔진은 때때로 실제 포트 이름 대신 `'data_in'`과 같은 키가 있는 딕셔너리를 전달할 수 있습니다(특히 사용자가 원을 정확하게 클릭하지 않은 경우). 이제 템플릿에는 이 경우를 처리하기 위한 코드가 포함되어 있습니다:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    이렇게 하면 부정확한 연결로 인해 노드가 실패하는 것을 방지합니다.

템플릿에는 이미 주석 처리된 예제가 포함되어 있습니다. 또한 엔진이 각 출력을 라우팅할 수 있도록 `get_output_for_port(self, port_name)`을 구현합니다:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. 외관 사용자 정의 (선택 사항)
`paint()` 메서드는 배경, 제목, 상태 및 추가 텍스트를 그립니다. 다음을 수정할 수 있습니다:
- 색상(`update_theme`로 자동 업데이트됨).
- 상태 텍스트(`self._status` 속성 사용).
- 요약 정보(예: 진폭 피크).

템플릿은 기본 예제를 보여줍니다.

### 3.5. 구성 가능한 매개변수 추가 (선택 사항)
노드에 사용자가 조정할 수 있는 매개변수가 필요한 경우(예: 윈도우 크기, 차단 주파수) 다음을 수행할 수 있습니다:
1. `__init__`에 속성 추가(예: `self.window_size = 512`).
2. 구성 대화 상자 생성(`QDialog` 상속).
3. `open_config_dialog()`에 대화 상자 연결(템플릿에 이미 있는 메서드).
4. 대화 상자에서 매개변수를 업데이트하고 `self.update()`를 호출합니다.

### 3.6. 노드를 직렬화 가능하게 만들기 (설정 저장 / 로드)
노드가 복사/붙여넣기, 실행 취소/다시 실행 또는 파일 메뉴의 저장/열기 명령을 사용할 때 매개변수를 저장하고 복구할 수 있도록 하려면 직렬화 믹스인을 상속하고 속성을 선언해야 합니다.

1. 파일에 믹스인을 져옵니다:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. 클래스 상속을 변경하여 `QGraphicsObject` 앞에 포함합니다:
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
    ```
3. 클래스 수준에서 `SERIALISABLE` 목록을 지속하려는 속성 이름으로 정의합니다. 단순 유형(`int`, `float`, `str`, `bool`), 목록, 딕셔너리 또는 NumPy 배열만 허용됩니다(후자는 `.sflow` 내에 `.npy` 파일로 자동 저장됨).
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frecuencia', 'amplitud', 'configuracion']
    ```
4. 이러한 속성이 `__init__`에서 초기화되는지 확인합니다:
    ```python
    self.frecuencia = 1000.0
    self.amplitud = 1.0
    self.configuracion = {'tipo': 'seno', 'fase': 0}
    ```

이렇게 하면 `serialize`/`deserialize` 메서드를 작성할 필요가 없습니다. 믹스인이 값을 자동으로 저장하고 복구합니다.

로드 중에 추가 로직이 필요한 경우(예: 하드웨어 장치 다시 연결) 먼저 부모 메서를 호출하여 `deserialize`를 재정의할 수 있습니다:
```python
def deserialize(self, data):
    super().deserialize(data)   # SERIALISABLE의 속성을 복원합니다
    self._iniciar_dispositivo()
```

---

## 4. 시스템 통합

노드 파일을 만든 후에는 `nodes` 폴더에 붙여넣기만 하면 인터페이스에 나타나고 시스템의 나머지 부분과 함께 작동합니다.

---

## 5. 국제화 (i18n)

모든 표시 텍스트는 `tr("키", default="...")`를 통해 번역 가능해야 합니다. 템플릿은 이미 이를 구현합니다. 해당 키를 `locales/` 내의 JSON 파일에 추가해야 합니다.

**권장 구조:**
```json
{
   "nodes": {
     "mi_nodo": {
       "title": "Mi Nodo",
       "tooltip": "Descripción emergente",
       "ports": {
         "input": "Entrada",
         "output1": "Salida 1",
         "output2": "Salida 2"
      },
       "status": {
         "no_data": "Sin datos",
         "ready": "Listo"
      },
       "menu": {
         "show_output": "Mostrar salida",
         "configure": "Configurar..."
      },
       "help_title": "Ayuda - Mi Nodo",
       "help_html": "<h3>🎛️ Filter Node</h3>\n<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mi_nodo": "Mi Nodo"
  }
}
```

HTML 도움말은 모든 노드에 공통적인 세 섹션 형식을 따릅니다(특정 설명 + "시스템에서 생각하는 방법" + "단축키 및 팁"). 템플릿은 이미 `get_help_text()`에 구조를 포함하고 있습니다.

---

## 6. 비주얼 테마

`update_theme(self, theme)` 메서드는 현재 테마에 의해 정의된 색상이 포함된 딕셔너리를 받습니다. 템플릿은 자동으로 다음을 업데이트합니다:
- 노드 배경(`node_normal_bg`)
- 테두리(`node_selected_border`)
- 제목 및 텍스트 색상(`node_normal_text`)
- 포트 색상(`port_circle`, `port_outline`, `port_inline`, `port_text`)

테마가 변경될 때 `MainWindow`(또는 `ThemeUpdater`)에서 각 노드에 대해 `node.update_theme()`가 호출되는지 확인합니다.

---

## 7. 체크리스트 및 문제 해결

### ✅ 체크리스트
- [ ] 노드가 도구 모음에서 올바르게 생성됩니다.
- [ ] 포트가 예상 위치에 표시되고 연결을 위해 감지할 수 있습니다(`Ctrl+클릭`).
- [ ] 입력 데이터를 받으면 `execute_program`이 호출되고 신호가 처리됩니다.
- [ ] 출력이 연결된 노드에 올바르게 전파됩니다.
- [ ] 컨텍스트 메뉴에서 시각화 채널을 변경할 수 있습니다(다중 출력이 있는 경우).
- [ ] 노드를 클릭하면 선택한 신호가 플롯 위젯에 그려집니다.
- [ ] 두 번 클릭하면 적절한 형식으로 도움말이 열립니다.
- [ ] 언어가 올바르게 변경됩니다(제목, 포트, 메뉴 텍스트).
- [ ] 테마가 올바르게 변경됩니다(노드 및 포트 색상).
- [ ] 복사/붙여넣기가 오류 없이 작동합니다.

!!! tip "포트 정확하게 연결하기"
    노드를 연결할 때는 대상 포트의 원을 정확하게 클릭해야 합니다. 노드 본문을 클릭하면 시스템이 일반 이름(`'data_in'`)을 사용합니다. 이제 템플릿이 이러한 이름을 허용하지만, 다중 출력의 올바른 라우팅을 보장하기 위해 원에 직접 연결하는 것이 좋습니다.

### 🐛 일반적인 문제

| 증상 | 가능한 원인 | 해결 방법 |
|---------|---------------|---------|
| 연결 화살표가 포트에 고정되지 않습니다. | 포트 원에 `setData(0, port_name)`가 없거나 `get_port_scene_pos`가 구현되지 않았습다. | `_create_ports`에서 `circle.setData(0, port_name)`가 실행되고 `get_port_scene_pos`가 해당 이름을 사용하는지 확인합니다. |
| 출력이 연결된 노드에 도달하지 않습니다. | `execute_program`이 딕셔너리를 반환하지 않거나(다중 출력의 경우) `get_output_for_port`가 구현되지 않았습니다. | `execute_program`이 `{포트_이름: 데이터}`를 반환하고 `get_output_for_port`가 해당 값을 반환하는지 확인합니다. |
| 노드를 클릭해도 플롯에 아무것도 표시되지 않습니다. | `get_display_signal`이 유효한 튜플 `(x, y)`를 반환하지 않거나 `display_channel`이 기존 출력과 일치하지 않습니다. | `get_display_signal`이 선택한 채널을 사용하고 데이터가 NumPy 배열인지 확인합니다. |
| 언어를 변경해도 텍스트가 업데이트되지 않습니다. | `languageChanged` 신호가 연결되지 않았거나 `update_language`가 요소를 업데이트하지 않습니다. | `__init__`에서 연결을 확인합니다: `language_manager.languageChanged.connect(self.update_language)`. |
| 테마가 적용되지 않습니다. | 노드 생성 또는 테마 변경 시 `update_theme`가 호출되지 않습니다. | `MainWindow`에서 노드를 만든 후 `node.update_theme(self.theme_manager.current_theme())`를 호출합니다. |
| 화살표가 노드 중심을 가리킵니다. | 원 대신 본문을 클릭했거나 이름이 `PORTS`와 일치하지 않습니다. | 원을 직접 클릭합니다. `get_port_scene_pos`에 별칭 매핑이 있는지 확인합니다. |
| 가져오기 중 `NameError: name 'self' is not defined`. | 인스턴스 속성이 `__init__` 외부에서 선언되었습니다. | `self.mi_parametro`와 같은 모든 속성은 `__init__` 내에서 정의해야 합니다. |
| `.sflow`를 복사/열 때 매개변수가 손실됩니다. | 노드가 `SerializableMixin`을 상속하지 않거나 `SERIALISABLE`이 정의되지 않았습니다. | 이 가이드의 3.6단계를 구현합니다. |

---

## 8. 결론

이 가이드를 따르고 `template_node.py` 템플릿을 사용하면 효율적이고 시스템의 나머지 부분과 일관되게 FloWorks에 새 노드를 추가할 수 있습니다. 전문적인 사용자 경험을 위해 항상 i18n 및 테마 호환성을 지하는 것을 기억하세요.

자신만의 노드로 기여해 보세요!
