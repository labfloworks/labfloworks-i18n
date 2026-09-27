# Filozofie

## Vedere generală

FloWorks este o aplicație desktop care vă permite să creați lanțuri de procesare a semnalelor prin diagrame de flux vizuale.
Trageți, conectați și configurați noduri; rezultatul este calculat și afișat în timp real.
Lucrați cu semnale simulate sau conectați instrumente reale (osciloscoape, generatoare, multimetre LCR) fără a scrie cod, deși dispuneți de un mediu de scripting puternic dacă doriți să extindeți funcționalitatea.

---

## Caracteristici principale

- **Diagrame interactive** – Construiți-vi fluxul de lucru unind noduri cu linii care reprezintă fluxul de date.
- **Procesare în timp real** – Fiecare modificare se reflectă imediat în grafice și vizualizări.
- **Simulare și hardware real** – Generați semnale de test sau capturați date direct din instrumente de laborator.
- **Nod de script avansat** – Încorporați propriul cod Python cu ajutor de autocompletare, parametri dinamici editabili și memorie persistentă între execuții.
- **Vizualizare profesională** – Semnale, spectre, spectrograme și grafice de înaltă calitate gata de export.
- **Multilingv** – Interfața detectează limba sistemului și permite comutarea între spaniolă, engleză și alte limbi în orice moment.
- **Teme vizuale** – Mod întunecat, luminos și contrast ridicat pentru a se adapta preferințelor sau nevoilor de accesibilitate.
- **Gestionare completă a proiectelor** – Salvați-vă lucrul în fișiere `.sflow` și recuperați-l exact așa cum l-ați lăsat, cu anulare și refacere nelimitate.

---

## Cum să lucrați cu FloWorks

### Noduri
Un nod este o piesă a procesării. Sunt organizate în trei categorii:

- **Surse** – Introduc semnale la începutul fluxului. De exemplu, un osciloscop (real sau simulat), un generator de funcții sau o operație matematic.
- **Procesare** – Transformă datele. Sume, diferențe, condiționale, filtre… inclusiv un nod special pentru a scrie propriile scripturi în Python.
- **Colectoare** – Afișează sau exportă rezultatele. Vizualizatorul grafic și exportatorul profesional de grafice sunt cele mai utilizate.

### Conexiuni
Uniunile dintre noduri sunt desenate ca curbe netede sau linii ortogonale. O animație de flux vă indică în orice moment direcția datelor. Sistemul organizează automat cablurile astfel încât să nu se suprapună.

### Vizualizare
De fiecare dată când un nod produce un semnal, acesta poate fi văzut în panoul de grafice integrat. Puteți explora diferite reprezentări (formă de undă, spectru, spectrogramă) și ajusta scala cu mouse-ul.

---

## Noduri evidențiate
Acestea sunt nodurile minime indispensabile, necesare pentru ca filozofia programului să aibă sens.

### Nod generator de semnale
Sursă de semnale care poate genera simulări personalizate de forme de undă la dorința utilizatorului. Permite dintr-un meniu contextual să selectați sau să introduceți forma de undă dorită.

### Nod de script
Un mediu de programare complet în interiorul diagramei:

- **Editor cu evidențiere sintaxă**, autocompletare și consolă de erori.
- **Parametri dinamici** – Definiți variabile editabile din panoul nodului fără a modifica codul.
- **Porturi configurabile** – Adăugați intrări și ieșiri suplimentare direct din editor.
- **Stare persistentă** – Salvați valori între execuții; totul este stocat împreună cu proiectul.

### Exportator de grafice
Nod colector care generează imagini de înaltă calitate pentru rapoarte sau publicații. Permite configurarea dimensiunii, rezoluției, formatului, printre altele.

---

## Personalizare

- **Limbă** – Aplicația detectează automat limba sistemului și salvează preferința. O puteți schimba din meniu fără a reporni.
- **Aspect** – Alegeți între temă întunecată, luminosă sau contrast ridicat în funcție de lumina ambientală sau nevoile vizuale.

---

## Proiecte și fișiere

Salvați diagrama completă într-un fișier `.sflow`.
La deschidere veți recupera toate nodurile, conexiunile, scripturile, parametrii și configurațiile de vizualizare.
Acțiunile de anulare și refacere vă permit să experimentați fără teama de a pierde lucrul anterior.

---

FloWorks este conceput pentru ca dvs. să vă concentrați pe analiza semnalelor și nu pe detaliile tehnice ale implementării. Trageți, conectați și descoperiți.
