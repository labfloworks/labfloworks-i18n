---
title: FloWorks
description: Uniwersalne wizualne laboratorium do przetwarzania sygnałów, instrumentacji naukowej i automatyzacji.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Uniwersalne wizualne laboratorium dla sygnałów, instrumentacji i AI

Przetwarzanie naukowe • DSP • VISA/SCPI • Automatyzacja • Machine Learning

![Zrzut ekranu FloWorks](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[Pierwsze kroki z FloWorks](getting-started.md){ .md-button }
[Anatomia interfejsu](interface-anatomy.md){ .md-button .md-button--primary }
[Filozofia](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## Czym jest FloWorks?
FloWorks to **wizualne laboratorium open-source** (Python + PySide6), w którym budujesz systemy łącząc bloki (węzły) zamiast pisać linie kodu.

Wyobraź sobie cyfrowe Płótno, na którym łączysz generatory sygnałów, filtry matematyczne, kontrolery sprzętowe (VISA/SCPI) i modele sztucznej inteligencji za pomocą wirtualnych kabli. Wszystko opiera się na **przepływie danych**: łączysz wyjście jednego bloku z wejściem drugiego, aby przetwarzać informacje, automatyzować urządzenia lub analizować wyniki w czasie rzeczywistym.

Jest skierowane do studentów, badaczy, inżynierów i każdego, kto chce eksperymentować, uczyć się lub tworzyć prototypy złożonych systemów w intuicyjny sposób, bez bariery tradycyjnego programowania.

### Misja
Scentralizować przepływ pracy eksperymentalnej w jednym wizualnym, otwartym i dostępnym narzędziu. Chcemy, aby użytkownicy skupili się na *eksperymentowaniu i odkrywaniu*, a nie na walce ze złożonością oprogramowania ani kosztami licencji.

### Wizja
Świat, w którym jedyną barierą między pomysłem eksperymentalnym a jego realizacją jest ciekawość eksperymentatora. FloWorks aspiruje do bycia platformą referencyjną dla nauki i techniki, zbudowaną przez i dla globalnej społeczności, eliminując mury zamkniętych narzędzi.

### Zasady
* **Całkowita wolność (MIT License):** Wiedza i narzędzia muszą być wolne i dostępne dla wszystkich.
* **Nieskończona rozszerzalność:** Jeśli brakuje bloku, każdy może go stworzyć i zintegrować z ekosystemem przy użyciu Pythona.
* **Wizualna transparentność:** Każdy etap procesu można inspekcjonować, debugować i rozumieć graficznie.
* **Połączenie ze światem rzeczywistym:** To nie tylko symulacja; pozwala sterować prawdziwą instrumentacją naukową bezpośrednio z Płótna.

W przeciwieństwie do zamkniętych lub wysoce wyspecjalizowanych narzędzi, FloWorks jest zaprojektowane jako modułowy i rozszerzalny ekosystem, w którym każdy komponent jest wielokrotnego użytku i łączony węzeł.

---

## Główne możliwości

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Rozszerzalny ekosystem węzłów**

    Techniczny katalog zorganizowany w warstwy: Źródła, Przetwarzanie, Kontrola, Hardware i Skryptowanie.

    Dynamiczna rejestracja, deklaratywna serializacja i jasne kontrakty dla szybkiego rozwoju.

    [:material-arrow-right: Dokumentacja węzłów](node-reference.md)

-   **:material-connection: Integracja VISA/SCPI**

    Bezpośrednie połączenie z oscyloskopami, miernikami LCR i generatorami.

    Obsługa wielokanałowa, zintegrowana symulacja przez `PyVISA-py` i zarządzanie firewallem w trybie przenośnym.

    [:material-arrow-right: Instrumentacja](instrumentation.md)

-   **:material-package-variant-closed: Przenośny format `.sflow`**

    Standard ZIP z samowystarczalną zawartością: graf JSON, tablice `.npy` i metadane.

    Całkowita powtarzalność eksperymentów i automatyczna normalizacja DPI.

    [:material-arrow-right: Format .sflow](sflow-format.md)

-   **:material-translate: Zaawansowana internacjonalizacja**

    Zmiana języka w trakcie działania bez ponownego uruchamiania aplikacji.

    Hierarchiczne tłumaczenia JSON i persistencja preferencji.

    [:material-arrow-right: Przewodnik i18n](translation-guide.md)

-   **:material-tools: SDK i szybki rozwój**

    Szablon bazowy (`template_node.py`), mixin serializacji i przewodniki krok po kroku.

    Architektura przygotowana na wtyczki i rozszerzanie przez społeczność.

    [:material-arrow-right: Tworzenie węzłów](adding-a-new-node.md)

</div>

---

## Obszary zastosowań

| Obszar | Zastosowania |
|--------|--------------|
| 🎓 **Edukacja** | Fizyka, elektronika, matematyka, laboratoria STEM |
| ⚙️ **Inżynieria** | DSP, sterowanie, instrumentacja, metrologia |
| 🤖 **AI** | ML, optymalizacja, potoki hybrydowe |
| 🔬 **Badania** | Automatyzacja i akwizycja danych |
| 🔌 **Hardware** | VISA/SCPI, symulacja i systemy hybrydowe |

---

!!! tip "Nowy w FloWorks?"

    Zacznij od sekcji **Pierwsze kroki z FloWorks**, następnie **Anatomia interfejsu**, aby zrozumieć architekturę interfejsu graficznego, a na koniec zapoznaj się z **Architekturą ogólną**, aby zrozumieć przepływ danych i strukturę silnika topologicznego.

---

!!! info "Model Open Core"

    FloWorks wykorzystuje model **Free/Open Core** na licencji **MIT License**.

    Rdzeń pozostaje wolny i otwarty, podczas gdy przyszłe rozszerzenia korporacyjne, programowe lub marketplace będą opcjonalne.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Przetwarzanie wizualne • Instrumentacja • Nauka • AI

<small>Dokumentacja zbudowana za pomocą MkDocs Material</small>

</div>
