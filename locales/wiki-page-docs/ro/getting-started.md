---
title: Primii pași cu FloWorks
description: Ghid rapid pentru configurarea mediului, rularea primului flux și accesarea versiunii portabile.
---

# 🚀 Primii pași cu FloWorks

Acest ghid te va conduce de la zero până la rularea primului tău flux de procesare a semnalelor. FloWorks este o aplicație de diagrame de flux pentru semnale, construită cu Python și PySide6, care suportă hardware real (VISA/SCPI), simulare integrată, scripting avansat și schimbarea limbii în timp real.

---

## 🌊 Primul tău flux exemplu

Să creăm un flux simplu: generăm un semnal sinusoidal și îl vizualizăm în timp real.

1. **Adăugarea nodurilor**
   În bara de instrumente de sus, selectează `Sursă` → selectează `Generator avansat de semnale`. Apoi, din `Prelucrare` → selectează, de exemplu, `Spectral`.
2. **Conectarea**
   Apasă `Ctrl+Clic` pe portul de ieșire (`dreapta`) al generatorului. Apoi, fă clic pe portul de intrare (`stânga`) al osciloscopului. Sau pur și simplu dă clic pe portul de ieșire și trage (ținând apăsat) până la portul de intrare al nodului următor.
3. **Configurarea (opțional)**
   Fă clic pe un nod; în panoul lateral stânga va apărea un editor cu parametrii nodului selectat pentru ajustarea condițiilor de operare. În partea de jos există o vizualizare grafică care reprezintă vizual datele generate sau achiziționate de noduri.
4. **Rularea**
   Apasă `F5` sau butonul ▶ din bara de instrumente. Motorul topologic va calcula ordinea de execuție, va procesa datele și vei vedea unda în panoul grafic. **Conectorul va fi animat indicând fluxul activ!**

---

## 🧠 Înțelegerea porturilor: categorii după culoare

În FloWorks, fiecare port aparține unei **categorii funcționale** identificate printr-o culoare. Conexiunile valide se fac **întotdeauna între porturi de aceeași culoare**: o ieșire dintr-o categorie se conectează exclusiv cu o intrare din aceeași categorie. În plus, linia conectorului preia automat culoarea porturilor pe care le unește, facilitând citirea vizuală.

| Tip | Culoare | Scop | Exemplu tipic |
|------|-------|-----------|----------------|
| `control` | Alb | Flux de control / activare. | Semnal de pornire către un nod de achiziție. |
| `exec` | Gri | Execuția operațiilor sau pașilor. | Declanșarea unei funcții sau callback. |
| `data` | Verde | Date generice / semnale numerice. | Ieșirea unui generator sau senzor. |
| `int` | Albastru | Numere întregi. | Index, dimensiune buffer, ID. |
| `float` | Cyan | Numere în virgulă mobilă. | Amplitudine, frecvență, prag. |
| `string` | Mov | Șiruri de text. | Nume fișier, etichetă. |
| `bool` | Roz | Valori booleene (`True`/`False`). | Flag de stare, activare. |
| `array` | Albastru închis | Tablouri / vectori. | Semnal multicanal, listă de eșantioane. |
| `trigger` | Portocaliu | Declanșatoare / evenimente discrete. | Puls de sincronizare, front. |

**Regula de aur:**

- Se conectează doar porturi de **aceeași culoare exactă** (ieșire ↔ intrare din aceeași categorie).
- Sistemul previne conexiuni invalide și evidențiază vizual porturile compatibile la tragere.
- Linia conectorului preia culoarea porturilor conectate; astfel fiecare rută se identifică dintr-o privire.

**Filozofia FloWorks:**
Porturile de date **păstrează dimensionalitatea** tablourilor. Nu se aplică niciodată aplatizare automată: dacă intră o matrice, iese o matrice, menținând integritatea semnalelor tale multidimensionale.

![FloWorks](assets/tipos_de_puertos.PNG)

---

## 🖱️ Navigarea pe pânză (Canvas)

Stăpânește spațiul de lucru cu aceste gesturi:

| Acțiune | Cum se face |
|--------|--------------|
| **Zoom** | Rotița mouse-ului sau `Ctrl + rotiță` |
| **Pan (deplasare)** | Ține apăsat `Spațiu` și trage, sau folosește butonul din mijloc al mouse-ului |
| **Selectare nod** | Clic stânga pe nod |
| **Selecție multiplă** | Trage un dreptunghi cu clic stânga, sau `Ctrl + clic` pe mai multe noduri |
| **Mutare selecție** | Trage oricare dintre nodurile selectate |
| **Deschidere configurare** | Dublu clic pe nod |

**Sfat:** Panoul din stânga se actualizează automat cu configurația nodului selectat, fără a deschide ferestre suplimentare.

---

## ⚡ Scurtături de tastatură și mișcări avansate

Aceste scurtături transformă un utilizator normal într-un **utilizator avansat**:

