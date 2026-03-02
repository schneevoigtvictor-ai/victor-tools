# Anforderungs-Assistent

## Was die App macht
KI-gestützter Sparringspartner für Business Analysten der SBK. 
Hilft dabei, unstrukturierte Anforderungen iterativ zu schärfen – von der Rohbeschreibung bis zur JIRA-fähigen Anforderung mit IST/SOLL, Akzeptanzkriterien und Testfällen.

## Aktueller Stand
Version 1.0 – vollständig funktionsfähig

**Features:**
- Landing-Page mit Token-Eingabe (KIA-Token, lokal gespeichert)
- Zwei Modi: Allgemeine Anforderung (aktiv) + Aktionssteuerung (Platzhalter)
- Split-View: Live-Dokument links, Chat rechts
- Iteratives Sparring mit strukturiertem Parsing (TITEL, IST, SOLL, Kennzahlen, Rückfragen)
- Interaktive Rückfragen-Blöcke mit "Entfällt"-Option
- Automatische Generierung von Akzeptanzkriterien und Testfällen (Tabelle)
- Finalisierungsansicht mit Copy-Funktionen
- Mobile-responsive mit Tab-Navigation
- Phase-Indikator in der Topbar

**Tech-Stack:**
- Single-file HTML (kein Framework)
- Vanilla CSS mit CSS-Variablen
- Vanilla JS
- API: Internes KIA-System der SBK (`kia.apps.test.sbk.de`)
- Modell: `openai/gpt-oss-120b`
- Fonts: DM Sans + Fraunces (Google Fonts)

## Geplante Änderungen
- [ ] Aktionssteuerung-Wizard implementieren (aktuell nur Platzhalter)
- [ ] [Eigene Ergänzungen hier]

## Bekannte Probleme / Einschränkungen
- Läuft nur mit aktivem KIA-Token (SBK-intern)
- Token-Validierung nur clientseitig
- Kein persistentes Speichern der Anforderung (Session-basiert)

## Wichtige Code-Stellen
- `SYS_GENERAL` – Haupt-System-Prompt für das Sparring
- `SYS_AKTF` – System-Prompt für Akzeptanzkriterien/Testfälle
- `parseKIA()` – Parst die strukturierte KIA-Antwort in Felder
- `callKIA()` – API-Aufruf gegen das interne KIA-System

## Nächste Session – Kontext für Claude
"Ich arbeite am Anforderungs-Assistent (anforderungs-assistent.html). 
Das ist ein Single-file HTML Tool für die SBK, das Analysten beim iterativen 
Erarbeiten von JIRA-Anforderungen unterstützt. Es nutzt die interne KIA-API.
Heute möchte ich: [HIER EINFÜGEN]"
