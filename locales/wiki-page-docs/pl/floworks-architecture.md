---
title: Architektura FloWorks
description: Przegląd komponentów i wewnętrznego działania dla użytkownika końcowego
---

# Architektura FloWorks – Wizja dla użytkownika

FloWorks to aplikacja desktopowa, która pozwala budować łańcuchy przetwarzania sygnałów za pomocą schematów blokowych. Łącz bloki (węzły) na interaktywnej kanwie i oglądaj wyniki w czasie rzeczywistym. Aby to było możliwe, aplikacja jest zorganizowana w kilka modułów, które współpracują ze sobą. Poniżej wyjaśniono, bez szczegółów technicznych, co robi każda część i jak się ze sobą łączą.

---

## Ogólna struktura

Aplikacja składa się z następujących obszarów funkcjonalnych:

| Obszar | Co robi? |
|------|------------|
| **Uruchomienie i okno główne** | Uruchamia program, wyświetla okno, menu i koordynuje wszystkie działania użytkownika. |
| **Silnik wykonawczy** | Oblicza kolejność, w jakiej mają być wykonywane węzły, wykrywa zależności i pętle oraz przesyła dane z jednego węzła do drugiego. |
| **Scena i diagram** | Zarządza płótnem, na którym umieszczasz węzły, połączeniami między nimi, karteczkami i akcjami cofnij/ponów. |
| **Węzły i przetwarzanie** | Zawiera wszystkie typy bloków, których możesz użyć: źródła sygnałów, operacje matematyczne, niestandardowe skrypty, eksport wykresów itp. |
| **Łączniki wizualne** | Rysuje linie łączące węzły (gładkie krzywe lub ortogonalne), animuje je, aby pokazać przepływ danych i zapobiega nakładaniu się. |
| **Interfejs użytkownika** | Obejmuje widok diagramu (powiększenie, przesuwanie), pasek narzędzi, tabelę parametrów, panele analizy (statystyki, kursory) i okna konfiguracyjne. |
| **Obsługa rzeczywistego sprzętu** | Umożliwia komunikację z instrumentami laboratoryjnymi (oscyloskopy, generatory, multimetry LCR) do akwizycji lub generowania rzeczywistych sygnałów. |
| **Eksport wykresów** | Generuje obrazy wysokiej jakości (PNG, PDF, SVG) z pełną personalizacją wizualną. |
| **Motywy i wygląd** | Zmienia wygląd całej aplikacji (ciemny, jasny, wysoki kontrast) i pozwala dostosować rozmiar czcionki. |
| **Języki** | Tłumaczy cały interfejs na wiele języków i umożliwia natychmiastową zmianę języka. |
| **Zarządzanie projektami** | Zapisuje i otwiera pliki `.sflow` z całym diagramem, w tym konfiguracje, skrypty i wyniki. |
| **Testy i diagnostyka** | Wewnętrzne narzędzia do weryfikacji poprawności działania (niewidoczne dla użytkownika końcowego). |

---

## Jak to działa wewnątrz

### Uruchomienie i okno główne
Po otwarciu FloWorks konfigurowane jest środowisko graficzne, wykrywana jest gęstość pikseli ekranu (aby wszystko wyglądało ostro na monitorach 4K i zwykłych) i wyświetlane jest okno główne. To okno centralizuje wszystkie elementy: obszar rysowania, menu, pasek narzędzi i panele boczne.

### Silnik przepływu
Gdy naciśniesz "Wykonaj" (lub F5), wewnętrzny silnik przechodzi przez wszystkie węzły we właściwej kolejności, respektując połączenia. Wie, które węzły zależą od innych i zapobiega cyklom nieskończonym. Obsługuje węzły odbierające wiele nazwanych wejść i produkujące wiele wyjść. Dane podróżują między węzłami bez utraty pierwotnej struktury.

### Scena diagramu
Płótno, na którym budujesz diagramy, to inteligentna scena:

