---
title: Instrumentacja VISA/SCPI
description: Przewodnik po łączeniu, konfiguracji i używaniu rzeczywistego i symulowanego sprzętu w FloWorks za pośrednictwem standardu VISA/SCPI.
---

# 🔌 Instrumentacja VISA/SCPI

FloWorks integruje bezpośrednią komunikację z prawdziwymi instrumentami laboratoryjnymi za pomocą protokołu **SCPI** (Standard Commands for Programmable Instruments) nad warstwą abstrakcji **VISA** (Virtual Instrument Software Architecture). Ponadto oferuje symulatory w czystym Pythonie do tworzenia, testowania i udostępniania przepływów bez konieczności posiadania fizycznego sprzętu.

---

## 🌐 Czym jest VISA/SCPI?

| Technologia | Opis |
|-------------|------|
| **VISA** | Standardowa warstwa abstrahująca interfejs fizyczny (USB-TMC, Ethernet/LAN, GPIB, RS‑232). Umożliwia przejście z instrumentu rzeczywistego na symulowany wyłącznie przez zmianę ciągu połączeniowego. |
| **SCPI** | Standaryzowany język poleceń ASCII do sterowania generatorami, oscyloskopami, multimetrami, miernikami LCR itp. Producenci rozszerzają standard, ale jego podstawa jest uniwersalna. |
| **PyVISA** | Backend Python używany przez FloWorks. Obsługuje `@py` (symulacja czysta) oraz backendy natywne (`@ni`, `@ivi`, `@keysight`, itp.). |

---

## ⚙️ Typowa konfiguracja

=== "📍 Ciąg połączeniowy (Resource String)"
    Standardowy format VISA:
    - `USB0::0x1AB1::0x0588::DS1ZA123456789::INSTR` (Oscyloskop USB)
    - `TCPIP0::192.168.1.100::inst0::INSTR` (LAN/Ethernet)
    - `ASRL1::INSTR` (Port szeregowy RS-232)
    - `GPIB0::1::INSTR` (GPIB legacy)

=== "⏱️ Limit czasu i opcje"
    - **Timeout**: Konfigurowalny w ms. Zwiększ, jeśli instrument wymaga długich pomiarów lub sweepów częstotliwości.
    - **Inicjalizacja**: Niektóre węzły pozwalają wstrzykiwać niestandardowe polecenia SCPI przy połączeniu (np. `*CLS`, `SYST:PRES`, `:CHAN1:DISP ON`).

---

## 📡 Dostępne węzły sprzętowe

<div class="grid cards" markdown>

- **🔭 Oscyloskop SCPI**
  Przechwytuje przebiegi czasowe. Obsługuje wiele kanałów, automatyczną skalę, trigger sprzętowy i menu „Pokaż kanał" do przełączania sygnałów na żywo.

- **⚡ Miernik LCR**
  Mierzy impedancję, indukcyjność, pojemność, opór i współczynnik strat. Zwraca `master_payload` z danymi pierwotnymi i wtórnymi w jednym pomiarze.

- **🎛️ Generator funkcji arbitralnych**
  Wysyła sygnały do sprzętu SDG lub symuluje wyjścia. Konfiguruje modulację (AM/FM/PM), sweep liniowy/logarytmiczny, burst i fazę.

- **📊 Multimetr cyfrowy (DMM)** *(W rozwoju)*
  Interfejs SCPI do pomiarów napięcia DC/AC, prądu, oporu i częstotliwości. Zgodny z Keithley, Agilent i Rigol.

- **🔋 Programowalne źródło zasilania** *(W rozwoju)*
  Sterowanie napięciem/prądem wyjściowym z zabezpieczeniem OVP/OCP. Przydatne w zautomatyzowanych stanowiskach testowych.

</div>

---

## 🔄 Typowy przepływ pracy

1. **Dodaj węzeł** na Płótno z Paska narzędzi (`Źródła` lub `Przyrządy`).
2. **Skonfiguruj połączenie**: Wybierz backend, wprowadź ciąg VISA i dostosuj timeout/inicjalizację.
3. **Połącz z przepływem**: Połącz wyjście instrumentu z węzłami przetwarzania (FFT, filtry, arytmetyka) lub wizualizacji.
4. **Uruchom (`F5`)**: Silnik topologiczny żąda akwizycji, driver parsuje odpowiedź SCPI i pakuje dane.
5. **Wizualizuj/Eksportuj**: Dane przepływają przez graf i są dalej przetwarzane przez kolejne węzły.

---

## 🛠️ Rozwiązywanie problemów

!!! warning "1. VISA nie znajduje instrumentu (`VI_ERROR_RSRC_NFOUND`)"
    - **Przyczyna:** Błędny ciąg, odłączony kabel lub backend nie wykrywa urządzenia.
    - **Rozwiązanie:** Uruchom `pyvisa-shell` lub narzędzie producenta (NI MAX, Keysight Connection Expert), aby wylistować zasoby. Sprawdź uprawnienia użytkownika.

!!! warning "2. Timeout podczas akwizycji"
    - **Przyczyna:** Powolny sweep, niespełniony trigger lub instrument zajęty innym zadaniem.
    - **Rozwiązanie:** Zwiększ timeout w węźle. Sprawdź konfigurację triggera oscyloskopu (`AUTO` lub `NORMAL`). Użyj `*CLS` na początku.

!!! warning "3. Symulacja nie odpowiada lub kończy się błędem"
    - **Przyczyna:** `PyVISA-py` nie jest zainstalowany lub występuje konflikt z innym backendem.
    - **Rozwiązanie:** `pip install pyvisa-py`. W węźle jawnie wybierz `@py` jako backend.

!!! warning "4. Błędy SCPI (`Command Error`, `Execution Error`)"
    - **Przyczyna:** Polecenie nieobsługiwane przez firmware lub niepoprawna składnia.
    - **Rozwiązanie:** Sprawdź podręcznik programowania SCPI instrumentu. Niektórzy producenci wymagają prefiksów `:` lub terminatorów `\n`. FloWorks dodaje `\n` automatycznie, ale terminator możesz dostosować w driverze.

!!! info "5. Tworzenie węzła dla nieobsługiwanego instrumentu"
    - Odziedzicz po `BaseNode` i użyj wzorca `DeviceBase` w `instrument/`.
    - Zaimplementuj driver `headless` zwracający krotki `(x, y)` lub `master_payload`.
    - Postępuj zgodnie z [📘 Przewodnikiem: Dodaj nowy węzeł](adding-a-new-node.md), aby zarejestrować porty, serializację i i18n.

---

## 📚 Powiązane zasoby

- [🧩 Techniczna dokumentacja węzłów](node-reference.md) → Szczegóły `oscilloscope_node`, `generator_node` i kontrakty serializacji.
- [📦 Przewodnik po przenośnym buildzie](guia-ejecutable-portable.md) → Obsługa firewalla, `resource_path()` i pakowanie PyInstaller.
- [📘 Dodaj nowy węzeł](adding-a-new-node.md) → Jak rozszerzyć `instrument/` i zarejestrować własne drivery.
- [📄 Format `.sflow`](sflow-format.md) → Jak zapisywane są konfiguracje sprzętowe i przechwycone tablice.
