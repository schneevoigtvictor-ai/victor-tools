# Anforderungs-Assistent

## Was die App macht
KI-gestützter Sparringspartner für Business Analysten der SBK. 
Hilft dabei, unstrukturierte Anforderungen iterativ zu schärfen – von der Rohbeschreibung bis zur JIRA-fähigen Anforderung mit IST/SOLL, Akzeptanzkriterien und Testfällen.
Neu: Test-Assistent für Testfallerstellung, -verfeinerung und Fehlerdokumentation.

## Aktueller Stand
Version 2.0 – IST-Modus und Rich-Copy
Version 2.1 – Vereinfachter IST-Modus (simple/detailed) + IST-Untermodus-Auswahl
Version 2.2 – **Test-Assistent** (3 Module: Erstellen, Verfeinern, Fehler dokumentieren)
Version 2.3 – **Einheitliche Prozessdarstellung** (Fließtext mit Unterüberschriften + Prozesstabellen in allen Modi)
Version 2.4 – **UX-Verbesserungen + Bugfixes** (ANFO-Ticket, Parser-Robustheit, Copy-Überarbeitung)

**Features (v2.4 – aktuell):**

*Allgemein:*
- Landing-Page mit Token-Eingabe (KIA-Token, lokal gespeichert)
- Vier Modi: IST-Beschreibung + **ANFO-Ticket** (ehem. Allgemeine Anforderung) + Test-Assistent + Aktionssteuerung (Platzhalter)
- Split-View: Live-Dokument links, Chat rechts
- Spracheingabe per Web Speech API (Chrome/Edge)
- Rich-Copy für Confluence: HTML + Plain-Text (Tabellen, Fettdruck, Listen, Zeilenumbrüche)
- Onboarding-Modal mit Beschreibung aller drei Modi
- Mobile-responsive mit Tab-Navigation
- Phase-Indikator in der Topbar
- Dynamischer Button-Text: "Neue Anforderung" vs. "Neuer Test"

*IST-Modus (v2.0 + v2.1 + v2.3):*
- Unterwahl: Vereinfacht (4 Kriterien) vs. Ausführlich (6 Kriterien)
- Eigener Sparring-Flow mit Fließtext + Prozesstabelle
- Strukturierter IST-Text-Renderer mit farbigen Sektionsblöcken
- Fließtext mit Unterüberschriften (Prozessauslöser, Bearbeitungsschritte, Output, optional Ablehnungen/Sonderfälle)
- Bearbeitungsschritte im Fließtext nur als Zusammenfassung – Details stehen in der Prozesstabelle
- Praxisbeispiele: APK (vereinfacht), Hautkrebsvorsorge (ausführlich)
- `istSubMode` steuert Routing (`SYS_IST` vs. `SYS_IST_SIMPLE`)

*ANFO-Ticket (ehem. Allgemeine Anforderung, v2.0 + v2.3 + v2.4):*
- **v2.4: Umbenannt** von "Allgemeine Anforderung" zu "ANFO-Ticket" (Landing-Page + Onboarding-Modal)
- **v2.4: "Art der Anforderung" entfernt** – `reqType` wird automatisch auf `'fachlich'` gesetzt (in `selectMode()` und `confirmNewReq()`)
- **v2.4: Praxisbeispiel** – Aufklappbarer `<details>`-Block im Intake-Bereich (Hautkrebsvorsorge: IST, SOLL, Kennzahlen, AK-Auszug, TF-Auszug)
- Iteratives Sparring mit 5-Kriterien-Bewertung (JA/TEILWEISE/NEIN)
- Dynamische Rückfragen mit "Entfällt"-Option
- Automatische Generierung von Akzeptanzkriterien und Testfällen
- Finalisierungsansicht mit differenzierten Copy-Buttons
- IST und SOLL jeweils mit Fließtext + eigener Prozesstabelle
  - Fließtext-Unterüberschriften IST: Prozessauslöser, Bearbeitungsschritte (Zusammenfassung), Output, optional Ablehnungen/Sonderfälle
  - Fließtext-Unterüberschriften SOLL: Zielbild, Prozessauslöser, Bearbeitungsschritte (Zusammenfassung), Output, optional Ablehnungen/Sonderfälle
  - IST-TABELLE und SOLL-TABELLE als eigene Sektionen im KIA-Output
  - Tab-Ansicht (Fliesstext | Prozesstabelle) im Living Document und in der Finalize-Ansicht
