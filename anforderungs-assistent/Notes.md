# Anforderungs-Assistent

## Was die App macht
KI-gestützter Sparringspartner für Business Analysten der SBK. 
Hilft dabei, unstrukturierte Anforderungen iterativ zu schärfen – von der Rohbeschreibung bis zur JIRA-fähigen Anforderung mit IST/SOLL, Akzeptanzkriterien und Testfällen.

## Aktueller Stand
Version 2.0 – vollständig funktionsfähig, IST-Modus und Rich-Copy neu
Version 2.1 – in Arbeit: Vereinfachter IST-Modus (Konzept steht, Umsetzung per str_replace ausstehend)

**Features (v2.0 – produktiv):**
- Landing-Page mit Token-Eingabe (KIA-Token, lokal gespeichert)
- Drei Modi: Allgemeine Anforderung (aktiv) + IST-Beschreibung (aktiv, neu) + Aktionssteuerung (Platzhalter)
- Split-View: Live-Dokument links, Chat rechts
- Iteratives Sparring mit strukturiertem Parsing (TITEL, IST, SOLL, Kennzahlen, Bewertung, Rückfragen)
- Dynamische Rückfragen-Anzahl basierend auf 5-Kriterien-Bewertung (JA/TEILWEISE/NEIN)
- Interaktive Rückfragen-Blöcke mit "Entfällt"-Option
- Automatische Generierung von Akzeptanzkriterien und Testfällen (Tabelle)
- IST-Modus: Eigener Sparring-Flow mit 6-Kriterien-Bewertung, Fließtext + Prozesstabelle, Best-Practice-Beispiele
- Strukturierter IST-Text-Renderer mit farbigen Sektionsblöcken (Prozessauslöser, Prüfschritte, Spezialfälle, Output)
- Spracheingabe per Web Speech API (Mikrofon-Button an allen Textfeldern)
- Rich-Copy für Confluence: HTML + Plain-Text in Zwischenablage (Tabellen, Fettdruck, Listen bleiben erhalten)
- Finalisierungsansicht mit differenzierten Copy-Buttons (Alles, Nur Text, Nur Tabelle)
- Onboarding-Modal ("Wie funktioniert der Assistent?")
- Mobile-responsive mit Tab-Navigation
- Phase-Indikator in der Topbar

**Geplant (v2.1 – vereinfachter IST-Modus):**
- IST-Modus mit Unterwahl: Vereinfacht vs. Ausführlich (Chip-Selection nach Klick auf IST-Karte)
- Vereinfacht: 4 Kriterien (ohne Prüfschritte Ja/Nein-Logik, ohne Sonderfälle), eigener Prompt `SYS_IST_SIMPLE`
- Ausführlich: Bestehendes Verhalten mit 6 Kriterien (unverändert, `SYS_IST`)
- Zwei Auswahlkarten mit Praxisbeispiel-Buttons und aufklappbaren Beispielen
- Praxisbeispiel vereinfacht: Manuelle Dokumentenklassifizierung (APK)
- Praxisbeispiel ausführlich: Hautkrebsvorsorge-Erstattung (bestehendes Beispiel 1:1)
- Nach Auswahl: Gewähltes Beispiel als aufklappbares `<details>`-Element über dem Textarea
- Landing Page: IST-Karte zuerst (ohne Badge), Allgemein-Karte danach (mit "Empfohlen"-Badge)
- State-Variable `istSubMode` steuert Routing (`SYS_IST` vs. `SYS_IST_SIMPLE`)

**Tech-Stack:**
- Single-file HTML (kein Framework)
- Vanilla CSS mit CSS-Variablen
- Vanilla JS
- API: Internes KIA-System der SBK (`kia.apps.test.sbk.de`)
- Modell: `openai/gpt-oss-120b`
- Fonts: DM Sans + Fraunces (Google Fonts)

## Geplante Änderungen
- [ ] **Vereinfachter IST-Modus umsetzen** (Konzept + Referenzdatei fertig, Umsetzung per str_replace auf funktionierender Basis)
- [ ] Aktionssteuerung-Wizard implementieren (aktuell nur Platzhalter)
- [ ] [Eigene Ergänzungen hier]

## Bekannte Probleme / Einschränkungen
- Läuft nur mit aktivem KIA-Token (SBK-intern)
- Token-Validierung nur clientseitig
- Kein persistentes Speichern der Anforderung (Session-basiert)
- Spracheingabe nur in Browsern mit Web Speech API (Chrome, Edge – kein Firefox)