- Pozwala dodawać, przesuwać, łączyć i zaznaczać węzły.
- Obsługuje nieograniczone cofanie i ponawianie dla każdej akcji.
- Zawiera skalowalne karteczki, które możesz dowolnie umieszczać i które są zapisywane z projektem.
- Dysponuje automatycznym organizatorem, który porządkuje węzły (Ctrl+Shift+L).
- Podczas zapisu cały diagram pakuje się do pliku `.sflow`, zawierającego opisy węzłów, połączenia, notatki i powiązane dane numeryczne.

### Łączniki
Linie łączące węzły rysowane są jako gładkie krzywe lub trasy ortogonalne. Delikatna animacja kropek lub kresek wskazuje kierunek przepływu. Menedżer pasów zapobiega nakładaniu się wielu połączeń między tymi samymi węzłami; oddziela je automatycznie, aby wszystko było czytelne.

### Typy węzłów
Węzły to podstawowe elementy składowe. Grupują się w trzy kategorie:

- **Źródła** – Generują sygnały. Mogą symulować fale (sinusoidalna, prostokątna itp.) lub odczytywać rzeczywiste dane z podłączonego oscyloskopu lub multimetru. Obsługują wiele kanałów jednocześnie (np. impedancja i faza z LCR).
- **Przetwarzanie** – Przekształcają dane. Obejmują operacje arytmetyczne (dodawanie, odejmowanie, mnożenie, dzielenie), decyzje warunkowe (rozgałęzienie Tak/Nie) i potężny węzeł skryptowy, który pozwala pisać własny kod Python z pomocą wizualną.
- **Ujścia** – Wyświetlają lub eksportują wyniki. Najczęstszy to wizualizator wykresów (wirtualny oscyloskop), ale istnieje też eksporter wykresów profesjonalnej jakości.

Każdy węzeł ma porty wejściowe (lewo/góra) i wyjściowe (prawo/dół). Połączenie portu wyjściowego z wejściowym powoduje przepływ sygnału między nimi.

#### Zaawansowany węzeł skryptowy
Węzeł skryptowy zasługuje na szczególną wzmiankę. Jest przeznaczony dla zaawansowanych użytkowników, którzy chcą dodać własne przetwarzanie bez opuszczania FloWorks. Oferuje:

- Edytor z podświetlaniem składni, autouzupełnianiem i numeracją wierszy.
- Możliwość definiowania parametrów edytowalnych z panelu węzła bez ingerowania w kod (np. wartość liczbowa używana w skrypcie).
- Dynamiczne porty wejściowe i wyjściowe: dodając specjalne komentarze w skrypcie, możesz tworzyć nowe złącza.
- Pamięć trwałą: specjalna zmienna (`persist`), która zachowuje wartość między wykonaniami, przydatna dla akumulatorów lub automatów stanów.
- Gotowe szablony skryptów i możliwość zapisywania własnych.
- Zintegrowany system pomocy i konsolę wyświetlającą błędy wykonania.

### Interfejs użytkownika
Oprócz płótna interfejs obejmuje:

- **Pasek narzędzi** ze wszystkimi węzłami uporządkowanymi według kategorii, menu języka, motywu i rozmiaru czcionki oraz dostępem do przeglądarki logów.
- **Tabelę parametrów** wyświetlającą informacje o zaznaczonych węzłach i wyróżniającą potencjalne niezgodności (np. próba operacji na sygnałach o różnej długości).
- **Dokowalne panele analizy**: statystyki (maksimum, minimum, wartość skuteczna), kursory A/B do pomiarów różnic oraz celownik ze znacznikiem szczytu.
- **Okno powitalne**, które dostosowuje się do rozdzielczości ekranu i oferuje opcje początkowe.

### Połączenie z rzeczywistymi instrumentami
Jeśli posiadasz kompatybilny sprzęt (oscyloskopy Siglent SDS, multimetry LCR, generatory SDG), FloWorks może z nimi komunikować się przez standardowy protokół VISA/SCPI. Konfiguracja odbywa się z dedykowanych paneli w aplikacji. Gdy przechwytujesz sygnał wielokanałowy (np. moduł i fazę z LCR), węzeł źródłowy pakuje wszystkie kanały, a ty możesz wybrać, który wyświetlić za pomocą prostego menu kontekstowego.

