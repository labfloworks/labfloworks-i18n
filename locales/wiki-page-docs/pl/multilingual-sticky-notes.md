# Karteczki (Sticky Notes) – Podręcznik użytkownika

## Czym są karteczki?

Karteczki (lub *sticky notes*) to małe bloki tekstowe, które można swobodnie umieszczać na diagramie. Służą do:

- Dodawania przypomnień, tytułów lub wyjaśnień bezpośrednio na Płótnie.
- Tworzenia samouczków krok po kroku, które prowadzą użytkownika przez projekt.
- Dokumentowania części przepływu pracy bez konieczności wychodzenia z FloWorks.
- Zostawiania komentarzy dla siebie lub innych współpracowników.

Karteczki można zmieniać rozmiar (przeciągając rogi), przesuwać w dowolne miejsce diagramu i są zapisywane razem z projektem. Po otwarciu pliku `.sflow` wszystkie karteczki pojawiają się dokładnie tam, gdzie zostały zostawione.

---

## Nowość: wielojęzyczne karteczki

Karteczki mogą automatycznie wyświetlać tekst w języku wybranym dla aplikacji.  
Zamiast pisać ostateczną wiadomość w jednym języku, można wstawić **specjalne znaczniki**, które przy zmianie języka FloWorks przetłumaczą się automatycznie.

Dzięki temu ta sama karteczka może być czytana po hiszpańsku, angielsku lub w innym dostępnym języku bez konieczności edytowania tekstu za każdym razem.

---

## Jak pisać wielojęzyczną karteczkę

Wewnątrz karteczki (utwórz ją dwuklikiem lub przyciskiem 📝 na Pasku narzędzi) można używać dwóch typów znaczników:

### 1. Za pomocą słowa `tr(…)`
Wpisz `tr("klucz")` i zastąp `klucz` opisową nazwą frazy.

Przykład:

```
tr("tutorial.paso1.titulo")
tr("tutorial.paso1.mensaje")
```

### 2. Za pomocą podwójnych nawiasów klamrowych `{{…}}`
Wpisz `{{klucz}}` w ten sam sposób.

Przykład:

```
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Obie formy działają identycznie; wybierz wygodniejszą (możesz je nawet łączyć w tej samej karteczce).

> **Ważne**: Tekst widoczny podczas edycji karteczki zawiera oryginalne znaczniki (np. `{{tutorial.paso1.titulo}}`).  
> Po zakończeniu edycji i powrocie do normalnego widoku diagramu znaczniki zostaną zastąpione frazą przetłumaczoną na aktualny język aplikacji.

---

## Zachowanie przy zmianie języka

- Jeśli zmienisz język z menu FloWorks (np. z hiszpańskiego na angielski), **wszystkie karteczki zawierające znaczniki zaktualizują się automatycznie**.
- Nie ma potrzeby zamykania i ponownego otwierania projektu, ani ręcznej edycji każdej karteczki.
- Karteczki zawierające tylko zwykły tekst (bez znaczników) nie są dotknięte; wyświetlają to samo w każdym języku.

---

## Zalety użycia znaczników

- **Natychmiastowe wielojęzyczne samouczki** – Jedna karteczka może prowadzić użytkowników różnych języków.
- **Spójność** – Jeśli zmodyfikujesz tłumaczenie w jednym miejscu (w pliku językowym zarządzanym przez zespół), wszystkie karteczki używające tego klucza zostaną zaktualizowane.
- **Łatwa konserwacja** – Możesz napisać treść raz i użyć jej wielokrotnie w różnych karteczkach.
- **Elastyczność** – Łącz stały tekst ze znacznikami. Na przykład:

```
🎯 KROK 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

---

## Praktyczny przykład: samouczek krok po kroku

Załóżmy, że chcesz dodać karteczkę wyjaśniającą pierwszy krok samouczka.  
W trybie edycji wpisujesz:

```
🎯 KROK 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Po zakończeniu edycji i korzystaniu z aplikacji po hiszpańsku zobaczysz:

```
🎯 PASO 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.
```

Jeśli zmienisz język na angielski, ta sama karteczka wyświetli:

```
🎯 STEP 1
Welcome to FloWorks!
Drag a signal source node to begin.
```

I tak dalej dla dowolnego innego skonfigurowanego języka.

---

## Podsumowanie

- Karteczki wzbogacają diagramy o informacje tekstowe.
- Teraz mogą być **wielojęzyczne** dzięki znacznikom `tr("klucz")` lub `{{klucz}}`.
- W trybie edycji widzisz klucze; w trybie podglądu przetłumaczony tekst.
- Zmień język aplikacji, a wszystkie karteczki natychmiast się dostosują.
- Idealne do tworzenia wizualnej dokumentacji, samouczków lub powiadomień, które mają działać w wielu językach.

Wykorzystaj tę funkcjonalność, aby Twoje projekty były bardziej dostępne i łatwiejsze do udostępniania!
