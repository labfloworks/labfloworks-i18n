---
title: Ghid pentru Adăugarea unui Nod Nou în FloWorks
description: Tutorial pas cu pas pentru crearea, înregistrarea și integrarea nodurilor personalizate în motorul de flux al FloWorks.
---

# 📘 Ghid pentru Dezvoltatori: Cum se Adaugă un Nod Nou în FloWorks

Acest ghid descrie procesul complet pentru crearea unui nou tip de nod în FloWorks, asigurând integrarea corectă cu motorul de flux, interfața utilizator, temele vizuale și sistemul de internaționalizare.

---

## 📋 Cuprins
- [📘 Ghid pentru Dezvoltatori: Cum se Adaugă un Nod Nou în FloWorks](#-ghid-pentru-dezvoltatori-cum-se-adaugă-un-nod-nou-în-floworks)
  - [📋 Cuprins](#-cuprins)
  - [1. Introducere în Arhitectură](#1-introducere-în-arhitectură)
  - [2. Utilizarea Șablonului `template_node.py`](#2-utilizarea-șablonului-template_nodepy)
  - [3. Pas cu Pas: Crearea unui Nod Personalizat](#3-pas-cu-pas-crearea-unui-nod-personalizat)
    - [3.1. Copierea și Redenumirea Șablonului](#31-copierea-și-redenumirea-șablonului)
    - [3.2. Definirea Porturilor și Etichetelor](#32-definirea-porturilor-și-etichetelor)
    - [3.3. Implementarea Logicii de Procesare](#33-implementarea-logicii-de-procesare)
    - [3.4. Personalizarea Aspectului (Opțional)](#34-personalizarea-aspectului-opțional)
    - [3.5. Adăugarea Parametrilor Configurabili (Opțional)](#35-adăugarea-parametrilor-configurabili-opțional)
    - [3.6. Serializarea Nodului (salvare / încărcare configurații)](#36-serializarea-nodului-salvare--încărcare-configurații)
  - [4. Integrarea în Sistem](#4-integrarea-în-sistem)
  - [5. Internaționalizarea (i18n)](#5-internaționalizarea-i18n)
  - [6. Teme Vizuale](#6-teme-vizuale)
  - [7. Lista de Verificare și Rezolvarea Problemelor](#7-lista-de-verificare-și-rezolvarea-problemelor)
    - [✅ Lista de Verificare](#-lista-de-verificare)
    - [🐛 Probleme Comune](#-probleme-comune)
  - [8. Concluzie](#8-concluzie)

---

## 1. Introducere în Arhitectură

FloWorks este construit pe PySide6 și utilizează un model de noduri conectabile care reprezintă un flux de procesare a semnalelor.

---

## 2. Utilizarea Șablonului `template_node.py`

Pentru a facilita crearea de noduri noi, se pune la dispoziție fișierul `nodes/template_node.py`. Acest șablon include:
- Suport complet pentru internaționalizare (conectare la `languageChanged`, metoda `update_language`).
- Suport complet pentru teme (metoda `update_theme`).
- Ajutor integrat cu format HTML pe trei secțiuni.
- Gestionarea mai multor porturi de intrare/ieșire configurabile.
- Ieșiri multiple cu `get_output_for_port`.
- Vizualizare în plot prin `get_display_signal`.
- Meniu contextual traductibil.

Se recomandă să se plece întotdeauna de la acest șablon la dezvoltarea unui nod nou.

---

## 3. Pas cu Pas: Crearea unui Nod Personalizat

### 3.1. Copierea și Redenumirea Șablonului
1. Copiați `nodes/template_node.py` cu numele noului nod, de exemplu `nodes/mi_nodo.py`.
2. Redenumiți clasa din `TemplateNode` în ceva descriptiv, ex. `MiNodoNode`.
3. Ajustați importurile dacă este necesar.

### 3.2. Definirea Porturilor și Etichetelor
!!! warning "Important: Coincidența Numelor"
    Numele porturilor din `PORTS`, `PORT_LABELS` și cheile dicționarului returnat de `execute_program` trebuie să fie **exact aceleași** (inclusiv majuscule/minuscule). Șablonul include acum un mapare de aliasuri (`'data_in'` → primul port stâng) pentru o mai mare robustețe.

Editați dicționarul `PORTS` din partea de sus a fișierului. Fiecare intrare are formatul:
```python
"nume_port": ("latură", fracțiune)
```
- **Laturi posibile:** `"left"`, `"right"`, `"top"`, `"bottom"`.
- **Fracțiune:** valoare între `0.0` și `1.0` care indică poziția de-a lungul laturii.

**Exemplu pentru un nod cu o intrare și două ieșiri:**
```python
PORTS = {
    "input":     ("left",  0.5),
    "magnitude": ("right", 0.35),
    "phase":     ("right", 0.65),
}
```
Dicționarul `PORT_LABELS` conține textul care va apărea lângă fiecare port. Se recomandă utilizarea cheilor de traducere în loc de text fix (vezi secțiunea Internaționalizare).

### 3.3. Implementarea Logicii de Procesare
Metoda cheie este `execute_program(self, input_data)`. Această metodă este invocată de motorul de flux când nodul primește date.

**`input_data` poate fi:**
- `None` dacă nu există intrare.
- O tuplă `(x, y)` pentru semnale temporale.
- Un array 1D.
- Un dicționar `{nume_port: date}` la noduri cu intrări multiple.

**Valoare returnată:**
- Pentru noduri cu o singură ieșire, returnați direct datele (ex. tuplă `(x, y)`).
- Pentru noduri cu ieșiri multiple, returnați un dicționar unde cheile coincid cu numele porturilor de ieșire definite în `PORTS`.

```python
def execute_program(self, input_data):
    # Procesați input_data și generați rezultatele
    rezultat_magnitudine = (freq, mag)
    rezultat_fază = (freq, phase)
    return {
        "magnitude": rezultat_magnitudine,
        "phase": rezultat_fază
    }
```

!!! tip "Notă despre numele generice de port"
    Motorul de flux poate transmite ocazional un dicționar cu chei precum `'data_in'` în locul numelui real al portului (în special dacă utilizatorul nu a făcut clic exact pe cerc). Șablonul include deja cod pentru gestionarea acestui caz:
    ```python
    if isinstance(input_data, dict):
        if 'data_in' in input_data:
            input_data = input_data['data_in']
    ```
    Acest lucru evită ca nodul să eșueze din cauza unei conexiuni imprecise.

Șablonul include deja un exemplu comentat. În plus, implementează `get_output_for_port(self, port_name)` pentru ca motorul să poată direcționa fiecare ieșire:
```python
def get_output_for_port(self, port_name):
    return self.output_data.get(port_name)
```

### 3.4. Personalizarea Aspectului (Opțional)
Metoda `paint()` desenează fundalul, titlul, starea și orice text suplimentar. Puteți modifica:
- Culorile (se actualizează automat cu `update_theme`).
- Textul stării (folosind atributul `self._status`).
- Informații de rezumat (ex. vârf de magnitudine).

Șablonul afișează un exemplu de bază.

### 3.5. Adăugarea Parametrilor Configurabili (Opțional)
Dacă nodul dumneavoastră necesită parametri ajustabili de utilizator (ex. dimensiune fereastră, frecvență de tăiere), puteți:
1. Adăuga atribute în `__init__` (ex. `self.window_size = 512`).
2. Crea un dialog de configurare (moștenește `QDialog`).
3. Conecta dialogul în `open_config_dialog()` (metodă deja prezentă în șablon).
4. Actualiza parametrii din dialog și apelați `self.update()`.

### 3.6. Serializarea Nodului (salvare / încărcare configurații)
Pentru ca nodul să poată salva și recupera parametrii săi la copiere/lipire, anulare/refacere, sau la utilizarea comenzilor Salvare/Deschidere din meniul Fișier, acesta trebuie să moștenească din mixin-ul de serializare și să-și declare atributele.

1. Importați mixin-ul în fișierul dumneavoastră:
    ```python
    from nodes.serializable import SerializableMixin
    ```
2. Schimbați moștenirea clasei pentru a-l include pe mixin înainte de `QGraphicsObject`:
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
    ```
3. Definiți lista `SERIALISABLE` la nivel de clasă, cu numele atributelor pe care doriți să le persistați. Acceptă doar tipuri simple (`int`, `float`, `str`, `bool`), liste, dicționare, sau array-uri NumPy (acestea din urmă se stochează automat ca fișiere `.npy` în interiorul `.sflow`).
    ```python
    class MiNodoNode(SerializableMixin, QGraphicsObject):
        SERIALISABLE = ['frecuencia', 'amplitud', 'configuracion']
    ```
4. Asigurați-vă că aceste atribute sunt inițializate în `__init__`:
    ```python
    self.frecuencia = 1000.0
    self.amplitud = 1.0
    self.configuracion = {'tipo': 'seno', 'fase': 0}
    ```

Cu aceasta, nu trebuie să scrieți metode `serialize`/`deserialize`; mixin-ul se ocupă automat de salvarea și recuperarea valorilor.

Dacă nodul dumneavoastră necesită logică suplimentară la încărcare (de exemplu, reconectarea unui instrument hardware), puteți suprascrie `deserialize` apelând mai întâi metoda părinte:
```python
def deserialize(self, data):
    super().deserialize(data)   # restaurează atributele din SERIALISABLE
    self._iniciar_dispositivo()
```

---

## 4. Integrarea în Sistem

Odată creat fișierul nodului, trebuie doar să-l lipiți în folderul `nodes` pentru a apărea în interfață și a funcționa cu restul sistemului.

---

## 5. Internaționalizarea (i18n)

Toate textele vizibile trebuie să fie traductibile prin `tr("cheie", default="...")`. Șablonul implementează deja acest lucru. Trebuie să adăugați cheile corespunzătoare în fișierele JSON din interiorul `locales/`.

**Structură recomandată:**
```json
{
   "nodes": {
     "mi_nodo": {
       "title": "Mi Nodo",
       "tooltip": "Descriere emergentă",
       "ports": {
         "input": "Intrare",
         "output1": "Ieșire 1",
         "output2": "Ieșire 2"
      },
       "status": {
         "no_data": "Fără date",
         "ready": "Pregătit"
      },
       "menu": {
         "show_output": "Afișează ieșirea",
         "configure": "Configurează..."
      },
       "help_title": "Ajutor - Mi Nodo",
       "help_html": "<h3>🎛️ Filter Node</h3>
<p>Applies a <b>digital filter</b>...</p>"
    }
  },
   "toolbar": {
     "add_mi_nodo": "Mi Nodo"
  }
}
```

Ajutorul HTML urmează formatul pe trei secțiuni comun tuturor nodurilor (descriere specifică + "Cum să gândești despre sistem" + "Scurtături și trucuri"). Șablonul include deja structura în `get_help_text()`.

---

## 6. Teme Vizuale

Metoda `update_theme(self, theme)` primește un dicționar cu culorile definite de tema curentă. Șablonul actualizează automat:
- Fundalul nodului (`node_normal_bg`)
- Bordura (`node_selected_border`)
- Culoarea titlului și a textului (`node_normal_text`)
- Culorile porturilor (`port_circle`, `port_outline`, `port_inline`, `port_text`)

Asigurați-vă că în `MainWindow` (sau `ThemeUpdater`) se apelează `node.update_theme()` pentru fiecare nod când se schimbă tema.

---

## 7. Lista de Verificare și Rezolvarea Problemelor

### ✅ Lista de Verificare
- [ ] Nodul se creează corect din bara de instrumente.
- [ ] Porturile se afișează în pozițiile așteptate și sunt detectabile pentru conexiuni (`Ctrl+clic`).
- [ ] La primirea datelor de intrare, `execute_program` este apelat și semnalul este procesat.
- [ ] Ieșirile se propagă corect către nodurile conectate.
- [ ] Meniul contextual permite schimbarea canalului de vizualizare (dacă există ieșiri multiple).
- [ ] La clic pe nod, se grafichează semnalul selectat în widget-ul de plot.
- [ ] Dublu clic deschide ajutorul cu formatul adecvat.
- [ ] Limba se schimbă corect (texte de titlu, porturi, meniuri).
- [ ] Tema se schimbă corect (culori ale nodului și porturilor).
- [ ] Copiere/lipire funcționează fără erori.

!!! tip "Conectare precis a porturilor"
    La conectarea nodurilor, asigurați-vă că faceți clic exact pe cercul portului destinație. Dacă faceți clic pe corpul nodului, sistemul va folosi un nume generic (`'data_in'`). Șablonul tolerează acum aceste nume, dar este o bună practică să vă conectați direct la cerc pentru a garanta direcționarea corectă a ieșirilor multiple.

### 🐛 Probleme Comune

| Simptom | Cauză Posibilă | Soluție |
|---------|---------------|----------|
| Săgeata de conexiune nu se ancorează la port. | Cercul portului nu are `setData(0, port_name)` sau `get_port_scene_pos` nu este implementat. | Verificați că în `_create_ports` se face `circle.setData(0, port_name)` și că `get_port_scene_pos` folosește acel nume. |
| Ieșirile nu ajung la nodurile conectate. | `execute_program` nu returnează un dicționar (pentru ieșiri multiple) sau `get_output_for_port` nu este implementat. | Asigurați-vă că `execute_program` returnează `{nume_port: date}` și că `get_output_for_port` returnează valoarea corespunzătoare. |
| La clic pe nod nu se grafichează nimic. | `get_display_signal` nu returnează o tuplă `(x, y)` validă sau `display_channel` nu coincide cu o ieșire existentă. | Verificați că `get_display_signal` folosește canalul selectat și că datele sunt array-uri NumPy. |
| Textele nu se actualizează la schimbarea limbii. | Nu s-a conectat semnalul `languageChanged` sau `update_language` nu actualizează elementele. | Verificați conectarea în `__init__`: `language_manager.languageChanged.connect(self.update_language)`. |
| Tema nu se aplică. | Nu se apelează `update_theme` la crearea nodului sau la schimbarea temei. | În `MainWindow`, după crearea nodului, invocați `node.update_theme(self.theme_manager.current_theme())`. |
| Săgeata indică spre centrul nodului. | S-a făcut clic pe corp în loc de cerc, sau numele nu coincide cu `PORTS`. | Faceți clic direct pe cerc. Verificați că `get_port_scene_pos` are maparea de aliasuri. |
| `NameError: name 'self' is not defined` la importare. | Atributele de instanță au fost declarate în afara `__init__`. | Toate atributele precum `self.mi_parametro` trebuie definite în interiorul `__init__`. |
| Parametrii se pierd la copiere/deschidere `.sflow`. | Nodul nu moștenește din `SerializableMixin` sau nu a definit `SERIALISABLE`. | Implementați pasul 3.6 al acestui ghid. |

---

## 8. Concluzie

Urmând acest ghid și utilizând șablonul `template_node.py`, veți putea adăuga noduri noi în FloWorks în mod eficient și coerent cu restul sistemului. Rețineți întotdeauna să mențineți compatibilitatea cu i18n și temele pentru o experiență profesională a utilizatorului.

Încurajați-vă să contribuiți cu propriile noduri!
