# Lernmodul Bausteine - Template Dokumentation

Diese Templates sind generalisierte, wiederverwendbare Bausteine für Lernmodule.
Alle Platzhalter sind mit `{{PLATZHALTER_NAME}}` gekennzeichnet und müssen durch den tatsächlichen Inhalt ersetzt werden.

---

## 📁 Übersicht der Templates

| Template | Datei | Beschreibung |
|----------|-------|--------------|
| **Wissensabfrage** | `template_wissensabfrage.html` | Quiz mit Multiple-Choice-Szenarien und Feedback |
| **Bild und Info** | `template_bild_info.html` | Text-Bild Layout mit Hinweisen und Formeln |
| **Code Block** | `template_code_block.html` | Syntax-Highlighting + Copy-Button |
| **Flipcard** | `template_flipcard.html` | 3D-Flip zwischen Foto ↔ Schaltplan |

---

## 🧠 Wissensabfrage (`template_wissensabfrage.html`)

### Platzhalter

| Platzhalter | Beschreibung | Beispiel |
|-------------|--------------|----------|
| `{{SECTION_ID}}` | Eindeutige Section-ID | `quiz-ldr` |
| `{{SECTION_EMOJI}}` | Emoji der Sektion | `🧠` |
| `{{QUIZ_TITLE}}` | Titel des Quiz | `Teste dein Wissen` |
| `{{QUIZ_INTRO}}` | Einleitungstext | `Beantworte folgende Fragen...` |
| `{{SCENARIO_ID}}` | ID des Szenarios | `1`, `2` |
| `{{SCENARIO_ICON}}` | FontAwesome Icon | `fa-solid fa-moon` |
| `{{SCENARIO_ICON_COLOR}}` | Icon-Farbe | `#6c757d` |
| `{{SCENARIO_TITLE}}` | Szenario-Titel | `Szenario A: Dunkle Umgebung` |
| `{{SCENARIO_DESC}}` | Beschreibung | `Sie messen in einer dunklen Umgebung...` |
| `{{SCENARIO_QUESTION}}` | Die Frage | `Welchen Widerstand wählen Sie?` |
| `{{OPTION_VALUE}}` | Wert der Option | `1000`, `10000` |
| `{{OPTION_TEXT}}` | Anzeigetext | `1 kΩ (Klein)` |
| `{{CORRECT_VALUE}}` | Richtige Antwort | `100000` |
| `{{CORRECT_MESSAGE}}` | Feedback (richtig) | `<strong>Richtig!</strong> ...` |
| `{{WRONG_MESSAGE}}` | Feedback (falsch) | `<strong>Nicht optimal.</strong> ...` |

---

## 🖼️ Bild und Info (`template_bild_info.html`)

### Platzhalter

| Platzhalter | Beschreibung | Beispiel |
|-------------|--------------|----------|
| `{{SECTION_ID}}` | Eindeutige Section-ID | `grundlagen` |
| `{{SECTION_EMOJI}}` | Emoji der Sektion | `💡` |
| `{{SECTION_TITLE}}` | Haupttitel | `Technische Grundlagen` |
| `{{SUBTITLE}}` | Untertitel (h3) | `Funktionsprinzip LDR` |
| `{{INFO_TEXT_1}}` | Erster Textabsatz | `Ein LDR ist ein...` |
| `{{INFO_TEXT_2}}` | Zweiter Textabsatz | `Je mehr Licht...` |
| `{{IMAGE_SRC}}` | Bildpfad | `Media/Images/LDR.jpg` |
| `{{IMAGE_ALT}}` | Alt-Text | `Foto eines LDR` |
| `{{IMAGE_WIDTH}}` | Bildbreite | `300px` |
| `{{HINT_ICON}}` | Hinweis-Icon | `fa-regular fa-lightbulb` |
| `{{HINT_TEXT}}` | Hinweistext | `Der Zusammenhang ist invers!` |
| `{{FORMULA_TITLE}}` | Formeltitel | `Die Formel` |
| `{{FORMULA_LATEX}}` | LaTeX-Formel | `V_{out} = V_{in} \cdot ...` |
| `{{IMAGE_CAPTION}}` | Bildunterschrift | `Abbildung: LDR-Verhalten` |

---

## 💻 Code Block (`template_code_block.html`)

### Platzhalter

| Platzhalter | Beschreibung | Beispiel |
|-------------|--------------|----------|
| `{{CODE_TITLE}}` | Titel über Code | `Arduino Code` |
| `{{CODE_INTRO}}` | Einleitungstext | `Dieser Code schaltet die LED ein...` |
| `{{CODE_LANGUAGE}}` | Programmiersprache | `cpp`, `python`, `arduino` |
| `{{CODE_CONTENT}}` | Der Code selbst | `void setup() { ... }` |
| `{{CODE_INSTRUCTIONS}}` | Anleitung | `Laden Sie den Code auf den Arduino...` |

---

## 🔄 Flipcard (`template_flipcard.html`)

### Platzhalter

| Platzhalter | Beschreibung | Beispiel |
|-------------|--------------|----------|
| `{{FLIPCARD_ID}}` | Eindeutige ID | `flip-spannungsteiler` |
| `{{FLIPCARD_WIDTH}}` | Kartenbreite | `650px` |
| `{{FLIPCARD_HEIGHT}}` | Kartenhöhe | `450px` |
| `{{FRONT_IMAGE_SRC}}` | Vorderseite Bild | `Media/Images/Foto.jpg` |
| `{{FRONT_IMAGE_ALT}}` | Vorderseite Alt | `Foto der Schaltung` |
| `{{FRONT_FALLBACK}}` | Vorderseite Fallback | `https://placehold.co/350x450` |
| `{{BACK_IMAGE_SRC}}` | Rückseite Bild | `Media/Images/Schaltplan.png` |
| `{{BACK_IMAGE_ALT}}` | Rückseite Alt | `Schaltplan` |
| `{{BACK_FALLBACK}}` | Rückseite Fallback | `https://placehold.co/350x450` |
| `{{FLIP_HINT}}` | Hinweistext | `Klicken: Foto ↔ Schaltplan` |
| `{{CAPTION_TEXT}}` | Bildunterschrift | `Spannungsteiler-Schaltung` |

---

## 📝 Verwendung

1. Template-Datei kopieren
2. Alle `{{PLATZHALTER}}` durch tatsächliche Inhalte ersetzen
3. Bei Bedarf Styles in das Haupt-CSS verschieben (um Duplikate zu vermeiden)
4. JavaScript-Funktionen prüfen (bei mehrfacher Verwendung nur einmal einbinden)

---

## ⚠️ Hinweise

- **CSS-Variablen**: Die Templates nutzen `--kit-primary-color`, `--correct-color`, `--incorrect-color`
- **Dependencies**: FontAwesome, highlight.js, MathJax (für Formeln)
- **Mehrfachnutzung**: Bei mehreren Flipcards/Quizzes pro Seite eindeutige IDs vergeben!