| Scurtătură | Acțiune |
|-------|--------|
| `F5` | Rulează fluxul |
| `Ctrl + S` | Salvează proiectul (`.sflow`) |
| `Ctrl + Clic` | Conectează noduri (clic pe port de ieșire → clic pe port de intrare) |
| `Ctrl + C` / `Ctrl + V` | Copiază / lipește nodurile selectate |
| `Ctrl + Z` / `Ctrl + Y` | Anulează / refă |
| `Ctrl + Shift + L` | Auto-organizează nodurile pe pânză |
| `Del` | Elimină nodurile selectate |
| `Ctrl + A` | Selectează toate nodurile |

**Mișcări avansate:**

- **Duplicare flux:** selectează un grup de noduri, `Ctrl + C`, `Ctrl + V` și trage copia în altă zonă.
- **Curățare grilă:** folosește `Ctrl + Shift + L` pentru a ordona întreaga pânză cu o singură comandă.
- **Conectare rapidă:** `Ctrl + Clic` pe un port de ieșire, apoi clic normal pe portul de intrare; FloWorks desenează conexiunea automat.

---

## 🎨 Personalizarea mediului

FloWorks se adaptează ție, nu invers.

### Schimbare temă în timp real
Din bara de sus, meniul **Vizualizare → Temă**, alege între deschis, întunecat și altele. Interfața se schimbă **instantaneu**, fără repornire și fără pierderea fluxului de lucru.

### Dimensiune font
În **Vizualizare → Dimensiune font** selectează o valoare predefinită sau una personalizată. Întreaga interfață se ajustează pe loc.

### Limbă
În **Vizualizare → Limbă** selectează limba dorită. FloWorks suportă **schimbare în timp real**: meniurile, butoanele și mesajele se traduc fără repornirea aplicației.

---

## ❗ Rezolvarea problemelor comune

| Problemă | Cauză posibilă | Soluție |
|----------|---------------|----------|
| Fluxul nu rulează | Există noduri neconfigurate sau conexiuni rupte | Verifică că toate nodurile au parametri valizi și că conexiunile sunt între porturi compatibile |
| Graficul nu se actualizează | Fluxul este în pauză sau nu curg date | Asigură-te că ai apăsat `F5` sau ▶, și că nodurile sursă generează date |
| Nu pot conecta două noduri | Porturile sunt de tip diferit | Verifică că ambele porturi sunt de **date** sau ambele de **control** |
| Programul merge încet cu fluxuri mari | Prea multe noduri sau grafice în timp real | Închide panouri de analiză nefolosite sau redu frecvența de eșantionare a nodurilor sursă |
| Temă nu se schimbă | Unele widgeturi pot să nu fie înregistrate | Repornește aplicația și încearcă din nou (în versiunile viitoare va fi rezolvat) |

---

## 🧪 Exemple practice rapide

Pe lângă fluxul sinusoidal inițial, încearcă aceste mini-proiecte pentru a stăpâni FloWorks:

| Exemplu | Noduri implicate | Rezultat așteptat |
|---------|-------------------|--------------------|
| **Filtru trece-jos** | Generator → Filtru → Vizualizator grafice | Vei vedea semnalul filtrat |
| **Achiziție simulată** | Generator → Analizor THD | Valoarea distorsiunii armonice a semnalului |
| **Control manual** | Generator → Inspector date | Tabel cu valorile semnalului trimis de generator |
| **Comparare semnale** | Doi generatori → Sumator → Vizualizator grafice | Rezultatul operației (sumă, diferență, înmulțire sau împărțire) a două unde într-un singur grafic |

Fiecare dintre aceste fluxuri poate fi asamblat în mai puțin de un minut, demonstrând agilitatea FloWorks față de codul tradițional.

---

## 📚 Ce urmează?

| Resursă | Descriere |
|---------|-------------|
| [🗺 Ghid de Anatomie a Interfeței](interface-anatomy.md) | Înțelegerea arhitecturii și filozofiei interfeței grafice |
| [🗺️ Harta Codului și Arhitecturii](philosophy.md) | Structura completă, manageri, contracte și DPI-Awareness. |
| [🧩 Referință Tehnică a Nodurilor](node-reference.md) | Catalog, `ScriptNode`, multicanal și cum se extinde sistemul. |
| [🌐 Ghid de Internaționalizare](translation-guide.md) | Adăugarea limbilor, validarea JSON și gestionarea cheilor `tr()`. |
| [📦 Ghid de Build Portabil](guia-ejecutable-portable.md) | PyInstaller, hook-uri, `--onefile`, rezolvarea erorilor și semnătura digitală. |

---

!!! warning "Note de compatibilitate și utilizare"
    1. **Versiune Python:** Poți folosi 3.9+ și sisteme pe 64 de biți.
    2. **Firewall Windows:** Dacă folosești hardware real (osciloscop VISA/SCPI), permite `FloWorks.exe` în firewall. Aplicația afișează un dialog personalizat dacă conexiunea este blocată (dialogul SO nu apare în modul `--windowed`).
    3. **Scurtături cheie:** `F5` (rulează), `Ctrl+S` (salvează `.sflow`), `Ctrl+Clic` (conectează), `Spațiu+clic` (pan liber), `Ctrl+Shift+L` (auto-layout).
    4. **Păstrarea datelor:** Motorul **niciodată** nu aplică `flatten()` pe tablouri. Lucrează cu copii locale dacă ai nevoie de vectorizare.
