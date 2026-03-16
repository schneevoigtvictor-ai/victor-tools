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

**Features (v2.3 – aktuell):**

*Allgemein:*
- Landing-Page mit Token-Eingabe (KIA-Token, lokal gespeichert)
- Vier Modi: IST-Beschreibung + Allgemeine Anforderung + Test-Assistent + Aktionssteuerung (Platzhalter)
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
- **v2.3: Fließtext mit Unterüberschriften** (Prozessauslöser, Bearbeitungsschritte, Output, optional Ablehnungen/Sonderfälle)
- **v2.3: Bearbeitungsschritte im Fließtext nur als Zusammenfassung** – Details stehen in der Prozesstabelle
- Praxisbeispiele: APK (vereinfacht), Hautkrebsvorsorge (ausführlich)
- `istSubMode` steuert Routing (`SYS_IST` vs. `SYS_IST_SIMPLE`)

*Allgemeine Anforderung (v2.0 + v2.3):*
- Iteratives Sparring mit 5-Kriterien-Bewertung (JA/TEILWEISE/NEIN)
- Dynamische Rückfragen mit "Entfällt"-Option
- Automatische Generierung von Akzeptanzkriterien und Testfällen
- Finalisierungsansicht mit differenzierten Copy-Buttons
- **v2.3: IST und SOLL jeweils mit Fließtext + eigener Prozesstabelle**
  - Fließtext-Unterüberschriften IST: Prozessauslöser, Bearbeitungsschritte (Zusammenfassung), Output, optional Ablehnungen/Sonderfälle
  - Fließtext-Unterüberschriften SOLL: Zielbild, Prozessauslöser, Bearbeitungsschritte (Zusammenfassung), Output, optional Ablehnungen/Sonderfälle
  - IST-TABELLE und SOLL-TABELLE als eigene Sektionen im KIA-Output
  - Tab-Ansicht (Fliesstext | Prozesstabelle) im Living Document und in der Finalize-Ansicht
  - Copy-Buttons: "Alles kopieren", "Nur Text", "Nur Tabelle" (analog IST-Modus)
- **v2.3: AK/TF-Generierung robuster**
  - Prompt enthält jetzt IST-TABELLE + SOLL-TABELLE für besseren Kontext
  - Auto-Retry bei leerem Ergebnis (kein Pipe-Tabelle im Return)
  - Warn-Toast wenn nach Retry immer noch leer

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

## Geplante Änderungen (v2.4)
- [ ] **Nachträgliche Eingaben sollen IST-Text/Tabelle/Kennzahlen aktualisieren** – aktuell werden Korrekturen im Chat nicht in die bestehenden Felder zurückgeschrieben (IST + Anforderungs-Modus)
- [ ] Aktionssteuerung-Wizard implementieren (aktuell nur Platzhalter)

## Bekannte Probleme / Einschränkungen
- Läuft nur mit aktivem KIA-Token (SBK-intern)
- Token-Validierung nur clientseitig
- Kein persistentes Speichern der Anforderung (Session-basiert)
- Spracheingabe nur in Browsern mit Web Speech API (Chrome, Edge – kein Firefox)
- `parseBewertung()` ist fragil – nicht ändern ohne guten Grund
- IST/Anforderungs-Modus: Nachträgliche Eingaben aktualisieren Living Doc nicht (nur Test-Assistent tut das korrekt)
- AK/TF-Generierung kann trotz Auto-Retry gelegentlich leer zurückkommen (KIA-Modell-abhängig)

## Wichtige Code-Stellen

*Bestehend (IST + Anforderung):*
- `SYS_GENERAL` – System-Prompt Allgemein-Modus (5 Bewertungskriterien), v2.3: IST/SOLL mit Unterüberschriften + IST-TABELLE/SOLL-TABELLE
- `SYS_IST` – System-Prompt IST-Modus ausführlich (6 Kriterien), v2.3: Fließtext mit Unterüberschriften
- `SYS_IST_SIMPLE` – System-Prompt IST-Modus vereinfacht (4 Kriterien), v2.3: Fließtext mit Unterüberschriften
- `SYS_AKTF` – System-Prompt Akzeptanzkriterien/Testfälle
- `parseKIA()` – Parser Allgemein-Modus (Titel, IST, istTab, SOLL, sollTab, Kennzahlen, Bewertung, Rückfragen). v2.3: erkennt `IST-TABELLE:` und `SOLL-TABELLE:` als eigene Sektionen; Pipe-Zeilen innerhalb IST/SOLL-Text werden automatisch in Tabellen-Felder umgeleitet
- `parseIST()` – Parser IST-Modus (Fließtext + Tabelle)
- `parseBewertung()` – Bewertungs-Zeilen-Extraktion (FRAGIL – nicht ändern!)
- `callKIA()` – API-Aufruf
- `renderIstTextStructured()` / `splitIstIntoBlocks()` – Farbige IST-Text-Blöcke
- `renderIstOutputCard()` – Tab-Ansicht (Fliesstext | Prozesstabelle), wird jetzt auch im Allgemein-Modus genutzt
- `renderDocSection()` – v2.3: Hilfsfunktion für IST/SOLL Living Doc mit Tabs wenn Tabelle vorhanden
- `renderMd()` – v2.3: Verschachtelte Sub-Listen (`.md-sub`) für Ja/Nein-Logik unter nummerierten Schritten; En-Dash (–) als Listen-Marker erkannt
- `mdToClipboardHtml()` – v2.3: Gleiche verschachtelte Sub-Listen-Logik für Confluence-Copy
- `copyRich()` / `pipeTableToClipboardHtml()` – Rich-Copy
- `copySec()` – v2.3: Versteht kombinierte Keys (`ist+istTab`, `soll+sollTab`) für "Alles kopieren"
- `copyAllSections()` – v2.3: Enthält IST-/SOLL-Prozesstabellen in der Kopier-Reihenfolge
- `generateAkTf()` – v2.3: Prompt enthält Prozesstabellen; Auto-Retry bei leerem Ergebnis
- `toggleSpeech()` – Spracheingabe
- `selIstMode()` / `toggleIstExample()` – IST-Untermodus-Auswahl
- `docState` – v2.3: Enthält `istTab` und `sollTab` als eigene Felder

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

