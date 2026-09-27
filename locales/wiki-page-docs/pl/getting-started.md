---
title: Pierwsze kroki z FloWorks
description: Szybki przewodnik po konfiguracji środowiska, uruchomieniu pierwszego przepływu i dostępie do wersji przenośnej.
---

# 🚀 Pierwsze kroki z FloWorks

Ten przewodnik przeprowadzi cię od zera do uruchomienia pierwszego przepływu przetwarzania sygnałów. FloWorks to aplikacja do diagramów przepływu dla sygnałów, zbudowana na Pythonie i PySide6, obsługująca rzeczywisty sprzęt (VISA/SCPI), zintegrowaną symulację, zaawansowane skrypty i zmianę języka w trakcie pracy.

---

## 🌊 Twój pierwszy przykładowy przepływ

Stworzymy prosty przepływ: wygenerujemy sygnał sinusoidalny i zwizualizujemy go w czasie rzeczywistym.

1. **Dodawanie węzłów**
   Na górnym pasku narzędzi wybierz `Źródło` → wybierz `Zaawansowany generator sygnałów`. Następnie z `Przetwarzanie` → wybierz na przykład `Spektralny`.
2. **Łączenie**
   Naciśnij `Ctrl+Klik` na porcie wyjściowym (`prawy`) generatora. Potem kliknij na porcie wejściowym (`lewy`) oscyloskopu. Albo po prostu kliknij port wyjściowy i przeciągnij go (trzymając wciśnięty przycisk) na port wejściowy następnego węzła.
3. **Konfiguracja (opcjonalnie)**
   Kliknij węzeł, w lewym panelu bocznym pojawi się edytor z parametrami wybranego węzła do dostosowania warunków pracy. W dolnej części znajduje się wykres wizualizujący dane generowane lub pozyskiwane przez węzły.
4. **Uruchomienie**
   Naciśnij `F5` lub przycisk ▶ na pasku narzędzi. Silnik topologiczny obliczy kolejność wykonania, przetworzy dane i zobaczysz falę na panelu graficznym. **Łącznik zostanie animowany, wskazując aktywny przepływ!**

---

## 🧠 Rozumienie portów: kategorie według kolorów

W FloWorks każdy port należy do **kategorii funkcjonalnej** zidentyfikowanej przez kolor. Prawidłowe połączenia wykonuje się **zawsze między portami tego samego koloru**: wyjście jednej kategorii łączy się wyłącznie z wejściem tej samej kategorii. Ponadto linia łącząca automatycznie przyjmuje kolor łączonych portów, ułatwiając czytanie wizualne.

| Typ | Kolor | Cel | Typowy przykład |
|------|-------|-----------|----------------|
| `control` | Biały | Przepływ sterowania / aktywacja. | Sygnał startowy do węza akwizycji. |
| `exec` | Szary | Wykonanie operacji lub kroków. | Wyzwolenie funkcji lub wywołania zwrotnego. |
| `data` | Zielony | Ogólne dane / sygnały numeryczne. | Wyjście generatora lub czujnika. |
| `int` | Niebieski | Liczby całkowite. | Indeks, rozmiar bufora, ID. |
| `float` | Cyjan | Liczby zmiennoprzecinkowe. | Amplituda, częstotliwość, próg. |
| `string` | Purpurowy | Łańcuchy tekstowe. | Nazwa pliku, etykieta. |
| `bool` | Różowy | Wartości logiczne (`True`/`False`). | Flaga stanu, włączenie. |
| `array` | Ciemnoniebieski | Tablice / wektory. | Sygnał wielokanałowy, lista próbek. |
| `trigger` | Pomarańczowy | Wyzwalacze / zdarzenia dyskretne. | Impuls synchronizacji, zbocze. |

**Złota zasada:**

- Łączą się tylko porty **dokładnie tego samego koloru** (wyjście ↔ wejście tej samej kategorii).
- System zapobiega nieprawidłowym połączeniom i podczas przeciągania wizualnie podświetla kompatybilne porty.
- Linia łącząca przyjmuje kolor połączonych portów; dzięki temu każda trasa jest identyfikowalna na pierwszy rzut oka.

**Filozofia FloWorks:**
Porty danych **zachowują wymiarowość** tablic. Nigdy nie jest stosowane automatyczne spłaszczenie: jeśli wchodzi macierz, wychodzi macierz, zachowując integralność sygnałów wielowymiarowych.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Nawigacja po kanwie (Canvas)

Opanuj przestrzeń roboczą dzięki tym gestom:

| Akcja | Jak to zrobić |
|--------|--------------|
| **Powiększenie** | Kółko myszy lub `Ctrl + kółko` |
| **Przesuwanie (pan)** | Przytrzymaj `Spacja` i przeciągnij, lub użyj środkowego przycisku myszy |
| **Zaznaczenie węzła** | Lewy klik na węzeł |
| **Zaznaczenie wielu** | Przeciągnij prostokąt lewym przyciskiem, lub `Ctrl + klik` na kilka węzłów |
| **Przesunięcie zaznaczenia** | Przeciągnij dowolny z zaznaczonych węzłów |
| **Otwarcie konfiguracji** | Podwójny klik na węzeł |

**Wskazówka:** Lewy panel aktualizuje się automatycznie z konfiguracją wybranego węzła, bez potrzeby otwierania dodatkowych okien.

---

## ⚡ Skróty klawiszowe i zaawansowane ruchy

Te skróty przekształcają zwykłego użytkownika w **power usera**:

| Skrót | Akcja |
|-------|--------|
| `F5` | Uruchom przepływ |
| `Ctrl + S` | Zapisz projekt (`.sflow`) |
| `Ctrl + Klik` | Połącz węzły (klik na port wyjściowy → klik na port wejściowy) |
| `Ctrl + C` / `Ctrl + V` | Kopiuj / wklej zaznaczone węzły |
| `Ctrl + Z` / `Ctrl + Y` | Cofnij / ponów |
| `Ctrl + Shift + L` | Auto-organizuj węzły na kanwie |
| `Del` | Usuń zaznaczone węzły |
| `Ctrl + A` | Zaznacz wszystkie węzły |

**Zaawansowane ruchy:**

- **Duplikowanie przepływu:** zaznacz grupę węzłów, `Ctrl + C`, `Ctrl + V` i przeciągnij kopię w inne miejsce.
- **Czyszczenie siatki:** użyj `Ctrl + Shift + L`, aby uporządkować całą kanwę jednym poleceniem.
- **Szybkie łączenie:** `Ctrl + Klik` na porcie wyjściowym, a potem zwykły klik na porcie wejściowym; FloWorks narysuje połączenie automatycznie.

---

## 🎨 Dostosowywanie środowiska

FloWorks dostosowuje się do ciebie, nie na odwrót.

### Zmiana motywu w trakcie pracy
Z górnego paska, menu **Widok → Motyw**, wybierz między jasnym, ciemnym i innymi. Interfejs zmienia się **natychmiast**, bez restartu i bez utraty przepływu.

### Rozmiar czcionki
W **Widok → Rozmiar czcionki** wybierz wartość predefiniowaną lub niestandardową. Cały interfejs dostosowuje się natychmiast.

### Język
W **Widok → Język** wybierz żądany język. FloWorks obsługuje **zmianę w trakcie pracy**: menu, przyciski i komunikaty są tłumaczone bez restartu aplikacji.

---

## ❗ Rozwiązywanie typowych problemów

| Problem | Prawdopodobna przyczyna | Rozwiązanie |
|----------|---------------|----------|
| Przepływ się nie uruchamia | Niektóre węzły nie są skonfigurowane lub połączenia są przerwane | Sprawdź, czy wszystkie węzły mają prawidłowe parametry i czy połączenia są między kompatybilnymi portami |
| Wykres się nie aktualizuje | Przepływ jest wstrzymany lub nie płyną dane | Upewnij się, że nacisnąłeś `F5` lub ▶, i że węzły źródłowe generują dane |
| Nie mogę połączyć dwóch węzłów | Porty są różnych typów | Sprawdź, czy oba porty są **danymi** lub oba **sterującymi** |
| Program działa wolno przy dużych przepływach | Zbyt wiele węzłów lub wykresów w czasie rzeczywistym | Zamknij nieużywane panele analizy lub zmniejsz częstotliwość próbkowania węzłów źródłowych |
| Motyw się nie zmienia | Niektóre widżety mogą nie być zarejestrowane | Zrestartuj aplikację i spróbuj ponownie (w przyszłych wersjach zostanie to rozwiązane) |

---

## 🧪 Szybkie przykłady praktyczne

Oprócz początkowego przepływu sinusoidalnego wypróbuj te mini-projekty, aby opanować FloWorks:

| Przykład | Zaangażowane węzły | Oczekiwany wynik |
|---------|-------------------|--------------------|
| **Filtr dolnoprzepustowy** | Generator → Filtr → Wizualizator wykresów | Zobaczysz przefiltrowany sygnał |
| **Symulowana akwizycja** | Generator → Analizator THD | Wartość zniekształceń harmonicznych sygnału |
| **Kontrola ręczna** | Generator → Inspektor danych | Tabela z wartościami sygnału wysłanego przez generator |
| **Porównanie sygnałów** | Dwa generatory → Sumator → Wizualizator wykresów | Wynik operacji (suma, różnica, mnożenie lub dzielenie) dwóch fal na jednym wykresie |

Każdy z tych przepływów można złożyć w mniej niż minutę, co pokazuje zwinność FloWorks w porównaniu do tradycyjnego pisania kodu.

---

## 📚 Co dalej?

| Zasób | Opis |
|---------|-------------|
| [🗺️ Przewodnik po anatomii interfejsu](interface-anatomy.md) | Zrozumienie architektury i filozofii interfejsu graficznego |
| [🗺️ Mapa kodu i architektury](philosophy.md) | Pełna struktura, menedżerowie, kontrakty i DPI-Awareness. |
| [🧩 Techniczna referencja węzłów](node-reference.md) | Katalog, `ScriptNode`, wielokanałowość i jak rozszerzyć system. |
| [🌐 Przewodnik po internacjonalizacji](translation-guide.md) | Dodawanie języków, walidacja JSON i zarządzanie kluczami `tr()`. |
| [📦 Przewodnik po przenośnym buildzie](guia-ejecutable-portable.md) | PyInstaller, hooki, `--onefile`, rozwiązywanie błędów i podpis cyfrowy. |

---

!!! warning "Uwagi dotyczące kompatybilności i użycia"
    1. **Wersja Pythona:** Możesz używać 3.9+ i systemów 64-bitowych.
    2. **Firewall Windows:** Jeśli używasz rzeczywistego sprzętu (oscyloskop VISA/SCPI), zezwól `FloWorks.exe` w zaporze. Aplikacja wyświetla własne okno dialogowe, jeśli połączenie jest zablokowane (okno systemowe nie pojawia się w trybie `--windowed`).
    3. **Kluczowe skróty:** `F5` (uruchom), `Ctrl+S` (zapisz `.sflow`), `Ctrl+Klik` (połącz), `Spacja+klik` (swobodne przesuwanie), `Ctrl+Shift+L` (auto-layout).
    4. **Zachowanie danych:** Silnik **nigdy** nie stosuje `flatten()` na tablicach. Pracuj na lokalnych kopiach, jeśli potrzebujesz wektoryzacji.