### Profesjonalny eksport wykresów
Eksporter wykresów pozwala generować obrazy gotowe do raportów lub publikacji. Dwukrotne kliknięcie otwiera okno z wieloma opcjami: możesz dostosować kolory, typy linii, etykiety, skale, wybierać między formatami PNG, PDF lub SVG i zapisywać preferencje jako profile wielokrotnego użytku.

### Personalizacja wizualna
FloWorks zawiera kilka motywów (ciemny, jasny, wysoki kontrast), które natychmiast zmieniają wygląd całego interfejsu bez restartu. Dodatkowo możesz dostosować globalny rozmiar czcionki z menu (Informacje → Rozmiar czcionki), a wszystkie elementy przeskalują się odpowiednio, w tym teksty wewnątrz węzłów, karteczki i wykresy.

### System językowy
Aplikacja automatycznie wykrywa język systemowy przy pierwszym uruchomieniu i zapisuje preferencję. Możesz zmienić język w dowolnym momencie z menu; wszystkie teksty, menu i pomoc aktualizują się w locie.

### Projekty i pliki `.sflow`
Cała twoja praca jest zapisywana w jednym pliku z rozszerzeniem `.sflow`. Plik ten zawiera cały diagram: węzły, połączenia, notatki, konfiguracje, skrypty i wygenerowane dane numeryczne. Możesz go udostępniać innym użytkownikom; po otwarciu na innym komputerze notatki i węzły automatycznie przeskalują się do gęstości pikseli tego ekranu.

---

## Typowe przepływy pracy

1. **Utwórz prosty diagram**  
   Wybierz węzeł źródłowy (np. Generator) i węzeł Wizualizator z paska narzędzi.  
   Połącz wyjście generatora z wejściem wizualizera (Ctrl+klik na porcie wyjściowym, potem klik na wejściowym).  
   Naciśnij F5, aby wykonać. Zobaczysz sygnał na wykresie.

2. **Użyj niestandardowego skryptu**  
   Dodaj węze Skrypt.  
   Napisz swój kod Python w edytorze; możesz zdefiniować edytowalne parametry i dodatkowe porty.  
   Połącz jego wejścia i wyjścia jak każdy inny węzeł.  
   Wykonaj przepływ; skrypt zostanie przetworzony z twoimi danymi.

3. **Przechwyć dane z rzeczywistego oscyloskopu**  
   Podłącz instrument i skonfiguruj komunikację z panelu węzła Oscyloskop.  
   Węzeł akwiruje sygnał i dostarcza go przez swoje porty wyjściowe (jeden na kanał).  
   Połącz te porty z innymi węzłami przetwarzania lub z wizualizerem.

4. **Eksportuj wykres do raportu**  
   Podłącz żądany sygnał do węzła Eksporter wykresów.  
   Wybierz w węźle (prawy przycisk myszy), aby skonfigurować wygląd wizualny wykresu.  
   Można też wczytać/zapisać profile, aby przyspieszyć uzyskiwanie wykresów gotowych do raportów i uzyskać plik obrazu w wybranym rozszerzeniu.

---

## Do czego to służy

Ta architektura została zaprojektowana tak, abyś mógł skupić się na analizie sygnałów bez martwienia się o wewnętrzną organizację programu. Każdy komponent ma wyraźną funkcję i współpracuje, aby zapewnić płynne doświadczenie, od symulacji po rzeczywistą instrumentację, przez personalizację wizualną aż po eksport wyników.

Jeśli kiedykolwiek będziesz musiał rozszerzyć możliwości FloWorks (np. dodając nowe typy węzłów lub podłączając inny instrument), wiedz, że istnieje modułowa struktura, która na to pozwala, choć to już domena dla deweloperów. Jako użytkownik końcowy ciesz się elastycznością, jaką daje ci to rozwiązanie.
