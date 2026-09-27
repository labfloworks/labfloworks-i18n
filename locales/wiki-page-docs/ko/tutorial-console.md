# 🧪 FloWorks 콘솔 인터랙티브 튜토리얼

FloWorks 실험실에 오신 것을 환영합니다. 이것은 더 고급 사용자를 위한 섹션입니다. 캔버스와 관련된 모든 것, 즉 노드와 연결을 제어하기 위한 **Python** 터미널입니다. 코드 줄을 통해 순차적으로 제어하며, 프로그램에 연결된 이 터미널은 프로그램을 제어하고 더 까다로운 사용자의 편의를 위해 동작이나 루틴을 결정할 수 있습니다.

이 가이드는 마우스를 만지지 않고 플로우 차트를 제어하고 분석하는 방법을 **단계별**로 보여줍니다. 각 예제는 인터랙티브 콘솔에서 검증되었으며 프로그램의 실제 데이터 구조를 반영합니다.

---

## 1. 지형 파악

콘솔은 세 가지 전역 객체를 주입합니다: `app`(메인 창), `graph`(장면/다이어그램) 및 `selected_node`(캔버스에서 현재 선택된 노드)입니다. 모든 명령은 이 세 가지에서 시작합니다.

### 든 노드 보기

```python
>>> graph.nodes
```

**예시 출력:**
```
장면의 노드:
  [0] 고급 신호 생성기 (유형: SignalSourceNode, 범주: Sources)
  [1] FFT (유형: FFTNode, 범주: Processing)
```

대괄호(`[0]`, `[1]`) 사이의 인덱스는 노드에 액세스하는 주요 방법입니다. 순서는 캔버스에서 생성된 순서입니다.

#### 대안: 노드 수 계산 또는 유형별 필터링

```python
>>> len(graph.nodes)
>>> [n for n in graph.nodes if 'FFT' in type(n).__name__]
```

### 모든 연결 보기

```python
>>> graph.connections
```

**예시 출력:**
```
장면의 연결:
  [0] 고급 신호 생성기 (out) → FFT (input)
```

출력은 소스 노드 이름, 출력 포트, 화살표, 대상 노드 및 입력 포트를 보여줍니다. 연결이 표시되지 않으면 흐름을 실행할 수 없습니다.

#### 대안: 단일 노드의 연결 보기

```python
>>> selected_node.connectors
```

### 선택한 노드 보기

캔버스에서 노드를 클릭한 다음 실행합니다:

```python
>>> selected_node
```

**예시 출력:**
```
노드: FFT
  유형: FFTNode
  범주: Processing
  포트: ['input', 'output', 'magnitude', 'phase']
```

> **💡 참고:** 선택한 노드가 없으면 `selected_node`는 `None`입니다. 노드를 선택하면 측면 매개변수 테이블도 자동으로 업데이트됩니다.

#### 대안: 코드로 노드 선택

```python
>>> graph.nodes[0].setSelected(True)
>>> app.console.update_namespace(selected_node=graph.nodes[0])
```

---

## 2. 마우스 없이 노드 및 연결 조작

### 새 노드 만들기

노드 클래스의 정확한 이름을 알아야 합니다(카탈로그와 동일). 인수는 `(유형, x, y)`입니다.

```python
>>> graph.add_catalog_node('SumNode', 300, 200)
```

노드가 좌표 (300, 200)의 캔버스에 나타납니다. 정확한 이름을 모르는 경우 범주를 나열하세요(7절 참).

#### 대안: 여러 노드를 한 번에 만들기

```python
>>> for i, tipo in enumerate(['SignalSourceNode', 'FFTNode', 'OscilloscopeNode']):
...     graph.add_catalog_node(tipo, 100 + i*200, 300)
```

### 노드 수동 연결

