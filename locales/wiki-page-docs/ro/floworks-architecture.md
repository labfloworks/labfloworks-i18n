---
title: Arhitectura FloWorks
description: Privire de ansamblu asupra componentelor și funcționării interne pentru utilizatorul final
---

# Arhitectura FloWorks – Viziune pentru utilizator

FloWorks este o aplicație desktop care vă permite să construiți lanțuri de procesare a semnalelor prin intermediul diagramelor de flux. Conectați blocuri (noduri) pe o pânză interactivă și vizualizați rezultatele în timp real. Pentru a face acest lucru posibil, aplicația este organizată în mai multe module care lucrează împreună. Mai jos se explică, fără detalii tehnice, ce face fiecare parte și cum se relaționează între ele.

---

## Structura generală

Aplicația se compune din următoarele zone funcționale:

| Zonă | Ce face? |
|------|------------|
| **Pornire și fereastra principală** | Pornește programul, afișează fereastra, meniurile și coordonează toate acțiunile utilizatorului. |
| **Motor de execuție** | Calculează ordinea în care trebuie executate nodurile, detectează dependențele și buclele și transmite datele de la un nod la altul. |
| **Scenă și diagramă** | Gestionează pânza unde plasați nodurile, conexiunile dintre ele, notele lipicioase și acțiunile de anulare/refacere. |
| **Noduri și procesare** | Conține toate tipurile de blocuri pe care le puteți folosi: surse de semnal, operații matematice, scripturi personalizate, export de grafice etc. |
| **Conectori vizuali** | Desenează liniile care unesc nodurile (curbe netede sau ortogonale), le animează pentru a arăta fluxul de date și evită suprapunerile. |
| **Interfață utilizator** | Include vizualizarea diagramei (zoom, deplasare), bara de instrumente, tabelul de parametri, panourile de analiză (statistici, cursoare) și dialogurile de configurare. |
| **Suport hardware real** | Permite comunicarea cu instrumente de laborator (osciloscoape, generatoare, multimetre LCR) pentru capturarea sau generarea de semnale reale. |
| **Export de grafice** | Generează imagini de înaltă calitate (PNG, PDF, SVG) cu personalizare vizuală completă. |
| **Teme și aspect** | Schimbă aspectul întregii aplicații (întunecat, deschis, contrast ridicat) și permite ajustarea dimensiunii fontului. |
| **Limbi** | Traduce întreaga interfață în mai multe limbi și permite schimbarea limbii instantaneu. |
| **Gestionare proiecte** | Salvează și deschide fișiere `.sflow` cu întreaga diagramă, inclusiv configurațiile, scripturile și rezultatele. |
| **Teste și diagnosticare** | Instrumente interne pentru verificarea funcționării corecte (nevăzibile pentru utilizatorul final). |

---

## Cum funcționează în interior

### Pornire și fereastra principală
La deschiderea FloWorks, se configurează mediul grafic, se detectează densitatea de pixeli a ecranului (astfel încât totul să arate clar pe monitoare 4K sau normale) și se afișează fereastra principală. Această fereastră centralizează toate elementele: zona de desen, meniurile, bara de instrumente și panourile laterale.

### Motor de flux
Când apăsați "Execută" (sau apăsați F5), un motor intern parcurge toate nodurile în ordinea corectă, respectând conexiunile. Știe ce noduri depind de altele și evită ciclurile infinite. Suportă ca un nod să primească mai multe intrări denumite și să producă ieșiri multiple. Datele circulă între noduri fără a-și pierde structura originală.

### Scena diagramei
Pânza unde construiți diagramele este o scenă inteligentă:

- Permite adăugarea, mutarea, conectarea și selectarea nodurilor.
- Suportă anulare și refacere nelimitată pentru orice acțiune.
- Include note lipicioase redimensionabile pe care le puteți plasa liber și care se salvează cu proiectul.
- Dispune de un organizator automat care rearanjează nodurile ordonat (cu Ctrl+Shift+L).
- La salvare, întreaga diagramă se împachetează într-un fișier `.sflow` care conține descrierile nodurilor, conexiunile, notele și datele numerice asociate.

### Conectori
Liniile care unesc nodurile se desenează ca curbe netede sau traiectorii ortogonale. O animație subtilă de puncte sau cratime indică direcția fluxului. Un manager de benzi evită ca mai multe conexiuni între aceleași noduri să se înghesuie; le separă automat pentru ca totul să fie lizibil.

### Tipuri de noduri
Nodurile sunt piesele fundamentale. Se grupează în trei categorii:

- **Surse** – Generează semnale. Pot simula unde (sinusoidală, pătratică etc.) sau citi date reale de la un osciloscop sau multimetru conectat. Suportă multiple canale simultane (de exemplu, impedanță și fază de la un LCR).
- **Procesare** – Transformă datele. Includ operații aritmetice (adunare, scădere, înmulțire, împărțire), decizii condiționale (ramificație Da/Nu) și un nod de script puternic care vă permite să scrieți propriul cod Python cu ajutoare vizuale.
- **Destinații** – Afișează sau exportă rezultatele. Cel mai comun este vizualizatorul grafic (osciloscop virtual), dar există și un exportator de grafice de calitate profesională.

Fiecare nod are porturi de intrare (stânga/sus) și de ieșire (dreapta/jos). La conectarea unui port de ieșire cu unul de intrare, semnalul curge între ele.

#### Nod de script avansat
Nodul de script merită o mențiune specială. Este conceput pentru utilizatori avansați care doresc să adauge propria procesare fără a părăsi FloWorks. Oferă:

