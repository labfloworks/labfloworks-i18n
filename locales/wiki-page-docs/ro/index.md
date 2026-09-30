---
title: FloWorks
description: Laborator vizual universal pentru procesarea semnalelor, instrumentația științifică și automatizare.
---

<div style="text-align: center; margin: 1em 0;">
  <img src="../assets/FloWorks.svg" alt="FloWorks" style="width: 60%; max-width: 600px; height: auto;">
</div>

<div class="hero-section" markdown>

## Laborator vizual universal pentru semnale, instrumentație și IA

Procesare științifică • DSP • VISA/SCPI • Automatizare • Machine Learning

![Captură de ecran FloWorks](assets/screenshot.PNG){ .hero-image }

<div class="hero-buttons" markdown>

[Primii pași cu FloWorks](getting-started.md){ .md-button }
[Anatomia interfeței](interface-anatomy.md){ .md-button .md-button--primary }
[Filozofie](philosophy.md){ .md-button .md-button--primary }

</div>
</div>

---

## Ce este FloWorks?
FloWorks este un **laborator vizual open-source** (Python + PySide6) unde construiești sisteme conectând blocuri (noduri) în loc să scrii linii de cod.

Imaginează-ți o Pânză digitală unde unești generatoare de semnale, filtre matematice, controlere hardware (VISA/SCPI) și modele de Inteligență Artificială prin cabluri virtuale. Totul se bazează pe **fluxul de date**: conectezi ieșirea unui bloc cu intrarea altuia pentru a procesa informații, automatiza echipamente sau analiza rezultate în timp real.

Este orientat către studenți, cercetători, ingineri și orice persoană care dorește să experimenteze, să învețe sau să realizeze prototipuri de sisteme complexe într-un mod intuitiv, fără bariera programării tradiționale.

### Misiune
Centralizarea fluxului de lucru experimental într-un singur instrument vizual, deschis și accesibil. Dorim ca utilizatorii să se concentreze pe *experimentare și descoperire*, nu pe lupta cu complexitatea software-ului sau costurile licențelor.

### Viziune
O lume în care singura barieră dintre o idee experimentală și execuția acesteia este curiozitatea experimentatorului. FloWorks aspiră să devină platforma de referință pentru știință și tehnică, construită de și pentru comunitatea globală, eliminând zidurile instrumentelor private.

### Principii
* **Libertate Totală:** Cunoștințele și instrumentele trebuie să fie accesibile tuturor. FloWorks este gratuit de utilizat și este dedicat unui nucleu deschis și extensibil.
* **Extensibilitate Infinită:** Dacă lipsește un bloc, oricine îl poate crea și integra în ecosistem folosind Python.
* **Transparență Vizuală:** Fiecare pas al procesului poate fi inspectat, depanat și înțeles grafic.
* **Conexiune cu Lumea Reală:** Nu este doar simulare; permite controlul instrumentației științifice reale direct de pe Pânză.

Spre deosebire de instrumentele închise sau foarte specializate, FloWorks este proiectat ca un ecosistem modular extensibil în care fiecare componentă este un nod reutilizabil și conectabil.

---

## Capacități principale

<div class="grid cards" markdown>

-   **:material-puzzle-outline: Ecosistem extensibil de noduri**

    Catalog tehnic organizat pe straturi: Surse, Procesare, Control, Hardware și Scripting.

    Înregistrare dinamică, serializare declarativă și contracte clare pentru dezvoltare rapidă.

    [:material-arrow-right: Referință noduri](node-reference.md)

-   **:material-connection: Integrare VISA/SCPI**

    Conexiune directă cu osciloscoape, metru LCR și generatoare.

    Suport multicanal, simulare integrată prin `PyVISA-py` și gestionare firewall în mod portabil.

    [:material-arrow-right: Instrumentație](instrumentation.md)

-   **:material-package-variant-closed: Format portabil `.sflow`**

    Standard ZIP autoconținut cu graf JSON, array-uri `.npy` și metadate.

    Reproductibilitate totală a experimentelor și normalizare DPI automată.

    [:material-arrow-right: Format .sflow](sflow-format.md)

-   **:material-translate: Internaționalizare avansată**

    Schimbarea limbii în timp real fără a reporni aplicația.

    Traduceri JSON ierarhice și persistența preferințelor.

    [:material-arrow-right: Ghid i18n](translation-guide.md)

-   **:material-tools: SDK și dezvoltare rapidă**

    Șablon de bază (`template_node.py`), mixin de serializare și ghiduri pas cu pas.

    Arhitectură pregătită pentru pluginuri și extindere comunitară.

    [:material-arrow-right: Creare noduri](adding-a-new-node.md)

</div>

---

## Domenii de aplicare

| Domeniu | Aplicații |
|---------|-----------|
| 🎓 **Educație** | Fizică, electronică, matematică, laboratoare STEM |
| ⚙️ **Inginerie** | DSP, control, instrumentație, metrologie |
| 🤖 **IA** | ML, optimizare, pipeline-uri hibride |
| 🔬 **Cercetare** | Automatizare și achiziție de date |
| 🔌 **Hardware** | VISA/SCPI, simulare și sisteme hibride |

---

!!! tip "Nou în FloWorks?"

    Începe cu secțiunea **Primii pași cu FloWorks**, apoi **Anatomia interfeței** pentru a înțelege arhitectura interfeței grafice și, în final, explorează **Arhitectura Generală** pentru a înțelege fluxul de date și structura motorului topologic.

---

<div markdown="1" style="text-align: center;">

## FloWorks

Procesare vizuală • Instrumentație • Știință • IA

<small>Documentație construită cu MkDocs Material</small>

</div>