구문: `graph.connect_nodes(소스, 대상, '출력_포트', '입력_포트')`입니다. 포트는 각 노드에 따라 다르므로 이름을 가정하지 마세요.

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', 'port_a')
```

> **💡 참고:** 연결하기 전에 항상 `graph.nodes[N].PORTS`를 확인하세요. FFT 노드에는 `'input'` 및 `'magnitude'`가 있습니다. 생성기에는 `'output'`이 있습니다.

#### 대안: 기본 포트에 연결

입력 포트의 정확한 이름을 모르는 경우 일부 노드는 첫 번째 사용 가능한 포트를 사용하기 위해 `None`을 허용합니다:

```python
>>> graph.connect_nodes(graph.nodes[0], graph.nodes[2], 'output', None)
```

### 노드 삭제

```python
>>> graph.remove_node(graph.nodes[2])
```

노드  관련된 모든 연결을 삭제합니다. `graph.nodes`의 인덱스가 재정렬되므로 오래된 참조를 유지하지 마세요.

#### 대안: 범주의 모든 노드 삭제

```python
>>> for n in list(graph.nodes):
...     if n.META.get('category') == 'Processing':
...         graph.remove_node(n)
```

### 노드의 포트 보기

```python
>>> graph.nodes[1].PORTS
```

**예시 출력:**
```
{'input': ('left', 0.5), 'output': ('right', 0.25), 'magnitude': ('right', 0.5), 'phase': ('right', 0.75)}
```

키는 포트 이름(문자열)입니다. 값은 시각적 위치가 포함된 플입니다. 연결을 위해 키만 관심이 있습니다.

#### 대안: 간단한 목록으로 포트 보기

```python
>>> list(graph.nodes[1].PORTS.keys())
```

---

## 3. 흐름 실행 및 결과 보기

### 전체 그래프 실행

```python
>>> graph.execute_flow()
```

이 메서드는 메인 창이 아닌 다이어그램(`graph`)에 속합니다. 위상 순서로 모든 노드를 통과하고 각 노드를 실행하며 결를 캐시합니다. 아무것도 반환하지 않습니다. 데이터는 내부적으로 저장됩니다.

#### 대안: 특정 분기 계산 강제 실행

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
```

이것은 표시된 노드의 상위 트리를 모두 다시 계산하고 전역 캐시를 수정하지 않고 결과를 직접 반환합니다.

### 노드의 캐시된 데이터 보기

특정 노드의 처리된 데이터에 액세스해야 하는 경우 두 가지 방법으로 수행할 수 있습니다:

#### 직접 방법(객체별)
```python
>>> graph.node_values[graph.nodes[1]]
```

**예시 출력:**
```
{'output': None, 'magnitude': (array([0., 78.125, ...]), array([0.0013, 0.0183, ...])), 'phase': (array([0., 78.125, ...]), array([0., 0.687, ...]))}
```

> **⚠️ 경고:** 장면에서 노드 순서가 변경된 경우(예: 노드를 삭제하거나 추가할 때) 또는 객체 인스턴스가 사전에 저장된 키와 정확히 일치하지 않는 경우 이 양식이 `KeyError`로 실패할 수 있습니다.

#### 대체 방법(위치별)
```python
>>> list(graph.node_values.values())[1]
```

이 양식은 객체의 정확한 ID에 의존하지 않기 때문에 **더 안정적**입니다. 값의 순서는 마지막 `graph.execute_flow()` 동안 노드가 실행된 순서를 따릅니다. 인덱스 `[1]`은 해당 순서에서 두 번째 노드에 해당합니다.

> **💡 참고:** 실행 순서에서 각 노드의 인덱스를 보려면 다음을 사용할 수 있습니다:
> ```python
> >>> list(graph.node_values.keys())
> ```

> **⚠️ 주의:** `graph.node_values`는 항상 직접 배열을 반환하지는 않습니다. 프로세서 노드(FFT, 필터 등)의 경우 각 키가 출력 포트인 **사전**을 반환합니다. 소스 노드의 경우 `(x, y)` 튜플을 반환합니다.