- **v2.4: Copy-Hierarchie überarbeitet**
  - Top-Button "Alles kopieren" = wirklich alles (Titel + IST + IST-Tab + SOLL + SOLL-Tab + KZ + AK + TF)
  - Per-Section-Buttons: "Text + Tabelle" (statt "Alles kopieren"), "Nur Text", "Nur Tabelle"
  - AK-Karte: Kopier-Button oben rechts im Header (statt unten an der Tabelle)
  - Top-Button dynamisch verdrahtet via `g('btnCopyAll').onclick` – im IST-Modus `copyIstAll()`, im Anforderungs-Modus `copyAllSections()`
- **v2.4: AK/TF-Generierung robuster**
  - Auto-Retry bei AK **oder** TF leer (statt nur wenn beide leer – AND→OR)
  - Ergebnisse aus beiden Versuchen werden zusammengeführt (Gutes aus 1. Versuch bleibt erhalten)
  - Filter-Chips-Erkennung lockerer: auch Gedankenstrich (—), Pipe, Punkt als Trennzeichen; `|positiv` mitten in Pipe-Zeile als Fallback

*Test-Assistent (v2.2):*
- Drei Module: Testfälle erstellen, Testfälle verfeinern, Testergebnis dokumentieren
- **Modul 1 – Testfälle erstellen:**
  - Eigene System-Prompts (`SYS_TEST_CREATE`) mit 4-Kategorie-Prefix-Pflicht
  - 4 Testfall-Kategorien: Positiv, Grenzwert, Negativ, Eingangsprüfung
  - Farbige Filter-Chips: Positiv (grün) + Grenzwert (orange) + Negativ (rot) = default AN, Eingangsprüfung (grau) = default AUS
  - Dynamische Tabellen-Header aus KIA-Output (nicht hardcoded)
  - Nummerierte Listen in Zellen werden als Zeilenumbrüche gerendert
  - `colgroup` für feste Spaltenbreiten bei 5-Spalten-Layout
  - Kopier-Button direkt an der Tabelle – kopiert nur sichtbare (gefilterte) Zeilen
  - Confluence-Copy: `innerHTML` mit `<br>`-Erhalt statt `textContent`
  - Prompt-Regeln: Keine Fake-Daten, keine Login-Testfälle, keine erfundenen Fehlermeldungen
- **Modul 2 – Testfälle verfeinern:**
  - 2-Phasen-Ablauf: Phase 1 = Analyse + Rückfragen (KEINE Tabelle), Phase 2 = Tabelle wenn alles klar
  - Nachher-Tabelle mit Filter-Chips + Copy-Button
  - Keine Annahmen: Platzhalter statt erfundener Systemverhalten
  - VORHER-Block wird nicht im Output angezeigt (nur Analyse + Begründung + Tabelle)
- **Modul 3 – Testergebnis dokumentieren:**
  - Nur Fehlerdokumentation (kein Bestanden/Teilweise)
  - Intake: Testfall-Name + Erwartetes Ergebnis + Fehlerbeschreibung (3 Felder)
  - Output: 4-Felder-Text (Ist-Ergebnis, Soll-Ergebnis, Abweichung, Empfohlene Maßnahme)
  - Keine Tabelle, nur strukturierter Text mit Rich-Copy
  - Zusammenfassung + Bewertung + Rückfragen im Result-Modus komplett unterdrückt
  - Screenshot-Tipp als Hinweis-Box
  - `cleanVal()` bereinigt stray `**`-Sternchen aus KIA-Output
  - `ergClean` schneidet RUECKFRAGEN-Anhang ab vor Feld-Extraktion
