---
title: Technische Knotenreferenz
description: Aktualisierter Katalog, Erweiterungsverträge und erweiterte Funktionen des FloWorks-Knotensystems
---

# 🧩 Technische Knotenreferenz

FloWorks ist nicht von einem statischen Katalog abhängig. Es verwendet ein **dynamisches Registrierungssystem** basierend auf klaren Verträgen. Dies ermöglicht es, die Plattform zu erweitern, ohne die Topologie-Engine anzufassen. Nachfolgend wird der implementierte Katalog, die tatsächlichen technischen Fähigkeiten und das Protokoll zur sicheren Erweiterung detailliert beschrieben.

---

## 📂 Kernkategorien

=== "📦 Schichtenansicht"
    <div class="grid cards" markdown>

    - **📥 Quellen/Eingabe**
      Generieren oder erfassen anfängliche Signale. Unterstützen integrierte Simulation, echte Hardware (VISA/SCPI) und Mehrkanalmodus.
    - **⚙️ Verarbeitung**
      Transformieren, kombinieren oder analysieren Daten. Bewahren Dimensionalität und interpolieren bei Bedarf automatisch.
    - **🔀 Steuerung/Fluss**
      Verzweigen, iterieren oder konditionieren die Ausführung. Enthalten native Unterstützung für Aktivierungssignale.
    - **🐍 Scripting/Erweitert**
      Führen dynamischen Python-Code mit parametrischen Ports (`# @param`), dynamischen Ports (`# @input`/`# @output`) und Zustandspersistenz (`persist`) aus.
    - **🔌 Hardware/Instrumentierung**
      Schnittstellen für Oszilloskope, LCR-Messgeräte und Generatoren.
    - **📤 Ausgabe/Export**
      Visualisieren, exportieren oder archivieren Ergebnisse. Unterstützen visuelle Designs, Benutzerprofile und professionelles Format (PNG/PDF/SVG).

    </div>

---

## 📋 Implementierter technischer Katalog

| Knoten | Typ | Hauptverantwortung | Schlüsseleigenschaften |
|------|------|---------------------------|------------------------|
| `SumNode` | Verarbeitung | Arithmetischer Operator (+, -, *, /) für zwei Eingänge. | Interpoliert Signale unterschiedlicher Auflösung (FFTs) automatisch. Bewahrt Dimensionalität. |
| `RhombusNode` | Steuerung | Bedingt (Ja/Nein-Verzweigung). | Zwei Ausgangsports. Bewertet Bedingung nach Schwellenwert oder boolescher Logik. |
| `TriggerNode` | Steuerung | Iterator/Akkumulator mit externer Aktivierung. | Empfängt `(x, y, "trigger")`. Akkumuliert bis zu N Iterationen und gibt gestapeltes/gemitteltes Ergebnis aus. |
| `ScriptNode` | Erweitert | Integrierte Python-Scripting-Umgebung. | QScintilla, Autovervollständigung, `# @param`, dynamische Ports, `persist`, Vorlagen, Fehlerkonsole, externer Interpreter mit Timeout. |
| `OscilloscopeNode` | Hardware | Erfasst von Oszilloskopen (SDS) oder LCR-Messgeräten. | Simulationsmodus, integrierter Firewall-Dialog, **Mehrkanalunterstützung** (`out_primary`, `out_secondary`), Menü „Kanal anzeigen". |
| `GeneratorNode` | Quelle | Sendet Signale an Generatoren (SDG) oder simuliert Ausgänge. | Modulations-/Sweep-Konfiguration, integrierter Simulationsdialog. |
| `GraphExporterNode` | Ausgabe | Professioneller Diagrammexporteur. | Konfiguration per Doppelklick, benutzerdefinierte Achsen, Designs, gespeicherte Profile usw. |

---

## 🔍 ScriptNode: Wesentliche Fähigkeiten

> **🐍 Integrierte Scripting-Umgebung**
>
> - **Integrierter Code-Editor:** Grundlegende Syntaxhervorhebung, Zeilennummerierung und Code-Faltung.
> - **Dynamisches Parameterpanel:** Direktiven `# @param NAME : Typ = Wert` injizieren bearbeitbare Steuerelemente (Spinbox, Textfeld usw.) in das Seitenpanel.
> - **Dynamische Ports:** `# @input name` und `# @output name` erstellen Ports in Echtzeit. Das Skript empfängt ein `inputs`-Wörterbuch und gibt `outputs` zurück.
> - **Zustandspersistenz:** Globales `persist`-Wörterbuch, das Werte zwischen Ausführungen beibehält.
> - **Vorlagen und Import/Export:** Dropdown-Menü mit Basisskripten. Der Benutzer kann seine Skripte in `nodes/script_node/templates/` speichern oder externe `.py`-Dateien importieren/exportieren.
> - **Integrierte Fehlerkonsole:** Zeigt Syntax-/Ausführungsfehler mit der genauen Zeile im Editor an.
> - **Hilfe und i18n:** Kontextbezogene Tooltips, `?`-Schaltfläche mit Kurzanleitung, alle Texte verwenden `tr()` für Übersetzung.
> - **Externer Interpreter mit Timeout:** Konfigurierbarer Pfad (`# @python_path` oder Schaltfläche „Durchsuchen…"). Isolierte Ausführung mit Zeitlimit und Fallback auf den internen Interpreter.
> - **Vollständige Serialisierung:** Speichert Skript, Parameter, dynamische Ports und `persist`-Zustand. Beim Laden eines `.sflow` werden Ports und Parameter automatisch wiederhergestellt.

---

## 📚 Verwandte Ressourcen

- [📖 Code-Karte und Architektur](architecture-ii.md) → Verantwortlichkeiten pro Modul und Arbeitsabläufe.
- [🌐 Internationalisierungsleitfaden (i18n)](i18n.md) → Wie man Sprachen hinzufügt und `tr()`-Schlüssel verwaltet.
- [🛠️ Neuen Knoten hinzufügen (Tutorial)](adding-a-new-node.md) → Schritt für Schritt mit praktischen Beispielen.
- [📦 Build- und Vertriebsleitfaden](build.md) → PyInstaller-Packaging, Hooks und digitale Signaturen.

---

💡 **Fehlt ein Knoten in diesem Katalog?**  
FloWorks ist für Erweiterbarkeit konzipiert. Wenn Sie einen Knoten benötigen, der nicht existiert, erstellen Sie ihn gemäß dem `BaseNode`-Vertrag und registrieren Sie ihn. Die Community und der zukünftige Marktplatz werden das Ökosystem kontinuierlich erweitern, ohne die Kompatibilität zu brechen.
