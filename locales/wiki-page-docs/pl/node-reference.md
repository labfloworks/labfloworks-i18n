---
title: Dokumentacja techniczna węzłów
description: Zaktualizowany katalog, kontrakty rozszerzeń i zaawansowane możliwości systemu węzłów FloWorks
---

# 🧩 Dokumentacja techniczna węzłów

FloWorks nie polega na statycznym katalogu. Wykorzystuje **system dynamicznej rejestracji** oparty na jasnych kontraktach. Pozwala to rozszerzać platformę bez ingerencji w silnik topologiczny. Poniżej znajduje się zaimplementowany katalog, rzeczywiste możliwości techniczne i protokół bezpiecznego rozszerzania.

---

## 📂 Kategorie rdzenia

=== "📦 Widok warstwowy"
    <div class="grid cards" markdown>

    - **📥 Źródła/Wejście**
      Generują lub przechwytują sygnały początkowe. Obsługują zintegrowaną symulację, rzeczywisty sprzęt (VISA/SCPI) i tryb wielokanałowy.
    - **⚙️ Przetwarzanie**
      Transformują, łączą lub analizują dane. Zachowują wymiarowość i automatycznie interpolują, gdy jest to konieczne.
    - **🔀 Kontrola/Przepływ**
      Rozgałęziają, iterują lub warunkują Uruchomienie. Obejmują natywną obsługę sygnałów aktywacyjnych.
    - **🐍 Scripting/Zaawansowane**
      Wykonują dynamiczny kod Python z portami parametrycznymi (`# @param`), portami dynamicznymi (`# @input`/`# @output`) i persistencją stanu (`persist`).
    - **🔌 Hardware/Instrumentacja**
      Interfejsy dla oscyloskopów, mierników LCR i generatorów.
    - **📤 Wyjście/Eksport**
      Wizualizują, eksportują lub archiwizują wyniki. Obsługują motywy wizualne, profile użytkownika i profesjonalne formaty (PNG/PDF/SVG).

    </div>

---

## 📋 Zaimplementowany katalog techniczny

| Węzeł | Typ | Główna odpowiedzialność | Kluczowe cechy |
|-------|-----|------------------------|----------------|
| `SumNode` | Przetwarzanie | Operator arytmetyczny (+, -, *, /) dla dwóch wejść. | Automatycznie interpoluje sygnały o różnej rozdzielczości (FFTs). Zachowuje wymiarowość. |
| `RhombusNode` | Kontrola | Warunek (rozgałęzienie Tak/Nie). | Dwa porty wyjściowe. Ocenia warunek według progu lub logiki boolowskiej. |
| `TriggerNode` | Kontrola | Iterator/akumulator z aktywacją zewnętrzną. | Odbiera `(x, y, "trigger")`. Akumuluje do N iteracji i emituje wynik złożony/uśredniony. |
| `ScriptNode` | Zaawansowane | Zintegrowane środowisko skryptowe Python. | QScintilla, autouzupełnianie, `# @param`, porty dynamiczne, `persist`, szablony, konsola błędów, interpreter zewnętrzny z timeoutem. |
| `OscilloscopeNode` | Hardware | Przechwytywanie z oscyloskopów (SDS) lub mierników LCR. | Tryb symulacji, zintegrowany dialog firewallu, **obsługa wielokanałowa** (`out_primary`, `out_secondary`), menu „Pokaż kanał". |
| `GeneratorNode` | Źródło | Wysyłanie sygnałów do generatorów (SDG) lub symulacja wyjść. | Konfiguracja modulacji/sweep, zintegrowany dialog symulacji. |
| `GraphExporterNode` | Wyjście | Profesjonalny eksporter wykresów. | Konfiguracja dwuklikiem, osie niestandardowe, motywy, zapisane profile itp. |

---

## 🔍 ScriptNode: Kluczowe możliwości

> **🐍 Zintegrowane środowisko skryptowe**
>
> - **Zintegrowany edytor kodu:** Podstawowe podświetlanie składni, numeracja wierszy i zwijanie kodu.
> - **Panel dynamicznych parametrów:** Dyrektywy `# @param NAZWA : typ = wartość` wstrzykują edytowalne kontrolki (spinbox, pole tekstowe itp.) na panel boczny.
> - **Porty dynamiczne:** `# @input nazwa` i `# @output nazwa` tworzą porty w czasie rzeczywistym. Skrypt odbiera słownik `inputs` i zwraca `outputs`.
> - **Persistencja stanu:** Globalny słownik `persist`, który zachowuje wartości między Uruchomieniami.
> - **Szablony i Import/Export:** Menu rozwijane z bazowymi skryptami. Użytkownik może zapisywać własne skrypty w `nodes/script_node/templates/` lub importować/eksportować zewnętrzne pliki `.py`.
> - **Zintegrowana konsola błędów:** Wyświetla błędy składni/wykonania z dokładnym wskazaniem wiersza w edytorze.
> - **Pomoc i i18n:** Kontekstowe tooltips, przycisk `?` z szybką ściągawką i wszystkie teksty używają `tr()` do tłumaczenia.
> - **Zewnętrzny interpreter z timeoutem:** Konfigurowalna ścieżka (`# @python_path` lub przycisk „Przeglądaj…"). Izolowane Uruchomienie z limitem czasowym i fallback do interpretera wewnętrznego.
> - **Pełna serializacja:** Zapisuje skrypt, parametry, porty dynamiczne i stan `persist`. Po załadowaniu `.sflow` automatycznie odtwarza porty i parametry.

---

## 📚 Powiązane zasoby

- [📖 Mapa kodu i architektura](architecture-ii.md) → Odpowiedzialności według modułów i przepływy pracy.
- [🌐 Przewodnik internacjonalizacji (i18n)](i18n.md) → Jak dodawać języki i zarządzać kluczami `tr()`.
- [🛠️ Dodaj nowy węzeł (samouczek)](adding-a-new-node.md) → Krok po kroku z praktycznymi przykładami.
- [📦 Przewodnik kompilacji i dystrybucji](build.md) → Pakowanie PyInstaller, hooks i podpisy cyfrowe.

---

💡 **Brakuje węzła w tym katalogu?**
FloWorks jest zaprojektowane jako rozszerzalne. Jeśli potrzebujesz węzła, który nie istnieje, utwórz go zgodnie z kontraktem `BaseNode` i zarejestruj. Społeczność i przyszły marketplace będą stale rozszerzać ekosystem bez łamania kompatybilności.