- **Übergreifend:**
  - `renderTestMsg()` mit eigenem Sektions-Parsing (unabhängig von `parseKIA`)
  - Sektions-Regex: `[\s:*]*\n` am Ende für flexible Header-Erkennung (`**HEADER:**`, `HEADER:` etc.)
  - `stripPipes()` entfernt Pipe-Zeilen aus Text-Sektionen (verhindert doppelte Darstellung)
  - `tfRawStore` (Map) speichert Rohdaten per ID für Filter/Copy
  - Fallback-Pipe-Extraktion aus rawText wenn Sektions-Parsing fehlschlägt
  - Praxisbeispiele zeigen finales Output-Format (Tabelle mit Filter-Chips, Text mit 4 Feldern)
  - Reset in `confirmNewReq()` für alle Test-State-Variablen

**Tech-Stack:**
- Single-file HTML (kein Framework)
- Vanilla CSS mit CSS-Variablen
- Vanilla JS
- API: Internes KIA-System der SBK (`kia.apps.test.sbk.de`)
- Modell: `openai/gpt-oss-120b`
- Fonts: DM Sans + Fraunces (Google Fonts)

## Geplante Änderungen (v2.5)
- [ ] **Nachträgliche Eingaben sollen IST-Text/Tabelle/Kennzahlen aktualisieren** – aktuell werden Korrekturen im Chat nicht in die bestehenden Felder zurückgeschrieben (IST + Anforderungs-Modus)
- [ ] Aktionssteuerung-Wizard implementieren (aktuell nur Platzhalter)
- [ ] Toter Code aufräumen: `selType()`-Funktion und `.type-chip`-CSS (nach Entfernung der Art-der-Anforderung-Auswahl)

## Bekannte Probleme / Einschränkungen
- Läuft nur mit aktivem KIA-Token (SBK-intern)
- Token-Validierung nur clientseitig
- Kein persistentes Speichern der Anforderung (Session-basiert)
- Spracheingabe nur in Browsern mit Web Speech API (Chrome, Edge – kein Firefox)
- `parseBewertung()` ist fragil – nicht ändern ohne guten Grund
- IST/Anforderungs-Modus: Nachträgliche Eingaben aktualisieren Living Doc nicht (nur Test-Assistent tut das korrekt)
- AK/TF-Generierung kann trotz Auto-Retry gelegentlich leer zurückkommen (KIA-Modell-abhängig)
- KIA-Antworten ohne erkannte Header (~20% der Fälle) fallen in den Fallback-Renderer – v2.4 verbessert, aber modellabhängig nicht komplett eliminierbar

## Wichtige Code-Stellen

