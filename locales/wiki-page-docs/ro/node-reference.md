---
title: Referință tehnică noduri
description: Catalog actualizat, contracte de extensie și capacități avansate ale sistemului de noduri FloWorks
---

# 🧩 Referință tehnică noduri

FloWorks nu depinde de un catalog static. Utilizează un **sistem de înregistrare dinamic** bazat pe contracte clare. Acest lucru permite extinderea platformei fără a atinge motorul topologic. Mai jos este detaliat catalogul implementat, capacitățile tehnice reale și protocolul pentru extinderea sigură.

---

## 📂 Categorii nucleu

=== "📦 Vizualizare pe straturi"
    <div class="grid cards" markdown>

    - **📥 Surse/Intrare**
      Generează sau capturează semnale inițiale. Suportă simulare integrată, hardware real (VISA/SCPI) și mod multicanal.
    - **⚙️ Procesare**
      Transformă, combină sau analizează date. Păstrează dimensionalitatea și interpolează automat când este necesar.
    - **🔀 Control/Flux**
      Bifurcă, iterează sau condiționează Rulează. Includ suport nativ pentru semnale de activare.
    - **🐍 Scripting/Avansat**
      Execută cod Python dinamic cu porturi parametrice (`# @param`), porturi dinamice (`# @input`/`# @output`) și persistență stare (`persist`).
    - **🔌 Hardware/Instrumentație**
      Interfețe pentru osciloscoape, metru LCR și generatoare.
    - **📤 Ieșire/Export**
      Vizualizează, exportă sau arhivează rezultate. Suportă teme vizuale, profiluri utilizator și format profesional (PNG/PDF/SVG).

    </div>

---

## 📋 Catalog tehnic implementat

| Nod | Tip | Responsabilitate principală | Caracteristici cheie |
|-----|-----|--------------------------|---------------------|
| `SumNode` | Procesare | Operator aritmetic (+, -, *, /) pentru două intrări. | Interpolează automat semnale de rezoluții diferite (FFTs). Păstrează dimensionalitatea. |
| `RhombusNode` | Control | Condițional (bifurcație Da/Nu). | Două porturi de ieșire. Evaluează condiția după prag sau logică booleană. |
| `TriggerNode` | Control | Iterator/accumulator cu activare externă. | Primește `(x, y, "trigger")`. Acumulează până la N iterații și emite rezultat stivuit/mediat. |
| `ScriptNode` | Avansat | Mediu de scripting Python integrat. | QScintilla, autocompletare, `# @param`, porturi dinamice, `persist`, șabloane, consolă erori, interpretor extern cu timeout. |
| `OscilloscopeNode` | Hardware | Captură de la osciloscoape (SDS) sau metru LCR. | Mod simulare, dialog firewall integrat, **suport multicanal** (`out_primary`, `out_secondary`), meniu „Afișează canal". |
| `GeneratorNode` | Sursă | Trimite semnale la generatoare (SDG) sau simulează ieșiri. | Configurare modulație/sweep, dialog simulare integrat. |
| `GraphExporterNode` | Ieșire | Exportator profesional de grafice. | Configurare prin dublu clic, axe personalizate, teme, profiluri salvate etc. |

---

## 🔍 ScriptNode: Capacități esențiale

> **🐍 Mediu de scripting integrat**
>
> - **Editor de cod integrat:** Evidențiere sintaxă de bază, numerotare linii și pliere cod.
> - **Panou parametri dinamici:** Directive `# @param NUME : tip = valoare` injectează controale editabile (spinbox, câmp text etc.) în panoul lateral.
> - **Porturi dinamice:** `# @input nume` și `# @output nume` creează porturi în timp real. Scriptul primește dicționarul `inputs` și returnează `outputs`.
> - **Persistență stare:** Dicționar global `persist` care menține valori între execuții.
> - **Șabloane și Import/Export:** Meniu drop-down cu scripturi de bază. Utilizatorul poate salva scripturi în `nodes/script_node/templates/` sau importa/exporta fișiere `.py` externe.
> - **Consolă erori integrată:** Afișează erori de sintaxă/execuție cu linia exactă indicată în editor.
> - **Ajutor și i18n:** Tooltip-uri contextuale, buton `?` cu ghid rapid, iar toate textele folosesc `tr()` pentru traducere.
> - **Interpretor extern cu timeout:** Cale configurabilă (`# @python_path` sau buton „Răsfoire…"). Execuție izolată cu limită de timp și fallback la interpretorul intern.
> - **Serializare completă:** Salvează script, parametri, porturi dinamice și stare `persist`. La încărcarea unui `.sflow`, reconstruiește automat porturile și parametrii.

---

## 📚 Resurse conexe

- [📖 Harta codului și arhitectura](architecture-ii.md) → Responsabilități pe module și fluxuri de lucru.
- [🌐 Ghid de internaționalizare (i18n)](i18n.md) → Cum se adaugă limbi și se gestionează cheile `tr()`.
- [🛠️ Adaugă un nod nou (tutorial)](adding-a-new-node.md) → Pas cu pas cu exemple practice.
- [📦 Ghid build și distribuție](build.md) → Ambalare PyInstaller, hooks și semnături digitale.

---

💡 **Lipsește un nod din acest catalog?**
FloWorks este proiectat pentru extensibilitate. Dacă ai nevoie de un nod care nu există, creează-l conform contractului `BaseNode` și înregistrează-l. Comunitatea și viitorul marketplace vor extinde continuu ecosistemul fără a sparge compatibilitatea.