#### 대안: 한 줄에 모든 노드의 데이터 보기
```python
>>> {n.name: type(v).__name__ for n, v in graph.node_values.items()}
```

### 소스 노드의 Y축에 액세스

생성기 노드(SignalSourceNode, FileInputNode 등)는 실행 시 `(시간, 신호)` 튜플을 반환합니다. Y축만 가오려면:

```python
>>> result = graph.get_node_branch_value(graph.nodes[0])
>>> y = result[1]
>>> y.max()
```

**예시 출력:**
```
Scalar NumPy (float64): 1.0
```

#### 대안: X축(시간) 가져오기

```python
>>> x = result[0]
>>> x[:5]
```

### Python의 할당: 중요한 세부 정보

Python에서 할당(`=`)은 **문장**이며 표현식이 아닙니다. 콘솔은 `x, y = ...` 후에 아무것도 인쇄하지 않습니다. 반환 값이 없기 때문입니다.

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
```

작동했는지 확인하려면 다음 줄에서 변수를 평가하세요:

```python
>>> x
>>> y.shape
```

또는 같은 줄에서 표현식을 연결하려면 `;`를 사용하세요:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); y.max()
```

또는 명시적 `print()`를 사용하세요:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0]); print(y.max())
```

---

## 4. 메 그래프에 그리기

### 그래프 지우기

```python
>>> app.plot_widget.clear_plot()
```

#### 대안: 즉시 지우고 다시 그리기

```python
>>> app.plot_widget.clear_plot(); graph.execute_flow()
```

### 콘솔에서 임의의 신호 그리기

NumPy로 배열을 만들고 어떤 노드를 거치지 않고도 플롯 위젯으로 직접 보낼 수 있습니다.

```python
>>> import numpy as np
>>> x = np.linspace(0, 1, 1000)
>>> y = np.sin(2 * np.pi * 10 * x)
>>> app.plot_widget.plot_waveform(x, y)
```

#### 대안: 사인의 합 그리기

```python
>>> y = np.sin(2*np.pi*5*x) + 0.3*np.sin(2*np.pi*50*x) + 0.1*np.random.randn(1000)
>>> app.plot_widget.plot_waveform(x, y)
```

### 소스 노드 결과 그리기

소스 노드는 `(x, y)`를 반환하므로 직접 언팩할 수 있습니다:

```python
>>> x, y = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x, y)
```

### FFT 노드 결과 그리기(다중 포트)

다중 출력 노드(FFT, 시간-주파수 분석 등)는 단순 튜플을 반환하지 않습니다. 각 키가 출력 포트인 `dict`를 반환합니다.

```python
>>> result = graph.get_node_branch_value(graph.nodes[1])
>>> result.keys()
```

**예시 출력:**
```
dict_keys(['output', 'magnitude', 'phase'])
```

노드에 일반 포트가 없으면 `'output'`이 `None`일 수 있습니다. 유용한 출력은 `'magnitude'` 및 `'phase'`이며, 이는 다시 `(frequencies, values)` 튜플입니다:

```python
>>> f, mag = result['magnitude']
>>> app.plot_widget.plot_waveform(f, mag)
```

#### 대안: 크기 대신 위상 그리기

```python
>>> f, phase = result['phase']
>>> app.plot_widget.plot_waveform(f, phase)
```

#### 대안: 두 신호 겹쳐 그리기

```python
>>> x1, y1 = graph.get_node_branch_value(graph.nodes[0])
>>> app.plot_widget.plot_waveform(x1, y1)
>>> x2, y2 = graph.get_node_branch_value(graph.nodes[2])  # 다른 노드
>>> app.plot_widget.plot_waveform(x2, y2)  # 겹쳐짐
```

> **💡 참고:** dict로 `x, y = result`를 시도하면 Python은 `ValueError: too many values to unpack`을 발생시킵니다. 언팩하기 전에 항상 `type(result)` 및 `result.keys()`로 검사하세요.

---

## 5. 실행 중 애플리케이션 수정

### 측면 매개변수 테이블 업데이트

코드로 매개변수를 수정하고 측면 테이블에 변경 사항을 반영하려면:

```python
>>> app.workspace_table.populate()
```

이 메서드는 인수를 받지 않습니다. 선택한 노드의 현재 값으로 테이블을 새로 고칩니다.

#### 대안: 다른 노드 선택 강제 및 새로 고침

```python
>>> graph.nodes[1].setSelected(True)
>>> app.workspace_table.populate()
```

### 도구 모음에서 노드 추가

```python
>>> app.add_node('SignalSourceNode')
```

이것은 도구 음의 "+" 버튼을 누르는 것과 동일합니다. 노드는 캔버스의 미리 정의된 위치에 배치됩니다.

### 창 제목 변경

`setWindowTitle`은 Qt의 기본 메서드입니다. 작동하지만 애플리케이션에 `update_title()`을 호출하는 타이머나 이벤트가 있어 자동으로 덮어쓸 수 있다는 점에 유의하세요.

```python
>>> app.setWindowTitle('나의 신호 실험실')
>>> app.windowTitle()
```

앱이 내부 상태(프로젝트 이름, 파일 등)에서 계산하는 "공식" 제목을 복원하려면:

```python
>>> app.update_title()
```

#### 대안: 프로젝트 이름이 포함된 제목

```python
>>> app.setWindowTitle(f'FloWorks — {graph.nodes[0].name}')
```

---

## 6. 콘솔의 탐색 및 생산성

콘솔은 단순한 `print()`가 아닙니다. 기록, 자동 완성 및 다중 줄 블록이 있습니다.

| 키 / 명령           | 동작                                                         |
|---------------------------|----------------------------------------------------------------|
| `↑` / `↓`                 | 실행된 명령 기록 탐색                |
| `Tab`                     | 네임스페이스의 변수, 속성 및 메서드 자동 완성     |
| `Ctrl + L`                | 전체 콘솔 지우기(텍스트 삭제, Python 상태는 아님)  |
| `if`, `for`, `def`, `class` | 다중 줄 블록의 경우 프롬프트가 `>>>`에서 `...`로 변경됨     |
| `Ctrl+C` (선택 영역)   | 콘솔에서 텍스트 복사                                     |
| `Ctrl+A`                  | 모든 내용 선택                                  |

> **💡 참고:** 자동 완성은 `rlcompleter`를 사용하며 주입된 전체 네임스페이스(`app`, `graph`, `selected_node`)와 세션에서 정의한 모든 변수를 인식합니다.

---

## 7. 고급 레시피

### 노드의 내부 매개변수 변경

노드의 매개변수는 평면 속성이 아닙니다. 이들은 `'preset'`, `'formula'` 또는 `'advanced'`와 같은 하위 섹션이 있는 `params` 사전 내에 중첩되어 있습니다. 절대로 `node.amplitude = 3.0`을 하지 마세요. 이렇게 하면 객체에 새 속성이 생성되지만 실제 매개변수는 수정되지 않습니다.

#### 사례 A: 프리셋 수정(사인, 사각파 등)

```python
>>> selected_node.params['mode'] = 'preset'
>>> selected_node.params['preset']['type'] = 'SINE'
>>> selected_node.params['preset']['amplitude'] = 2.0
>>> selected_node.params['preset']['frequency'] = 1000.0
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### 사례 B: 사용자 지정 수식 사용