*Bestehend (IST + ANFO-Ticket):*
- `SYS_GENERAL` – System-Prompt ANFO-Ticket-Modus (5 Bewertungskriterien), v2.3: IST/SOLL mit Unterüberschriften + IST-TABELLE/SOLL-TABELLE
- `SYS_IST` – System-Prompt IST-Modus ausführlich (6 Kriterien), v2.3: Fließtext mit Unterüberschriften
- `SYS_IST_SIMPLE` – System-Prompt IST-Modus vereinfacht (4 Kriterien), v2.3: Fließtext mit Unterüberschriften
- `SYS_AKTF` – System-Prompt Akzeptanzkriterien/Testfälle
- `parseKIA()` – Parser ANFO-Ticket-Modus. v2.4: Header-Regexes gelockert (Doppelpunkt optional bei Zeilenende, `Zusammenfassung:` als Alias für `Analyse:`, `Offene Fragen:` für Rückfragen, `Aktueller Zustand/Prozess` für IST, `Zielzustand/Zielbild` für SOLL)
- `parseIST()` – Parser IST-Modus (Fließtext + Tabelle)
- `parseBewertung()` – Bewertungs-Zeilen-Extraktion (FRAGIL – nicht ändern!)
- `callKIA()` – API-Aufruf
- `renderIstTextStructured()` / `splitIstIntoBlocks()` – Farbige IST-Text-Blöcke
- `renderIstOutputCard(textContent, tableContent, tableId, tabPrefix)` – Tab-Ansicht (Fliesstext | Prozesstabelle). v2.4: 4. Parameter `tabPrefix` für stabile Tab-IDs (verhindert Date.now()-Kollision bei IST/SOLL)
- `renderDocSection()` – Hilfsfunktion für IST/SOLL Living Doc mit Tabs. v2.4: übergibt `'doc-'+field` als `tabPrefix`
- `renderMd()` – Verschachtelte Sub-Listen (`.md-sub`) für Ja/Nein-Logik unter nummerierten Schritten; En-Dash (–) als Listen-Marker erkannt
- `mdToClipboardHtml()` – Gleiche verschachtelte Sub-Listen-Logik für Confluence-Copy
- `pipeTableToClipboardHtml()` – v2.4: Neue `cellHtml()`-Hilfsfunktion wandelt `**Fettdruck**` → `<strong>`, nummerierte Listen → `<br>`-Umbrüche, statt reinem `escH()`
- `copyRich()` – Rich-Copy
- `copySec()` – Versteht kombinierte Keys (`ist+istTab`, `soll+sollTab`) für "Text + Tabelle"
- `copyAllSections()` – v2.4: AK wird jetzt als Pipe-Tabelle kopiert (wie TF), nicht mehr als Markdown-Text; Reihenfolge: titel, ist, istTab, soll, sollTab, kz, ak, tf
- `copyIstAll()` – IST-Modus "Alles kopieren" (Titel + Text + Tabelle + KZ)
- `generateAkTf()` – v2.4: Auto-Retry bei AK **oder** TF leer (OR statt AND); Ergebnisse zusammengeführt
- `renderTfWithToolbar()` – v2.4: Filter-Chips-Erkennung lockerer (Gedankenstrich, Pipe, Punkt als Trennzeichen)
- `renderAkAsTable(text, externalId)` – v2.4: Optionale `externalId` für stabile DOM-IDs; Bottom-Button entfernt (Button ist jetzt im Card-Header)
- `renderKIAMsg()` – v2.4: Verbesserter Fallback-Renderer (Absatz-Trennung, Markdown-Header als `<h4>`, statt rohem renderMd-Dump)
- `toggleSpeech()` – Spracheingabe
- `selIstMode()` / `toggleIstExample()` – IST-Untermodus-Auswahl
- `docState` – Enthält `istTab` und `sollTab` als eigene Felder
- `reqType` – v2.4: Wird automatisch auf `'fachlich'` gesetzt, `selType()` ist toter Code

*Test-Assistent (v2.2):*
- `SYS_TEST_CREATE` – Prompt Testfälle erstellen (4 Kategorien, keine Fake-Daten, 2-Phasen)
- `SYS_TEST_REFINE` – Prompt Testfälle verfeinern (2-Phasen: Fragen → Tabelle)
- `SYS_TEST_RESULT` – Prompt Testergebnis dokumentieren (4-Felder-Text, nur Fehler)
- `renderTestMsg(p, rawText)` – Eigenes Sektions-Parsing + Rendering für alle 3 Module
- `renderTfTable(raw)` – Testfall-Tabelle mit dynamischen Headern, colgroup, nummerierte Listen
- `filterTfCat(tfId, btn)` – 4-Kategorie-Filter (Positiv/Grenzwert/Negativ/Eingangsprüfung)
- `copyTfFromChat(tfId, btn)` – Kopiert sichtbare Zeilen aus DOM (innerHTML für `<br>`-Erhalt)
- `copyErgText(elId, btn)` – Kopiert Testergebnis-Text als Rich-HTML für Confluence
- `selTestModule(mod)` – Modul-Auswahl (create/refine/result)
- `startTestSparring()` – Sparring starten mit modulspezifischem Prompt
- `callAndRenderTest()` – KIA-Aufruf + renderTestMsg
- `updateTestDoc(field, content, status)` – Living-Doc-Update
- `finalizeTest()` – Finalisierungsansicht
- `tfRawStore` (Map) – Rohdaten-Speicher für Filter/Copy
- `getTestExampleCreate/Refine/Result()` – Praxisbeispiele im finalen Output-Format

## Änderungshistorie v2.4

**UX-Verbesserungen + Bugfixes**

