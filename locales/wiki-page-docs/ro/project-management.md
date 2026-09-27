---
title: Gestionarea proiectelor
description: Cum să salvați, deschideți, exportați și protejați fluxurile în FloWorks.
---

# 📁 Gestionarea proiectelor

FloWorks salvează fluxurile în fișiere cu extensia **`.sflow`**. Aceste fișiere conțin toate informațiile proiectului: noduri, conexiuni, configurare și note adezive.

---

## Crearea, deschiderea și salvarea

| Acțiune | Meniu | Scurtătură |
|--------|------|-------|
| **Proiect nou** | Fișier → Nou | `Ctrl + N` |
| **Deschide proiect** | Fișier → Deschide | `Ctrl + O` |
| **Salvează** | Fișier → Salvează | `Ctrl + S` |
| **Salvează ca…** | Fișier → Salvează ca… | `Ctrl + Shift + S` |

**Regula de Aur:**
Fluxurile sunt **complet compatibile cu toate versiunile** FloWorks (Core, Lite, Pro). Nu trebuie să convertiți sau modificați nimic: doar deschideți și executați.

---

## Export și import

- Pentru **a partaja un flux**, copiați fișierul `.sflow` pe alt echipament.
- Pentru **a aduce un flux extern**, folosiți **Fișier → Deschide** și selectați fișierul.
- Dacă aveți nevoie să **exportați date numerice** (de exemplu, în CSV), utilizați instrumentul **Foaie de calcul** din panoul lateral și salvați tabelul de acolo.

---

## Recuperare în caz de închideri neașteptate

FloWorks **nu salvează automat**. De aceea este important:

- Să salvați frecvent (`Ctrl + S`), în special înainte de a executa fluxuri cu hardware real.
- Dacă aplicația se închide neașteptat, modificările nesalvate ar putea fi pierdute.
- Pentru a lucra cu deplină liniște, obișnuiți-vă să salvați după fiecare modificare importantă.

---

## Organizare recomandată

- Creați un folder pentru fiecare proiect sau client și salvați acolo toate fișierele `.sflow` corelate.
- Utilizați **note adezive** pe pânză pentru a documenta secțiuni ale fluxului.
- Atribuiți **nume descriptive nodurilor** (dublu clic → nume) pentru a fi mai ușor de găsit și înțeles fluxul săptmâni mai târziu.

---

## Bune practici

- Înainte de a executa un flux cu instrumente reale, salvați fișierul.
- Dacă lucrați în echipă, folosiți un sistem de control al versiunilor (Git, copii manuale) pentru a nu suprascrie fluxuri importante.
- Faceți copii de siguranță ale fluxurilor de calibrare sau de diagnosticare critice.

---

> **Sfat:** Un flux bine organizat și salvat este baza unei lucrări profesionale în FloWorks. Nu subestimați puterea unui nume clar și a unui folder ordonat.
