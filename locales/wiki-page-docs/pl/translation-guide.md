---
title: Przewodnik po internacjonalizacji (i18n)
description: Instrukcje krok po kroku dotyczące dodawania i zarządzania tłumaczeniami w FloWorks
---

# 🌐 Przewodnik po internacjonalizacji (i18n)

Ten dokument wyjaśnia, jak dodać nowy język do FloWorks i efektywnie zarządzać plikami tłumaczeń.

---

## ➕ Jak dodać nowy język

### Krok 1: Utworzenie pliku JSON
Przejdź do folderu `locales/`. Skopiuj `en.json` i zmień jego nazwę, używając odpowiedniego dwuliterowego kodu [ISO 639-1](https://pl.wikipedia.org/wiki/ISO_639-1) (np. `fr.json` dla francuskiego, `de.json` dla niemieckiego).

### Krok 2: Tłumaczenie ciągów znaków
Otwórz nowy plik JSON w edytorze tekstu.

!!! warning "Nie modyfikuj kluczy"
    **Nigdy nie zmieniaj kluczy** (lewej strony każdej pary). Tłumacz tylko wartości (prawą stronę).

**Oryginał (`en.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "File",
    "edit": "Edit"
  }
}
```

**Przetłumaczony przykład (`es.json`):**
```json
{
  "app_title": "FloWorks",
  "menu": {
    "file": "Archivo",
    "edit": "Editar"
  }
}
```
Upewnij się, że klucz główny `"language_name"` zawiera rodzimą nazwę języka (np. `"Français"`, `"Deutsch"`, `"Español"`).

### Krok 3: Weryfikacja JSON
Sprawdź, czy plik jest prawidłowym JSON-em (bez końcowych przecinków, poprawnych cudzysłowów, odpowiednich escape'ów). Możesz użyć walidatorów online, takich jak [JSONLint](https://jsonlint.com), lub uruchomić:
```bash
python -m json.tool locales/es.json
```

### Krok 4: Testowanie nowego języka
1. Uruchom FloWorks.
2. Przejdź do **INFO → Język** i wybierz nowy język.
3. Sprawdź, czy wszystkie elementy interfejsu aktualizują się natychmiast (menu, panele, okna dialogowe, etykiety węzłów itp.).

### Krok 5: Automatyczne wykrywanie (opcjonalne)
Jeśli ustawienia regionalne systemu użytkownika zgadzają się z kodem nowego języka, FloWorks automatycznie go użyje przy pierwszym uruchomieniu (o ile nie zapisano wcześniej preferencji w `QSettings`).

---

## 🌍 Dostępne języki
- **Angielski** (`en`) – Język podstawowy / rezerwowy
- **Hiszpański** (`es`)

---

## ⚙️ Ważne uwagi i dobre praktyki

!!! info "Mechanizm rezerwowy"
    Językiem podstawowym jest **angielski**. Jeśli w pliku językowym brakuje klucza tłumaczenia, FloWorks automatycznie użyje ciągu znaków w języku angielskim jako rezerwowego.

!!! warning "Zapobieganie przepełnieniu UI"
    Zachowaj zwięzłość tłumaczeń, aby uniknąć uszkodzenia układu. Jeśli przetłumaczony tekst jest znacznie dłuższy, rozważ skrócenie lub poleganie na systemie motywów w celu dynamicznego skalowania.

!!! tip "Zachowanie HTML i placeholderów"
    - **Tagi HTML:** Zachowaj wszystkie tagi HTML dokładnie tak, jak są (np. `<h3>`, `<b>`, `<pre>`, `<br>`).
    - **Placeholdery:** Zachowaj składnię `{variable}` tam, gdzie jest używana (np. `"Idioma cambiado a: {name} ({code})"`). Nie zmieniaj ich kolejności ani nie usuwaj.

---

## 🔗 Powiązana dokumentacja
- [📖 Mapa kodu i architektura](architecture-ii.md)
- [📦 Przewodnik po kompilacji i dystrybucji](build.md)
- [🧩 Dokumentacja węzłów](node-reference.md)
