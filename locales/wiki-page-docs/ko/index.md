---
title: FloWorks
description: 신호 처리, 과학적 계측 및 자동화를 위한 범용 시각적 실험실입니다.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## 신호, 계측 및 AI를 위한 범용 시각적 실험실

과학적 처리 • DSP • VISA/SCPI • 자동화 • 머신 러닝

![FloWorks 스크린샷](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[FloWorks 시작하기](getting-started.md){ .md-button }
[인터페이스 구조](interface-anatomy.md){ .md-button .md-button--primary }
[철학](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## FloWorks란?
FloWorks는 코드 줄을 작성하는 대신 블록(노드)을 연결하여 시스템을 구축하는 **오픈 소스 시각적 실험실**(Python + PySide6)입니다.

신호 생성기, 수학적 필터, 하드웨어 컨트롤러(VISA/SCPI) 및 인공 지능 모델을 가상 케이블로 연결하는 디지털 캔버스를 상상해 보세요. 모든 것은 **데이터 흐름**을 기반으로 합니다: 정보를 처리하고, 장비를 자동화하거나 결과를 실시간으로 분석하기 위해 한 블록의 출력을 다른 블록의 입력에 연결합니다.

학생, 연구원, 엔지니어 및 전통적인 프로그래밍의 장벽 없이 직관적으로 실험하고, 학습하거나 복잡한 시스템을 프로토타이핑하려는 모든 사람을 위해 설계되었습니다.

### 미션
실험 워크플로를 단일 시각적, 개방적이고 근 가능한 도구로 중앙 집중화합니다. 사용자가 소프트웨어의 복잡성이나 라이선스 비용과 씨름하는 대신 *실험하고 발견*하는 데 집중할 수 있기를 바랍니다.

### 비전
실험적 아이디어와 그 실행 사이의 유일한 장벽이 실험자의 호기심인 세상입니다. FloWorks는 전 세계 커뮤니티에 의해, 전 세계 커뮤니티를 위해 구축된 과학 및 공학을 위한 참조 플랫폼이 되는 것을 목표로 하며, 사유 도구의 벽을 제거합니다.

### 원칙
* **완전한 자유:** 지식과 도구는 모든 사람이 접근할 수 있어야 합니다. FloWorks는 무료로 사용할 수 있으며 개방적이고 확장 가능한 코어를 지향합니다.
* **무한한 확장성:** 블록이 부족하면 누구나 Python을 사용하여 생성하고 에코시스템에 통합할 수 있습니다.
* **시각적 투명성:** 프로세스의 각 단계를 그래픽으로 검사하고, 디버그하고, 이해할 수 있습니다.
* **실제 세계와의 연결:** 단순한 시뮬레이션이 아닙니다. 캔버스에서 직접 실제 과학 기기를 제어할 수 있습니다.

폐쇄적이거나 고도로 전문화된 도구 달리 FloWorks는 각 구성 요소가 재사용 가능하고 연결 가능한 노드인 모듈식 확장 가능한 에코시스템으로 설계되었습니다.

---

## 주요 기능

<div class="grid cards" markdown>

-   **:material-puzzle-outline: 확장 가능한 노드 에코시스템**

    계층적으로 구성된 기술 카탈로그: 소스, 처리, 제어, 하드웨어 및 스크립팅.

    동적 등록, 선언적 직렬화 및 빠른 개발을 위한 명확한 계약.

    [:material-arrow-right: 노드 참조](node-reference.md)

-   **:material-connection: VISA/SCPI 통합**

    오실로스코프, LCR 미터 및 생성기와의 직접 연결.

    다중 채널 지원, `PyVISA-py`를 통한 통합 시뮬레이션 및 휴대용 모드의 방화벽 관리.

    [:material-arrow-right: 계측](instrumentation.md)

-   **:material-package-variant-closed: 휴대용 `.sflow` 형식**

    JSON 그래프, `.npy` 배열 및 메타데이터가 포함된 자체 포함 ZIP 표준.

    실험의 완전한 재현성 및 자동 DPI 정규화.

    [:material-arrow-right: .sflow 형](sflow-format.md)

-   **:material-translate: 고급 국제화**

    앱을 다시 시작하지 않고 핫 언어 변경.

    계층적 JSON 번역 및 환경 설정 지속성.

    [:material-arrow-right: i18n 가이드](translation-guide.md)

-   **:material-tools: SDK 및 신속한 개발**

    기본 템플릿(`template_node.py`), 직렬화 믹스인 및 단계별 가이드.

    플러그인 및 커뮤니티 확장을 위한 준비된 아키텍처.

    [:material-arrow-right: 노드 만들기](adding-a-new-node.md)

</div>

---

## 적용 분야

| 분야 | 애플리케이션 |
|------|--------------|
| 🎓 **교육** | 물리학, 전자공학, 수학, STEM 실험실 |
| ⚙️ **엔지니어링** | DSP, 제어, 계측, 계측학 |
| 🤖 **AI** | ML, 최적화, 하이브리드 파이프라인 |
| 🔬 **연구** | 자동화 및 데이터 수집 |
| 🔌 **하드웨어** | VISA/SCPI, 시뮬레이션 및 하이브리드 시스템 |

---

!!! tip "FloWorks가 처음이신가요?"

    **FloWorks 시작하기** 섹션부터 시작한 다음 그래픽 인터페이스 아키텍처를 이해하기 위해 **인터페이스 구조**를 살펴보고, 마지막으로 데이터 흐름과 토폴로지 엔진 구조를 이해하기 위해 **일반 아키텍처**를 탐색하세요.

---

<div markdown="1" style="text-align: center;">

## FloWorks

시각적 처리 • 계측 • 과학 • AI

<small>MkDocs Material로 구축된 문서</small>

</div>
