# Lepicí poznámky (Sticky Notes) – Uživatelská příručka

## Co jsou lepicí poznámky?

Lepicí poznámky (nebo *sticky notes*) jsou malé textové bloky, které můžete volně umisťovat na diagram. Slouží k:

- Přidání upomínek, nadpisů nebo vysvětlivek přímo na Plátno.
- Vytvoření tutoriálů krok za krokem, které provedou uživatele vaším projektem.
- Dokumentování částí pracovního toku bez nutnosti opustit FloWorks.
- Zanechání komentářů pro sebe nebo pro ostatní spolupracovníky.

Poznámky lze měnit velikost (táhnutím rohů), přesouvat kamkoli na diagram a ukládají se spolu s projektem. Po otevření souboru `.sflow` se všechny poznámky zobrazí přesně tam, kde jste je nechali.

---

## Novinka: vícejazyčné poznámky

Lepicí poznámky mohou automaticky zobrazovat text v jazyce, který si zvolíte pro aplikaci.  
Místo psaní konečné zprávy v jednom jazyce můžete vložit **speciální značky**, které se při změně jazyka FloWorks automaticky přeloží.

Takže jedna poznámka může být čtena ve španělštině, angličtině nebo jakémkoli jiném dostupném jazyce, aniž byste museli pokaždé upravovat text.

---

## Jak psát vícejazyčnou poznámku

Uvnitř poznámky (vytvořte ji dvojklikem nebo tlačítkem 📝 na Panelu nástrojů) můžete použít dva typy značek:

### 1. Pomocí slova `tr(…)`
Napište `tr("klíč")` a nahraďte `klíč` popisným názvem fráze.

Příklad:

```
tr("tutorial.paso1.titulo")
tr("tutorial.paso1.mensaje")
```

### 2. Pomocí dvojitých složených závorek `{{…}}`
Napište `{{klíč}}` stejným způsobem.

Příklad:

```
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Obě formy fungují stejně; vyberte si pohodlnější (dokonce je můžete v jedné poznámce kombinovat).

> **Důležité**: Text, který vidíte při úpravách poznámky, obsahuje původní značky (např. `{{tutorial.paso1.titulo}}`).  
> Po dokončení úprav a návratu do normálního zobrazení diagramu se značky nahradí frází přeloženou do aktuálního jazyka aplikace.

---

## Chování při změně jazyka

- Pokud změníte jazyk z nabídky FloWorks (např. ze španělštiny na angličtinu), **všechny lepicí poznámky obsahující značky se automaticky aktualizují**.
- Není nutné projekt zavřít a znovu otevřít, ani upravovat každou poznámku ručně.
- Poznámky, které obsahují pouze běžný text (bez značek), nejsou ovlivněny; zobrazují se stejně v jakémkoli jazyce.

---

## Výhody použití značek

- **Okamžité vícejazyčné tutoriály** – Jedna poznámka může sloužit jako průvodce pro uživatele různých jazyků.
- **Konzistence** – Pokud upravíte překlad na jednom místě (v jazykovém souboru, který spravuje váš vývojový tým), všechny poznámky používající tento klíč se aktualizují.
- **Jednoduchá údržba** – Můžete obsah napsat jednou a znovu použít ve více poznámkách.
- **Flexibilita** – Kombinujte pevný text se značkami. Například:

```
🎯 KROK 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

---

## Praktický příklad: tutoriál krok za krokem

Předpokládejme, že chcete přidat poznámku vysvětlující první krok tutoriálu.  
V režimu úprav napíšete:

```
🎯 KROK 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Po dokončení úprav a při používání aplikace ve španělštině uvidíte:

```
🎯 PASO 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.
```

Pokud změníte jazyk na angličtinu, stejná poznámka zobrazí:

```
🎯 STEP 1
Welcome to FloWorks!
Drag a signal source node to begin.
```

A tak dále pro jakýkoli jiný nakonfigurovaný jazyk.

---

## Shrnutí

- Lepicí poznámky obohacují diagramy o textové informace.
- Nyní mohou být **vícejazyčné** pomocí značek `tr("klíč")` nebo `{{klíč}}`.
- Při úpravách vidíte klíče; při prohlížení přeložený text.
- Změňte jazyk aplikace a všechny poznámky se okamžitě přizpůsobí.
- Ideální pro tvorbu vizuální dokumentace, tutoriálů nebo upozornění, která mají fungovat ve více jazycích.

Využijte tuto funkci k tomu, aby byly vaše projekty přístupnější a snadněji sdílitelné!