```python
>>> selected_node.params['mode'] = 'formula'
>>> selected_node.params['formula']['expr'] = '2 * sin(2*pi*1000*t)'
>>> selected_node.params['formula']['vars'] = {'amp': 2.0, 'freq': 1000.0, 'offset': 0.0}
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

표현식은 시간 변수로 `t`를 사용합니다. `vars`의 값은 수식에서 참조할 수 있는 기호입니다. `vars`를 생략하면 노드는 기본값을 사용하고 수식이 변경 사항을 영하지 못할 수 있습니다.

#### 사례 C: 고급 매개변수 변경(샘플 속도, 지속 시간)

```python
>>> selected_node.params['advanced']['duration'] = 0.02
>>> selected_node.params['advanced']['sample_rate'] = 44100
>>> selected_node._generate_signal()
>>> graph.execute_flow()
```

#### 사례 D: 비생성기 노드의 매개변수 변경(예: FFT)

```python
>>> selected_node.params['window'] = 'hann'
>>> graph.execute_flow()
```

> **💡 참고:** `getattr(obj, '_generate_signal', lambda: None)()`는 안전한 패턴입니다. 메서드가 존재하면(생성기 노드) 호출하고, 그렇지 않으면 아무것도 하지 않고 오류를 발생시키지 않습니다. 프로세서 노드의 경우 `graph.execute_flow()`만으로 충분합니다.

### 사용 가능한 모든 노드 범주 나열

가져오기는 카탈로그를 로드하지만 자동으로 표시하지는 않습니다. Python에서 공적인 가져오기는 아무것도 인쇄하지 않습니다. 객체를 평가해야 합니다.

```python
>>> from nodes.node_catalog import NODE_CATEGORIES
>>> NODE_CATEGORIES
```

읽을 수 있는 요약을 보려면:

```python
>>> for cat, nodos in NODE_CATEGORIES.items():
...     print(f"{cat}: {len(nodos)} 노드")
```

#### 대안: 범주별 노드 이름 나열

```python
>>> {cat: [n.__name__ for n in nodos] for cat, nodos in NODE_CATEGORIES.items()}
```

### 모든 메서드의 도움말 보기

```python
>>> help(graph.connect_nodes)
```

Docstring이 콘솔에 직접 표시됩니다. 소스 코드를 열지 않고 메서드가 예상하는 인수를 발견하는 데 유용합니다.

#### 대안: 필터링된 속성 보기

```python
>>> [m for m in dir(graph) if 'connect' in m.lower()]
>>> [m for m in dir(selected_node) if 'param' in m.lower()]
```

---

## 8. 문제가 발생하면 어떻게 해야 하나요?

- **콘솔의 빨간색 오류:** 추적이 완전히 표시됩니다. 애플리케이션이 닫히지 않습니다. 명령을 수정하고 다시 시도할 수 있습니다.
- **인터페이스가 멈춤:** 아마도 무한 루프를 작성했습니다. 콘솔은 별도의 스레드에서 실행되지만 루프가 GUI 스레드에 영향을 미치면 애플리케이션을 다시 시작하세요.
- **예기치 않은 `None`:** 노드가 데이터 대신 `None`을 반환하면 업스트림에 연결되어 있는지(`graph.connections`) 및 흐름이 실행되었는지(`graph.execute_flow()`) 확인하세요.
- **`ValueError: too many values to unpack`:** dict를 튜플인 것처럼 언팩하려고 합니다. 먼저 `result.keys()`를 사용하세요.
- **`ValueError: not enough values to unpack`:** 2개의 값을 예상하지만 노드가 1개(dict) 또는 3개(스펙트로그램)를 반환합니다. 언팩하기 전에 `type(result)`로 검사하세요.
- **`AttributeError`:** 객체에 해당 속성이 없습니다. `dir(obj)` 또는 `[a for a in dir(obj) if '단어' in a.lower()]`를 사용하여 올바른 이름을 찾으세요.
- **실행 시 아무 일도 일어나지 않음:** 체인에 연된 소스 노드가 하나 이상 있는지 및 `graph.execute_flow()`가 호출되었는지 확인하세요. 프로세서 노드는 혼자서 데이터를 생성하지 않습니다.
- **플롯이 변경되지 않음:** 매개변수를 수정한 후 `graph.execute_flow()`를 호출했는지 확인하세요. `params`만 변경하면 자동으로 다시 계산되지 않습니다.

---

© 2026 FloWorks — 신호 실험실