- Un editor cu evidențiere sintaxă, autocompletare și numerotare linii.
- Posibilitatea de a defini parametri editabili din panoul nodului fără a atinge codul (de exemplu, o valoare numerică folosită ulterior în script).
- Porturi de intrare și ieșire dinamice: adăugând comentarii speciale în script, puteți crea noi conectori.
- Memorie persistentă: o variabilă specială (`persist`) care își păstrează valoarea între execuții, utilă pentru acumulatori sau mașini de stare.
- Șabloane de scripturi pregătite și opțiunea de a salva propriile șabloane.
- Un sistem de ajutor integrat și o consolă care afișează erorile de execuție.

### Interfață utilizator
Pe lângă pânză, interfața include:

- O **bară de instrumente** cu toate nodurile organizate pe categorii, meniuri de limbă, temă și dimensiune font, plus acces la vizualizatorul de jurnale.
- Un **tabel de parametri** care afișează informații despre nodurile selectate și evidențiază posibile incompatibilități (cum ar fi încercarea de a opera cu semnale de lungimi diferite).
- **Panouri de analiză** andocabile: statistici (maxim, minim, valoare efectivă), cursoare A/B pentru măsurarea diferențelor și un punct de mira cu marcator de vârf.
- Un **dialog de bun venit** care se adaptează la rezoluția ecranului și vă oferă opțiuni inițiale.

### Conexiune cu instrumente reale
Dacă dispuneți de hardware compatibil (osciloscoape Siglent SDS, multimetre LCR, generatoare SDG), FloWorks poate comunica cu ele prin protocolul standard VISA/SCPI. Configurarea se realizează din panouri specifice în cadrul aplicației. Când capturați un semnal multicanal (de exemplu, modul și faza de la un LCR), nodul sursă împachetează toate canalele și puteți alege care să vizualizați printr-un simplu meniu contextual.

### Export profesional de grafice
Exportatorul de grafice vă permite să generați imagini gata pentru rapoarte sau publicații. La dublu clic pe el, se deschide un dialog cu multiple opțiuni: puteți personaliza culori, tipuri de linie, etichete, scări, alege între formatele PNG, PDF sau SVG și salva preferințele ca profiluri reutilizabile.

### Personalizare vizuală
FloWorks include mai multe teme (întunecat, deschis, contrast ridicat) care schimbă aspectul întregii aplicații instantaneu, fără repornire. De asemenea, puteți ajusta dimensiunea globală a fontului din meniu (Informații → Dimensiune font) și toate elementele se redimensionează în consecință, inclusiv textele din interiorul nodurilor, notele lipicioase și graficele.

### Sistem de limbi
Aplicația detectează automat limba sistemului la prima pornire și salvează preferința. Puteți schimba limba în orice moment din meniu; toate textele, meniurile și ajutoarele se actualizează pe parcurs.

### Proiecte și fișiere `.sflow`
Întreaga dumneavoastră lucrare se salvează într-un singur fișier cu extensia `.sflow`. Acest fișier conține întreaga diagramă: noduri, conexiuni, note, configurații, scripturi și datele numerice generate. Îl puteți partaja cu alți utilizatori; la deschiderea pe alt calculator, notele și nodurile se rescalează automat pentru a se adapta la densitatea de pixeli a acelui ecran.

---

## Fluxuri de lucru tipice

1. **Creați o diagramă simplă**  
   Selectați un nod sursă (de ex., Generator) și un nod Vizualizator din bara de instrumente.  
   Conectați ieșirea generatorului la intrarea vizualizatorului (Ctrl+clic pe portul de ieșire, apoi clic pe cel de intrare).  
   Apăsați F5 pentru a executa. Veți vedea semnalul pe grafic.

2. **Folosiți un script personalizat**  
   Adăugați un nod Script.  
   Scrieți codul Python în editor; puteți defini parametri editabili și porturi suplimentare.  
   Conectați intrările și ieșirile ca la orice alt nod.  
   Executați fluxul; scriptul se procesează cu datele dumneavoastră.

3. **Capturați date de la un osciloscop real**  
   Conectați instrumentul și configurați comunicația din panoul nodului Osciloscop.  
   Nodul achiziționează semnalul și îl livrează prin porturile de ieșire (câte unul pe canal).  
   Conectați acele porturi la alte noduri de procesare sau la vizualizator.

4. **Exportați un grafic pentru un raport**  
   Conectați semnalul dorit la un nod Exportator de grafice.  
   Selectați în nod (clic dreapta) pentru a configura aspectul vizual al graficului.  
   De asemenea, se pot încărca/salva profiluri pentru a grăbi obținerea de grafice gata pentru rapoarte, obținând fișierul imagine în extensia aleasă.

---

## La ce servește toate acestea

Această arhitectură este gândită pentru ca dumneavoastră să vă puteți concentra pe analiza semnalelor fără să vă preocupați de organizarea internă a programului. Fiecare componentă are o funcție clară și lucrează împreună pentru a oferi o experiență fluidă, de la simulare la instrumentație reală, trecând prin personalizarea vizuală și exportul rezultatelor.

Dacă vreodată aveți nevoie să extindeți capacitățile FloWorks (de exemplu, adăugând noi tipuri de noduri sau conectând un instrument diferit), știți că există o structură modulară care permite acest lucru, deși acesta este teritoriul dezvoltatorilor. Ca utilizator final, bucurați-vă de flexibilitatea pe care v-o oferă acest design.