*Umbenennung + UI-Bereinigung (3 Änderungen):*
1. "Allgemeine Anforderung" → "ANFO-Ticket" (Landing-Page Karte + Onboarding-Modal)
2. "Art der Anforderung" (Chips: Fachlich/Change/Technisch) komplett entfernt – `reqType` automatisch `'fachlich'` in `selectMode('general')` und `confirmNewReq()`
3. Praxisbeispiel im ANFO-Intake: Aufklappbarer `<details>`-Block mit kompaktem Hautkrebsvorsorge-Beispiel (IST, SOLL, KZ, AK-Auszug 3 Zeilen, TF-Auszug 3 Zeilen mit farbigen Kategorie-Dots)

*Bugfix: Tab-Kollision IST/SOLL (1 Änderung):*
- `renderIstOutputCard()` hat neuen 4. Parameter `tabPrefix` – erzeugt stabile IDs statt `Date.now()`. `renderDocSection()` übergibt `'doc-ist'` / `'doc-soll'` → keine doppelten DOM-IDs mehr, SOLL-Tab schaltet jetzt korrekt um

*Bugfix: Parser-Robustheit (2 Änderungen):*
- `parseKIA()` Header-Regexes gelockert: Doppelpunkt optional bei eindeutigen langen Formen + Zeilenende (`$`), neue Aliase (`Zusammenfassung` → Analyse, `Offene Fragen` → Rückfragen, `Aktueller Zustand/Prozess` → IST, `Zielzustand/Zielbild` → SOLL, `IST-Prozesstabelle` → IST-Tab). Kurze Formen (`IST:`, `SOLL:`) behalten Doppelpunkt-Pflicht (Sicherheit gegen false matches)
- `renderKIAMsg()` Fallback-Renderer verbessert: Text wird an Doppel-Newlines aufgesplittet, Markdown-Header als `<h4>` gerendert, Absätze einzeln formatiert statt roher `renderMd()`-Dump

*Bugfix: AK/TF-Generierung (2 Änderungen):*
- `generateAkTf()` Retry-Bedingung: `&&` → `||` (Retry wenn AK **oder** TF leer). Ergebnisse aus beiden Versuchen zusammengeführt – Gutes aus 1. Versuch bleibt erhalten
- `renderTfWithToolbar()` Filter-Chips-Erkennung lockerer: Regex erkennt auch Gedankenstrich (—), Pipe (`|`), Punkt als Trennzeichen nach Kategorie-Präfix; `|positiv` mitten in Pipe-Zeile als Fallback

*Copy-Überarbeitung (4 Änderungen):*
- Top-Button "Alles kopieren" (`#btnCopyAll`) dynamisch verdrahtet: `finalizeIst()` → `copyIstAll()`, `startFinalize()` → `copyAllSections()`
- Per-Section-Buttons umbenannt: "Alles kopieren" → "Text + Tabelle" (IST-Finalize, Anforderung-IST, Anforderung-SOLL)
- `copyAllSections()`: AK wird jetzt als Pipe-Tabelle kopiert (`pipeTableToClipboardHtml`) statt Markdown (`mdToClipboardHtml`)
- `renderAkAsTable(text, externalId)`: Bottom-Button entfernt; AK-Karte in `startFinalize()` hat Kopier-Button im Header (konsistent mit allen anderen Sektionen); Chat behält eigenen "Tabelle kopieren"-Button

*Confluence-Formatierung (1 Änderung):*
- `pipeTableToClipboardHtml()`: Neue `cellHtml()`-Hilfsfunktion ersetzt `escH()` für Zellinhalte. Wandelt `**text**` → `<strong>text</strong>`, nummerierte Listen (`1. xxx 2. xxx`) → `<br>`-Umbrüche. Wirkt auf alle Pipe-Tabellen-Kopien (AK, TF, IST-Tab, SOLL-Tab)

## Umsetzungshinweise

**Methode:** Ausschließlich per str_replace. Kein komplettes Neuschreiben.

**Kritisch:** `parseBewertung()` nicht ändern. `parseKIA()` und `parseIST()` nur erweitern, nicht umbauen. `renderMd()` Änderungen wirken global – sorgfältig testen. `pipeTableToClipboardHtml()` wirkt auf alle Tabellen-Kopien – Änderungen in `cellHtml()` betreffen AK, TF, IST-Tab und SOLL-Tab gleichzeitig.
