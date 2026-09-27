# Filozofia

## Widok ogólny

FloWorks to aplikacja desktopowa, która pozwala tworzyć łańcuchy przetwarzania sygnałów za pomocą wizualnych schematów blokowych.
Przeciągaj, łącz i konfiguruj węzły; wynik jest obliczany i wyświetlany w czasie rzeczywistym.
Pracuj z sygnałami symulowanymi lub podłącz prawdziwe instrumenty (oscyloskopy, generatory, multimetry LCR) bez konieczności pisania kodu, choć masz do dyspozycji potężne środowisko skryptowe, jeśli chcesz rozszerzyć funkcjonalność.

---

## Główne cechy

- **Interaktywne diagramy** – Zbuduj swój przepływ pracy łącząc węzły liniami reprezentującymi przepływ danych.
- **Przetwarzanie w czasie rzeczywistym** – Każda modyfikacja jest natychmiast odzwierciedlana na wykresach i wizualizacjach.
- **Symulacja i prawdziwy sprzęt** – Generuj sygnały testowe lub rejestruj dane bezpośrednio z instrumentów laboratoryjnych.
- **Zaawansowany węzeł skryptowy** – Włącz własny kod Python z pomocą autouzupełniania, edytowalnych parametrów dynamicznych i pamięci między wykonaniami.
- **Profesjonalna wizualizacja** – Sygnały, widma, spektrogramy i wykresy wysokiej jakości gotowe do eksportu.
- **Wielojęzyczność** – Interfejs wykrywa język systemu i pozwala przełączać się między hiszpańskim, angielskim i innymi językami w dowolnym momencie.
- **Motywy wizualne** – Tryb ciemny, jasny i wysoki kontrast, aby dostosować się do Twoich preferencji lub potrzeb dostępności.
- **Kompleksowe zarządzanie projektami** – Zapisz swoją pracę w plikach `.sflow` i odzyskaj ją dokładnie tak, jak ją zostawiłeś, z nieograniczonym cofaniem i ponawianiem.

---

## Jak pracować z FloWorks

### Węzły
Węzeł to element przetwarzania. Są zorganizowane w trzy kategorie:

- **Źródła** – Wprowadzają sygnały na początek przepływu. Na przykład oscyloskop (prawdziwy lub symulowany), generator funkcji lub operacja matematyczna.
- **Przetwarzanie** – Przekształcają dane. Sumy, różnice, warunki, filtry… w tym specjalny węzeł do pisania własnych skryptów w Pythonie.
- **Ujścia** – Wyświetlają lub eksportują wyniki. Wizualizator wykresów i profesjonalny eksporter grafów są najczęściej używane.

### Połączenia
Połączenia między węzłami rysowane są jako gładkie krzywe lub linie ortogonalne. Animacja przepływu wskazuje w każdej chwili kierunek danych. System automatycznie organizuje przewody, aby się nie nakładały.

### Wizualizacja
Za każdym razem, gdy węzeł produkuje sygnał, można go zobaczyć w zintegrowanym panelu wykresów. Możesz eksplorować różne reprezentacje (kształt fali, widmo, spektrogram) i dostosowywać skalę za pomocą myszy.

---

## Wyróżnione węzły
Są to minimalne niezbędne węzły, konieczne, aby filozofia programu miała sens.

### Węzeł generatora sygnałów
Źródło sygnałów, które może generować symulacje dowolnych przebiegów zdefiniowanych przez użytkownika. Pozwala z menu kontekstowego wybrać lub wpisać wymagany przebieg.

### Węzeł skryptowy
Kompletne środowisko programistyczne wewnątrz diagramu:

- **Editor z podświetlaniem składni**, autouzupełnianiem i konsolą błędów.
- **Parametry dynamiczne** – Definiuj edytowalne zmienne z panelu węzła bez modyfikowania kodu.
- **Konfigurowalne porty** – Dodawaj dodatkowe wejścia i wyjścia bezpośrednio z edytora.
- **Stan trwały** – Zapisuj wartości między wykonaniami; wszystko jest przechowywane wraz z projektem.

### Eksporter wykresów
Węzeł ujścia, który generuje obrazy wysokiej jakości do raportów lub publikacji. Pozwala konfigurować rozmiar, rozdzielczość, format i inne.

---

## Personalizacja

- **Język** – Aplikacja automatycznie wykrywa język systemu i zapisuje Twoje preferencje. Możesz go zmienić z menu bez ponownego uruchamiania.
- **Wygląd** – Wybierz między ciemnym, jasnym lub wysokokontrastowym motywem w zależności od oświetlenia otoczenia lub swoich potrzeb wizualnych.

---

## Projekty i pliki

Zapisz swój kompletny diagram w pliku `.sflow`.
Po otwarciu odzyskasz wszystkie węzły, połączenia, skrypty, parametry i konfiguracje wizualizacji.
Akcje cofania i ponawiania pozwalają eksperymentować bez obawy utraty poprzedniej pracy.

---

FloWorks został zaprojektowany tak, abyś mógł skupić się na analizie sygnałów, a nie na szczegółach technicznych implementacji. Przeciągaj, łącz i odkrywaj.
