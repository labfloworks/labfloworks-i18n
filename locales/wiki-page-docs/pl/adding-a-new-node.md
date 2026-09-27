---
title: Przewodnik dodawania nowego węzła do FloWorks
description: Samouczek krok po kroku dotyczący tworzenia, rejestracji i integracji niestandardowych węzłów z silnikiem przepływu FloWorks.
---

# 📘 Przewodnik dla deweloperów: Jak dodać nowy węzeł do FloWorks

Ten przewodnik opisuje kompletny proces tworzenia nowego typu węzła w FloWorks, zapewniając jego prawidłową integrację z silnikiem przepływu, interfejsem użytkownika, motywami wizualnymi i systemem internacjonalizacji.

---

## 📋 Spis treści
- [📘 Przewodnik dla deweloperów: Jak dodać nowy węzeł do FloWorks](#-przewodnik-dla-deweloperów-jak-dodać-nowy-węzeł-do-floworks)
  - [📋 Spis treści](#-spis-treści)
  - [1. Wprowadzenie do architektury](#1-wprowadzenie-do-architektury)
  - [2. Korzystanie z szablonu `template_node.py`](#2-korzystanie-z-szablonu-template_nodepy)
  - [3. Krok po kroku: Tworzenie niestandardowego węzła](#3-krok-po-kroku-tworzenie-niestandardowego-węzła)
    - [3.1. Kopiowanie i zmiana nazwy szablonu](#31-kopiowanie-i-zmiana-nazwy-szablonu)
    - [3.2. Definiowanie portów i etykiet](#32-definiowanie-portów-i-etykiet)
    - [3.3. Implementacja logiki przetwarzania](#33-implementacja-logiki-przetwarzania)
    - [3.4. Dostosowanie wyglądu (opcjonalne)](#34-dostosowanie-wyglądu-opcjonalne)
    - [3.5. Dodawanie konfigurowalnych parametrów (opcjonalne)](#35-dodawanie-konfigurowalnych-parametrów-opcjonalne)
    - [3.6. Serializacja węzła (zapisywanie / ładowanie konfiguracji)](#36-serializacja-węzła-zapisywanie--ładowanie-konfiguracji)
  - [4. Integracja z systemem](#4-integracja-z-systemem)
  - [5. Internacjonalizacja (i18n)](#5-internacjonalizacja-i18n)
  - [6. Motywy wizualne](#6-motywy-wizualne)
  - [7. Lista kontrolna i rozwiązywanie problemów](#7-lista-kontrolna-i-rozwiązywanie-problemów)
    - [✅ Lista kontrolna](#-lista-kontrolna)
    - [🐛 Powszechne problemy](#-powszechne-problemy)
  - [8. Wniosek](#8-wniosek)

---

## 1. Wprowadzenie do architektury

FloWorks jest zbudowany na PySide6 i wykorzystuje model połączonych węzłów reprezentujących przepływ przetwarzania sygnałów.

---

## 2. Korzystanie z szablonu `template_node.py`

Aby ułatwić tworzenie nowych węzłów, udostępniony jest plik `nodes/template_node.py`. Ten szablon zawiera:
- Pełne wsparcie dla internacjonalizacji (podłączenie do `languageChanged`, metoda `update_language`).
- Pełne wsparcie dla motywów (metoda `update_theme`).
- Zintegrowaną pomoc w formacie HTML z trzema sekcjami.
- Zarządzanie wieloma konfigurowalnymi portami wejściowymi/wyjściowymi.
- Wiele wyjść za pomocą `get_output_for_port`.
- Wizualizację w wykresie poprzez `get_display_signal`.
- Tłumaczalne menu kontekstowe.

Zaleca się zawsze wychodzić z tego szablonu podczas tworzenia nowego węzła.

---

## 3. Krok po kroku: Tworzenie niestandardowego węzła

### 3.1. Kopiowanie i zmiana nazwy szablonu
1. Skopiuj `nodes/template_node.py` pod nazwą nowego węzła, np. `nodes/mi_nodo.py`.
2. Zmień nazwę klasy z `TemplateNode` na coś opisowego, np. `MiNodoNode`.
3. Dostosuj importy, jeśli to konieczne.

### 3.2. Definiowanie portów i etykiet
!!! warning "Ważne: Zgodność nazw"
    Nazwy portów w `PORTS`, `PORT_LABELS` i klucze słownika zwracanego przez `execute_program` muszą być **dokładnie takie same** (w tym wielkość liter). Szablon zawiera teraz mapowanie aliasów (`'data_in'` → pierwszy lewy port) dla większej odporności.

Edytuj słownik `PORTS` w górnej części pliku. Każdy wpis ma format:
```python
"nazwa_portu": ("strona", ułamek)
```
- **Możliwe strony:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Ułamek:** wartość między `0.0` a `1.0` wskazująca pozycję wzdłuż strony.

**Przykład węzła z jednym wejściem i dwoma wyjściami:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
Słownik `PORT_LABELS` zawiera tekst, który pojawi się obok każdego portu. Zaleca się używanie kluczy tłumaczeń zamiast stałego tekstu (zobacz sekcję Internacjonalizacja).

### 3.3. Implementacja logiki przetwarzania
Kluczową metodą jest `execute_program(self, input_data)`. Metoda ta jest wywoływana przez silnik przepływu, gdy węzeł otrzyma dane.

**`input_data` może być:**
- `None`, jeśli nie ma wejścia.
- Krotka `(x, y)` dla sygnałów czasowych.
- Tablica 1D.
- Słownik `{nazwa_portu: dane}` dla węzłów z wieloma wejściami.

**Wartość zwracana:**
- Dla węzłów z jednym wyjściem zwróć dane bezpośrednio (np. krotkę `(x, y)`).
- Dla węzłów z wieloma wyjściami zwróć słownik, którego klucze odpowiadają nazwom portów wyjściowych zdefiniowanych w `PORTS`.

```python
def execute_program(self, input_data):
    # Przetwórz input_data i wygeneruj wyniki
    wynik_magnitude = (freq, mag)
    wynik_phase = (freq, phase)
    return {
        "magnitude": wynik_magnitude,
        "phase": wynik_phase
    }
```

!!! tip "Uwaga o ogólnych nazwach portów"
    Silnik przepływu może czasem przekazać słownik z kluczami takimi jak `'data_in'` zamiast rzeczywistej nazwy portu (zwłaszcza jeśli użytkownik nie kliknie dokładnie na okrąg). Szablon zawiera już kod obsługujący ten przypadek:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    To zapobiega awarii węzła z powodu nieprecyzyjnego połączenia.

Szablon zawiera już przykład w komentarzu. Ponadto implementuje `get_output_for_port(self, port_name)`, aby silnik mógł kierować każde wyjście:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Dostosowanie wyglądu (opcjonalne)
Metoda `paint()` rysuje tło, tytuł, stan i dodatkowy tekst. Możesz zmodyfikować:
- Kolory (automatycznie aktualizowane przez `update_theme`).
- Tekst stanu (używając atrybutu `self._status`).
- Informacje podsumowujące (np. szczyt amplitudy).

Szablon zawiera podstawowy przykład.

### 3.5. Dodawanie konfigurowalnych parametrów (opcjonalne)
Jeśli Twój węzeł wymaga parametrów regulowanych przez użytkownika (np. rozmiar okna, częstotliwość odcięcia), możesz:
1. Dodać atrybuty w `__init__` (np. `self.window_size = 512`).
2. Utworzyć okno konfiguracyjne (dziedziczące z `QDialog`).
3. Podłączyć okno w `open_config_dialog()` (metoda już obecna w szablonie).
4. Zaktualizować parametry z okna i wywołać `self.update()`.

### 3.6. Serializacja węzła (zapisywanie / ładowanie konfiguracji)
Aby węzeł mógł zapisywać i przywracać swoje parametry podczas kopiowania/wklejania, cofania/ponawiania lub przy użyciu poleceń Zapisz/Otwórz w menu Plik, musi dziedziczyć po mixinie serializacji i zadeklarować swoje atrybuty.

1. Zaimportuj mixin do swojego pliku:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Zmień dziedziczenie klasy, aby uwzględnić mixin przed `QGraphicsObject`:
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
    ```
3. Zdefiniuj listę `SERIALISABLE` na poziomie klasy z nazwami atrybutów, które chcesz zachować. Obsługuje tylko proste typy (`int`, `float`, `str`, `bool`), listy, słowniki lub tablice NumPy (te ostatnie są automatycznie zapisywane jako pliki `.npy` wewnątrz `.sflow`).
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frecuencia', 'amplitud', 'configuracion']
    ```
4. Upewnij się, że te atrybuty są inicjalizowane w `__init__`:
    ```python
    self.frecuencia = 1000.0
    self.amplitud = 1.0
    self.configuracion = {'tipo': 'seno', 'fase': 0}
    ```

Dzięki temu nie musisz pisać metod `serialize`/`deserialize`; mixin automatycznie zajmuje się zapisywaniem i przywracaniem wartości.

Jeśli Twój węzeł wymaga dodatkowej logiki przy ładowaniu (np. ponowne podłączenie urządzenia sprzętowego), możesz nadpisać `deserialize`, wywołując najpierw metodę rodzica:
```python
def deserialize(self, data):
    super().deserialize(data)   # przywraca atrybuty z SERIALISABLE
    self._iniciar_dispositivo()
```

---

## 4. Integracja z systemem

Po utworzeniu pliku węzła wystarczy wkleić go do folderu `nodes`, aby pojawił się w interfejsie i działał z resztą systemu.

---

## 5. Internacjonalizacja (i18n)

Wszystkie widoczne teksty muszą być tłumaczalne za pomocą `tr("klucz", default="...")`. Szablon już to implementuje. Musisz dodać odpowiednie klucze do plików JSON w folderze `locales/`.

**Zalecana struktura:**
```json
{
   "nodes": {
     "mi_nodo": {
       "title": "Mi Nodo",
       "tooltip": "Opis podręczny",
       "ports": {
         "input": "Wejście",
         "output1": "Wyjście 1",
         "output2": "Wyjście 2"
      },
       "status": {
         "no_data": "Brak danych",
         "ready": "Gotowe"
      },
       "menu": {
         "show_output": "Pokaż wyjście",
         "configure": "Konfiguruj..."
      },
       "help_title": "Pomoc - Mi Nodo",
       "help_html": "<h3>🎛️ Filter Node</h3>
<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mi_nodo": "Mi Nodo"
  }
}
```

Pomoc HTML podąża za formatem trzech sekcji wspólnych dla wszystkich węzłów (opis specyficzny + "Jak myśleć o systemie" + "Skróty i triki"). Szablon zawiera już strukturę w `get_help_text()`.

---

## 6. Motywy wizualne

Metoda `update_theme(self, theme)` otrzymuje słownik z kolorami zdefiniowanymi przez aktualny motyw. Szablon automatycznie aktualizuje:
- Tło węzła (`node_normal_bg`)
- Obramowanie (`node_selected_border`)
- Kolor tytułu i tekstu (`node_normal_text`)
- Kolory portów (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Upewnij się, że w `MainWindow` (lub `ThemeUpdater`) jest wywoływane `node.update_theme()` dla każdego węzła przy zmianie motywu.

---

## 7. Lista kontrolna i rozwiązywanie problemów

### ✅ Lista kontrolna
- [ ] Węzeł tworzy się poprawnie z paska narzędzi.
- [ ] Porty wyświetlają się na oczekiwanych pozycjach i są wykrywalne do połączeń (`Ctrl+klik`).
- [ ] Po otrzymaniu danych wejściowych wywoływane jest `execute_program` i sygnał jest przetwarzany.
- [ ] Wyjścia poprawnie propagują do podłączonych węzłów.
- [ ] Menu kontekstowe pozwala zmienić kanał wizualizacji (jeśli jest wiele wyjść).
- [ ] Po kliknięciu na węzeł wybrany sygnał jest rysowany w widgecie wykresu.
- [ ] Podwójne kliknięcie otwiera pomoc we właściwym formacie.
- [ ] Język zmienia się poprawnie (teksty tytułów, portów, menu).
- [ ] Motyw zmienia się poprawnie (kolory węzła i portów).
- [ ] Kopiowanie/wklejanie działa bez błędów.

!!! tip "Precyzyjne łączenie portów"
    Podczas łączenia węzłów upewnij się, że klikasz dokładnie na okrąg portu docelowego. Jeśli klikniesz na ciało węzła, system użyje ogólnej nazwy (`'data_in'`). Szablon teraz toleruje te nazwy, ale dobrą praktyką jest łączenie się bezpośrednio z okręgiem, aby zagwarantować właściwe kierowanie wielu wyjść.

### 🐛 Powszechne problemy

| Objaw | Prawdopodobna przyczyna | Rozwiązanie |
|---------|---------------|----------|
| Strzałka połączenia nie zakotwicza się do portu. | Okrąg portu nie ma `setData(0, port_name)` lub `get_port_scene_pos` nie jest zaimplementowane. | Sprawdź, że w `_create_ports` jest `circle.setData(0, port_name)` i że `get_port_scene_pos` używa tej nazwy. |
| Wyjścia nie docierają do podłączonych węzłów. | `execute_program` nie zwraca słownika (dla wielu wyjść) lub `get_output_for_port` nie jest zaimplementowane. | Upewnij się, że `execute_program` zwraca `{nazwa_portu: dane}` i że `get_output_for_port` zwraca odpowiednią wartość. |
| Po kliknięciu na węzeł nic się nie rysuje. | `get_display_signal` nie zwraca prawidłowej krotki `(x, y)` lub `display_channel` nie odpowiada istniejącemu wyjściu. | Sprawdź, że `get_display_signal` używa wybranego kanału i że dane to tablice NumPy. |
| Teksty nie aktualizują się przy zmianie języka. | Nie podłączono sygnału `languageChanged` lub `update_language` nie aktualizuje elementów. | Sprawdź podłączenie w `__init__`: `language_manager.languageChanged.connect(self.update_language)`. |
| Motyw nie jest stosowany. | Nie wywołano `update_theme` przy tworzeniu węzła lub przy zmianie motywu. | W `MainWindow`, po utworzeniu węzła, wywołaj `node.update_theme(self.theme_manager.current_theme())`. |
| Strzałka wskazuje na środek węzła. | Kliknięto na ciało zamiast okręgu, lub nazwa nie zgadza się z `PORTS`. | Kliknij bezpośrednio na okrąg. Sprawdź, że `get_port_scene_pos` ma mapowanie aliasów. |
| `NameError: name 'self' is not defined` przy imporcie. | Atrybuty instancji zadeklarowano poza `__init__`. | Wszystkie atrybuty jak `self.moj_parametr` muszą być zdefiniowane wewnątrz `__init__`. |
| Parametry znikają przy kopiowaniu/otwieraniu `.sflow`. | Węzeł nie dziedziczy z `SerializableMixin` lub nie zdefiniowano `SERIALISABLE`. | Zaimplementuj krok 3.6 tego przewodnika. |

---

## 8. Wniosek

Postępując zgodnie z tym przewodnikiem i korzystając z szablonu `template_node.py`, będziesz mógł efektywnie i spójnie z resztą systemu dodawać nowe węzły do FloWorks. Zawsze pamiętaj o zachowaniu kompatybilności z i18n i motywami dla profesjonalnego doświadczenia użytkownika.

Zachęcamy do wnoszenia własnych węzłów!
