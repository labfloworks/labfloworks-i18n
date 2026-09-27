# Sticky Notes – User Guide

## What are sticky notes?

Sticky notes are small text blocks that you can place freely on the diagram. They are useful for:

- Adding reminders, titles or explanations directly on the canvas.
- Creating step-by-step tutorials that guide anyone who uses your project.
- Documenting parts of the workflow without leaving FloWorks.
- Leaving comments for yourself or other collaborators.

Notes are resizable (by dragging their corners), can be moved anywhere on the diagram and are saved together with the project. When you open a `.sflow` file, all notes appear exactly where you left them.

---

## The new feature: multilingual notes

Sticky notes can automatically display text in the language you choose for the application.
Instead of writing the final message in a single language, you can insert **special markers** that will translate themselves when you change FloWorks' language.

This way, a single note can be read in Spanish, English or any other available language without needing to edit the text each time.

---

## How to write a multilingual note

Inside a note (create one by double-clicking or with the 📝 button on the toolbar), you can use two types of markers:

### 1. Using the word `tr(…)`
Type `tr("key")` and replace `key` with a descriptive name for the phrase.

Example:

tr("tutorial.step1.title")
tr("tutorial.step1.message")

### 2. Using double braces `{{…}}`
Type `{{key}}` in the same way.

Example:

{{tutorial.step1.title}}
{{tutorial.step1.message}}


Both formats work the same; choose whichever is more comfortable (you can even combine them in the same note).

> **Important**: The text you see when editing the note contains the original markers (for example `{{tutorial.step1.title}}`).
> When you finish editing and return to the normal diagram view, the markers are replaced by the phrase translated into the application's current language.

---

## Behavior when changing language

- If you change the language from the FloWorks menu (for example, from Spanish to English), **all sticky notes containing markers update automatically**.
- There is no need to close and reopen the project, or touch each note manually.
- Notes that only contain plain text (without markers) are not affected; they show the same thing in any language.

---

## Advantages of using markers

- **Instant multilingual tutorials** – A single note serves to guide users of different languages.
- **Consistency** – If you modify the translation in a single place (the language file that your development team maintains), all notes that use that key will update.
- **Easy maintenance** – You can write the content once and reuse it in multiple notes.
- **Flexibility** – Combine fixed text with markers. For example:

---

## Practical example: a step-by-step tutorial

Suppose you want to add a note explaining the first step of a tutorial.
In edit mode you write:

🎯 STEP 1
{{tutorial.step1.title}}
{{tutorial.step1.message}}


When you finish editing and are using the application in Spanish, you will see:

🎯 STEP 1
¡Bienvenido a FloWorks!
Arrastre un nodo fuente de señal para comenzar.


If you switch the language to English, the same note will display:

🎯 STEP 1
Welcome to FloWorks!
Drag a signal source node to begin.


And so on for any other language you have configured.

---

## Summary

- Sticky notes enrich your diagrams with textual information.
- They can now be **multilingual** using the markers `tr("key")` or `{{key}}`.
- When editing you will see the keys; when viewing, the translated text.
- Change the application language and all notes will adapt instantly.
- Perfect for creating visual documentation, tutorials or notices that need to work in several languages.

Take advantage of this feature to make your projects more accessible and easier to share!
