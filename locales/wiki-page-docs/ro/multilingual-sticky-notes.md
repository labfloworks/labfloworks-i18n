# Note adezive (Sticky Notes) – Ghidul utilizatorului

## Ce sunt notele adezive?

Notele adezive (sau *sticky notes*) sunt mici blocuri de text pe care le puteți plasa liber pe diagramă. Servesc pentru a:

- Adăuga mementouri, titluri sau explicații direct pe Pânză.
- Crea tutoriale pas cu pas care să ghideze utilizatorul proiectului dumneavoastră.
- Documenta părți ale fluxului de lucru fără a părăsi FloWorks.
- Lăsa comentarii pentru dumneavoastră sau pentru alți colaboratori.

Notele pot fi redimensionate (trăgând de colțuri), mutate oriunde pe diagramă și se salvează împreună cu proiectul. La deschiderea unui fișier `.sflow`, toate notele apar exact unde le-ați lăsat.

---

## Noutate: note multilingve

Notele adezive pot afișa automat textul în limba aleasă pentru aplicație.  
În loc să scrieți mesajul final într-o singură limbă, puteți insera **marcaje speciale** care se vor traduce singure la schimbarea limbii FloWorks.

Astfel, o aceeași notă poate fi citită în spaniolă, engleză sau orice altă limbă disponibilă, fără a fi nevoie să editați textul de fiecare dată.

---

## Cum se scrie o notă multilingvă

În interiorul unei note (creați una prin dublu clic sau cu butonul 📝 din Bară de unelte), puteți folosi două tipuri de marcaje:

### 1. Cu cuvântul `tr(…)`
Scrieți `tr("cheie")` și înlocuiți `cheie` cu un nume descriptiv al frazei.

Exemplu:

```
tr("tutorial.paso1.titulo")
tr("tutorial.paso1.mensaje")
```

### 2. Cu acolade duble `{{…}}`
Scrieți `{{cheie}}` în același mod.

Exemplu:

```
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

Ambele formate funcționează la fel; alegeți-l pe cel mai confortabil (le puteți chiar combina în aceeași notă).

> **Important**: Textul pe care îl vedeți când editați nota conține marcajele originale (de ex. `{{tutorial.paso1.titulo}}`).  
> La terminarea editării și revenirea la vizualizarea normală a diagramei, marcajele sunt înlocuite cu fraza tradusă în limba curentă a aplicației.

---

## Comportament la schimbarea limbii

- Dacă schimbați limba din meniul FloWorks (de ex. din spaniolă în engleză), **toate notele adezive care conțin marcaje se actualizează automat**.
- Nu este necesar să închideți și să redeschideți proiectul, nici să editați manual fiecare notă.
- Notele care conțin doar text normal (fără marcaje) nu sunt afectate; arată la fel în orice limbă.

---

## Avantajele utilizării marcajelor

- **Tutoriale multilingve instantanee** – O singură notă poate servi drept ghid pentru utilizatori de diferite limbi.
- **Consistență** – Dacă modificați traducerea într-un singur loc (fișierul de limbi pe care echipa de dezvoltare îl întreține), toate notele care folosesc acea cheie se vor actualiza.
- **Întreținere simplă** – Puteți scrie conținutul o singură dată și îl reutiliza în mai multe note.
- **Flexibilitate** – Combinați text fix cu marcaje. De exemplu:

```
🎯 PASUL 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

---

## Exemplu practic: tutorial pas cu pas

Să presupunem că doriți să adăugați o notă care explică primul pas al unui tutorial.  
În modul de editare scrieți:

```
🎯 PASUL 1
{{tutorial.paso1.titulo}}
{{tutorial.paso1.mensaje}}
```

După terminarea editării și utilizarea aplicației în spaniolă, veți vedea:

```
🎯 PASO 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.
```

Dacă schimbați limba în engleză, aceeași notă va afișa:

```
🎯 STEP 1
Welcome to FloWorks!
Drag a signal source node to begin.
```

Și așa mai departe pentru orice altă limbă configurată.

---

## Rezumat

- Notele adezive îmbogățesc diagramele cu informații textuale.
- Acum pot fi **multilingve** folosind marcajele `tr("cheie")` sau `{{cheie}}`.
- La editare vedeți cheile; la vizualizare, textul tradus.
- Schimbați limba aplicației și toate notele se vor adapta instantaneu.
- Perfecte pentru crearea de documentație vizuală, tutoriale sau alerte care trebuie să funcționeze în mai multe limbi.

Profitați de această funcționalitate pentru a face proiectele dumneavoastră mai accesibile și mai ușor de partajat!
