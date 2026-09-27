---
title: VISA/SCPI 계측
description: FloWorks에서 VISA/SCPI 표준을 사용하여 실제 및 시뮬레이션 하드웨어를 연결, 구성 및 사용하기 위한 가이드입니다.
---

# 🔌 VISA/SCPI 계측

FloWorks는 **SCPI**(Standard Commands for Programmable Instruments) 프로토콜을 통해 **VISA**(Virtual Instrument Software Architecture) 추상화 계층 위에서 실제 실험실 장비와 직접 통신을 통합합니다. 또한 물리적 하드웨어 없이도 흐름을 개발, 테스트 및 공유할 수 있도록 순수 Python 기반 시뮬레이터를 제공합니다.

---

## 🌐 VISA/SCPI란?

| 기술 | 설명 |
|------------|-------------|
| **VISA** | 물리적 인터페이스(USB-TMC, Ethernet/LAN, GPIB, RS‑232)를 추상화하는 표준 계층입니다. 연결 문자열만 수정하면 실제 장비에서 시뮬레이션 장비로 전환할 수 있습니다. |
| **SCPI** | 생성기, 오실로스코프, 멀티미터, LCR 미터 등을 제어하기 위한 표준화된 ASCII 명령 언어입니다. 제조업체는 표준을 확장하지만 기본은 보편적입니다. |
| **PyVISA** | FloWorks에서 사용하는 Python 백엔드입니다. `@py`(순수 시뮬레이션) 및 네이티브 백엔드(`@ni`, `@ivi`, `@keysight` 등)를 지원합니다. |

---

## ⚙️ 일반적인 구성

=== "📍 연결 문자열(Resource String)"
    표준 VISA 형식:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (USB 오실로스코프)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (RS-232 시리얼 포트)
    - `GPIB0::1::INSTR` (레거시 GPIB)

=== "⏱️ 타임아웃 및 옵션**"
    - **Timeout**: 밀리초 단위로 구성 가능합니다. 장비에 긴 측정이나 주파수 스윕이 필요한 경우 늘립니다.
    - **Initialization**: 일부 노드는 연결 시 사용자 지정 SCPI 명령을 주입할 수 있습니다(예: `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 사용 가능한 하드웨어 노드

<div class="grid cards" markdown>

- **🔭 SCPI 오실로스코프**  
  시간 도메인에서 파형을 캡처합니다. 멀티채널, 자동 스케일링, 하드웨어 트리거 및 "채널 표시" 메뉴를 지원하여 실시간으로 신호를 전환할 수 있습니다.

- **⚡ LCR 미터**  
  임피던스, 인덕턴스, 커패시턴스, 저항 및 손실 계수를 측정합니다. 단일 획득에서 기본 및 보조 데이터가 포함된 `master_payload`를 반환합니다.

- **🎛️ 임의 함수 생성기**  
  SDG 하드웨어에 신호를 보내거나 출력을 시뮬레이션합니다. 변조(AM/FM/PM), 선형/대수 스윕, 버스트 및 위상을 구성합니다.

- **📊 디지털 멀티미터(DMM)** *(확장 중)*  
  DC/AC 전압, 전류, 저항 및 주파수 측정을 위한 SCPI 인터페이스입니다. Keithley, Agilent 및 Rigol과 호환됩니다.

- **🔋 프로그래머블 전원 공급 장치** *(확장 중)*  
  OVP/OCP 호 기능이 있는 출력 전압/전류 제어입니다. 자동화된 테스트 벤치에 유용합니다.

</div>

---

## 🔄 일반적인 워크플로

1. **노드 추가**: 툴바에서 (`소스` 또는 `장비`) 캔버스에 노드를 추가합니다.
2. **연결 구성**: 백엔드를 선택하고, VISA 문자열을 입력하고, 타임아웃/초기화를 조정합니다.
3. **흐름에 연결**: 장비 출력을 처리(FFT, 필터, 산술) 또는 시각화 노드에 연결합니다.
4. **실행 (`F5`)**: 토폴로지 엔진이 획득을 요청하고, 드라이버가 SCPI 응답을 파싱하고 데이터를 패키징합니다.
5. **시각화/내보내기**: 데이터가 그래프를 통해 흘러 다음 노드에서 처리됩니다. 

---

## 🛠️ 문제 해결

!!! warning "1. VISA가 장비를 찾을 수 없음 (`VI_ERROR_RSRC_NFOUND`)"
    - **원인:** 잘못된 문자열, 연결되지 않은 케이블 또는 백엔드가 장비를 감지하지 못함.
    - **해결 방법:** `pyvisa-shell` 또는 제조업체 유틸리티(NI MAX, Keysight Connection Expert)를 행하여 유효한 리소스를 나열합니다. 사용자 권한을 확인합니다.

!!! warning "2. 획득 중 타임아웃"
    - **원인:** 느린 스윕, 트리거가 충족되지 않음 또는 장비가 다른 작업 중임.
    - **해결 방법:** 노드에서 타임아웃을 늘립니다. 오실로스코프 트리거가 올바르게 구성되었는지 확인합니다(`AUTO` 또는 `NORMAL`). 시작 시 `*CLS`를 사용합니다.

!!! warning "3. 시뮬레이션이 응답하지 않거나 실패"
    - **원인:** `PyVISA-py`가 설치되지 않았거나 다른 백엔드와 충돌.
    - **해결 방법:** `pip install pyvisa-py`를 실행합니다. 노드에서 `@py`를 백엔드로 명시적으로 선택합니다.

!!! warning "4. SCPI 오류 (`Command Error`, `Execution Error`)"
    - **원인:** 펌웨어에서 지원하지 않는 명령이거나 구문이 잘못되었음.
    - **해결 방법:** 장비의 SCPI 프로그래밍 매뉴얼을 참조하세요. 일부 제조업체는 `:` 접두사 또는 `
` 종결자가 필요합니다. FloWorks는 자동으로 `
`을 추가하지만 드라이버에서 종결자를 조정할 수 있습니다.

!!! info "5. 지원되지 않는 장비용 노드 만들기"
    - `BaseNode`를 상속하고 `instrument/`에서 `DeviceBase` 패턴을 사용합니다.
    - `(x, y)` 튜플 또는 `master_payload`를 반환하는 `headless` 드라이버를 구현합니다.
    - 포트, 직렬화 및 i18n을 등록하려면 [📘 가이드: 새 노드 추가](adding-a-new-node.md)를 따르세요.

---

## 📚 관련 리소스

- [🧩 노드 기술 참조](node-reference.md) → `oscilloscope_node`, `generator_node` 및 직렬화 계약 세부 정보.
- [📦 휴대용 실행 파일 가이드](guia-ejecutable-portable.md) → 방화벽 관리, `resource_path()` 및 PyInstaller 패키징.
- [📘 새 노드 추가](adding-a-new-node.md) → `instrument/` 확장 및 사용자 지정 드라이버 등록 방법.
- [📄 `.sflow` 형식](sflow-format.md) → 하드웨어 구성 및 캡처된 배열이 지속되는 방식.