## Wichtige Code-Stellen
- `SYS_GENERAL` – Haupt-System-Prompt für das Sparring (Allgemein-Modus, 5 Bewertungskriterien)
- `SYS_IST` – System-Prompt für den IST-Modus (6 Bewertungskriterien, Fließtext + Tabelle)
- `SYS_IST_SIMPLE` – **NEU (v2.1):** System-Prompt für vereinfachten IST-Modus (4 Kriterien, keine Entscheidungslogik)
- `SYS_AKTF` – System-Prompt für Akzeptanzkriterien/Testfälle
- `parseKIA()` – Parst die strukturierte KIA-Antwort (Allgemein-Modus) in Felder
- `parseIST()` – Parst die strukturierte KIA-Antwort (IST-Modus) in Felder inkl. Tabellenerkennung
- `parseBewertung()` – Extrahiert Bewertungs-Zeilen (JA/TEILWEISE/NEIN) aus KIA-Antwort
- `callKIA()` – API-Aufruf gegen das interne KIA-System
- `renderIstTextStructured()` / `splitIstIntoBlocks()` – Strukturierter IST-Text-Renderer mit farbigen Blöcken
- `copyRich()` / `mdToClipboardHtml()` / `pipeTableToClipboardHtml()` – Rich-Copy-Logik (HTML + Plain-Text)
- `toggleSpeech()` – Spracheingabe-Steuerung (Web Speech API)
- `selIstMode()` / `toggleIstExample()` – **NEU (v2.1):** IST-Untermodus-Auswahl und Praxisbeispiel-Toggle
- `tfRawData` – Separate Variable für Testfall-Rohdaten (verhindert Überschreiben durch `updateDoc()`)

## Umsetzungshinweise für v2.1

**Methode:** Ausschließlich per str_replace auf der funktionierenden v2.0-Basis. Kein komplettes Neuschreiben.

**Referenzdatei:** `ist-vereinfacht-referenz.md` enthält alle Code-Bausteine (CSS, HTML, JS, Prompts) fertig vorbereitet.

**Vier Änderungsblöcke:**
1. **Block A (CSS):** ~20 Zeilen neue Klassen für `.ist-mode-cards`, `.ist-mode-card`, `.ist-example-btn` etc. Einfügen vor dem `@media`-Block.
2. **Block B (Landing Page):** IST-Karte nach oben (ohne Badge), Allgemein-Karte darunter (mit "Empfohlen"). str_replace auf die `mode-cards`.
3. **Block C (IST-Intake HTML):** Auswahlkarten + Praxisbeispiele einfügen. Bestehender Textarea-Block wird in `istInputArea`-Wrapper mit `display:none` gepackt.
4. **Block D (JavaScript):** `istSubMode`-Variable, `SYS_IST_SIMPLE`-Prompt, `selIstMode()`/`toggleIstExample()`, Routing in `startIstSparring()`/`callAndRenderIst()`, Reset-Erweiterung in `confirmNewReq()`.

**Kritisch:** `parseBewertung()` ist bekannt fragil – bei keiner Änderung verlieren. Bestehende System-Prompts (`SYS_GENERAL`, `SYS_IST`, `SYS_AKTF`) bleiben 1:1 erhalten.

## Nächste Session – Kontext für Claude
"Ich arbeite am Anforderungs-Assistent (index.html im Upload).
Single-file HTML Tool für die SBK, nutzt KIA-API.
Heute möchte ich: Den vereinfachten IST-Modus einbauen.
Die hochgeladene Datei ist die funktionierende Basis – bitte NUR per str_replace ändern, nichts neu schreiben.
Änderungen in vier Blöcken: (A) CSS, (B) Landing Page Reihenfolge, (C) IST-Intake Auswahlkarten mit Praxisbeispielen, (D) JS-Routing mit SYS_IST_SIMPLE.
Praxisbeispiel vereinfacht: Manuelle Dokumentenklassifizierung (APK).
Praxisbeispiel ausführlich: Das bestehende Hautkrebsvorsorge-Beispiel 1:1 übernehmen.
istSubMode steuert ob SYS_IST oder SYS_IST_SIMPLE verwendet wird.
Referenzdatei ist-vereinfacht-referenz.md enthält alle fertigen Code-Bausteine."
