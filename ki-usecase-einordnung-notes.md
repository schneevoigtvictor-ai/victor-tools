# KI-Usecase-Einordnung

## Was die App macht
Interaktives Lern- und Quiz-Tool für Business Analyst:innen im Krankenversicherungs-Kontext.
Nutzer ordnen reale KI-Use-Cases einer von 6 Kategorien zu und erhalten sofort Feedback, Erklärungen und konkrete nächste Schritte. Ziel: Gespür entwickeln, wann Automatisierung, KI oder gar nichts davon passt.

## Aktueller Stand
Version 1.0 – vollständig funktionsfähig

**Features:**
- 20 Use Cases aus dem Krankenkassen-/Versicherungsumfeld
- 6 Kategorien zur Einordnung (mit Info-Modal pro Kategorie)
- Quiz-Flow: Übersicht → Spielen → Ergebnis mit Erklärung
- Fortschritts-Tracking: Richtig / Falsch / Offen mit Filter-Tabs
- Falsch beantwortete Fälle werden gesperrt (bis zum Reset)
- Tastatursteuerung: Tasten 1–6 zum Auswählen, ESC zum Zurückgehen
- Confetti-Animation bei 100% Abschluss
- Fortschritt wird in localStorage gespeichert (bleibt nach Reload erhalten)
- Dark-Mode Design mit Dot-Grid-Hintergrund

**Die 6 Kategorien:**
- 🟢 Automatisierung – regelbasierte Prozessautomatisierung ohne KI
- 🔵 KI – Textverständnis (GPT / LLM) – Sprachverständnis, Zusammenfassung, Klassifikation
- 🟣 KI – Vorhersage (Machine Learning) – Prognosen auf Basis historischer Daten
- 🟡 KI – Auffälligkeiten erkennen – Anomalie-Detektion, Betrugserkennung
- 🩵 KI im Arbeitsprozess – KI als Assistenz im Tagesgeschäft
- ⚪ Keine KI notwendig – Standard-Software/Reporting reicht aus

**Tech-Stack:**
- Single-file HTML
- Vanilla JS + Vanilla CSS (Dark Theme mit CSS-Variablen)
- Fonts: DM Sans + DM Mono (Google Fonts)
- localStorage für Fortschritts-Persistenz
- Kein Backend, keine externe API

## Datenstruktur (Use Cases im JS-Array)
Jeder Use Case hat folgende Felder:
```javascript
{
  title: "Kurzer Titel",
  description: "Detaillierte Szenario-Beschreibung...",
  solution: "Kategorie-Label (exakt)",  // z.B. "Automatisierung"
  reason: "Warum diese Kategorie?",
  nextSteps: ["Schritt 1", "Schritt 2", "Schritt 3"]
}
```

## Geplante Änderungen
- [ ] Weitere Use Cases hinzufügen (aktuell 20)
- [ ] Schwierigkeitsgrade einführen
- [ ] [Eigene Ergänzungen hier]

## Bekannte Probleme / Einschränkungen
- Fortschritt nur im eigenen Browser gespeichert (localStorage) – kein Sharing
- Gesperrte (falsch beantwortete) Fälle nur durch vollständigen Reset freischaltbar
- Kein Export der Ergebnisse

## Wichtige Code-Stellen
- `useCases[]` – Array mit allen 20 Use Cases (Daten + Lösungen)
- `categories[]` – Array mit den 6 Kategorien (Label, Icon, Tastenkürzel)
- `categoryInfo{}` – Erklärungstexte für die Info-Modals
- `saveState()` / `loadState()` – localStorage-Persistenz
- `doAnswer()` – Auswertungslogik (richtig/falsch)
- `renderSelect()` – Übersichts-Grid mit Filter-Logik

## Nächste Session – Kontext für Claude
"Ich arbeite am KI-Usecase-Einordnung Tool (ki-usecase-einordnung.html).
Das ist ein Single-file HTML Quiz-Tool für Business Analyst:innen der SBK.
Nutzer ordnen KI-Use-Cases in 6 Kategorien ein und lernen dabei den Unterschied
zwischen Automatisierung, verschiedenen KI-Typen und 'Keine KI'.
Fortschritt wird in localStorage gespeichert.
Heute möchte ich: [HIER EINFÜGEN]"