## Änderungshistorie v2.3

**Einheitliche Prozessdarstellung – Fließtext + Prozesstabellen**

*System-Prompts (4 Änderungen):*
1. `SYS_IST` – IST-TEXT mit Unterüberschriften: Prozessauslöser, Bearbeitungsschritte (Zusammenfassung), Output, optional Ablehnungen/Sonderfälle
2. `SYS_IST_SIMPLE` – Gleiches Schema ohne Ja/Nein-Logik
3. `SYS_GENERAL` IST-Sektion – Unterüberschriften + IST-TABELLE als eigene Sektion
4. `SYS_GENERAL` SOLL-Sektion – Unterüberschriften (mit Zielbild) + SOLL-TABELLE als eigene Sektion
5. Alle 4 Prompts: Bearbeitungsschritte im Fließtext nur als Zusammenfassung (2-3 Sätze), KEINE Einzelschritte – Details stehen in der Prozesstabelle

*Parser (1 Änderung):*
- `parseKIA()` – Neue Header `H_IST_TAB` und `H_SOLL_TAB` erkennen `IST-TABELLE:` und `SOLL-TABELLE:`; werden VOR `H_IST`/`H_SOLL` geprüft (sonst false match); Pipe-Zeilen innerhalb IST/SOLL-Text werden automatisch in `istTab`/`sollTab` umgeleitet; Return enthält `istTab` und `sollTab`

*Living Document + Rendering (6 Änderungen):*
- `docState` – Neue Felder `istTab` und `sollTab`
- `updateDoc()` – Ruft `renderDocSection()` für IST/SOLL; istTab/sollTab triggern Re-Render des Parent
- `renderDocSection()` – NEU: Rendert IST/SOLL mit `renderIstOutputCard()` (Tab-Ansicht) wenn Tabelle vorhanden
- `callAndRender()` – Übergibt `p.istTab` und `p.sollTab` an `updateDoc`
- `startFinalize()` – IST/SOLL als Tab-Karten mit 3 Copy-Buttons (Alles / Nur Text / Nur Tabelle)
- `copySec()` – Kombinierte Keys `ist+istTab` und `soll+sollTab` für "Alles kopieren"
- `copyAllSections()` – Reihenfolge: titel, ist, istTab, soll, sollTab, kz, ak, tf

*Rendering-Verbesserungen (3 Änderungen):*
- `renderMd()` – Verschachtelte Sub-Listen: Dash/Bullet-Zeilen nach nummeriertem Schritt werden als `<ul class="md-sub">` innerhalb des `<li>` gerendert; En-Dash (–, U+2013) als Listen-Marker erkannt
- `mdToClipboardHtml()` – Gleiche verschachtelte Sub-Listen-Logik für korrekte Confluence-Copy
- CSS `.md-sub` – Kompakte Sub-Liste (kleinere Schrift, Border-Left, kein Card-Look)
- CSS `.ist-section-body .md-ol li` – Nummerierte Schritte innerhalb Sektionsblöcke kompakt ohne Card-Look

*AK/TF-Generierung (1 Änderung):*
- `generateAkTf()` – Prompt enthält IST-TABELLE + SOLL-TABELLE; kein `stripMd()` mehr; Auto-Retry bei leerem Ergebnis; Warn-Toast bei Fehlschlag

## Umsetzungshinweise

**Methode:** Ausschließlich per str_replace. Kein komplettes Neuschreiben.

**Kritisch:** `parseBewertung()` nicht ändern. `parseKIA()` und `parseIST()` nur erweitern, nicht umbauen. `renderMd()` Änderungen wirken global – sorgfältig testen.
