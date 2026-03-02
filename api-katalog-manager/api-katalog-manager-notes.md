# API-Katalog Manager

## Was die App macht
Excel-basiertes Analyse-Tool für API-Kataloge. Ermöglicht zwei Kernfunktionen:
1. **Suche & Auswahl**: Excel-Katalog hochladen, durchsuchen, APIs per Checkbox auswählen, Auswahl als PowerPoint exportieren
2. **Quartalsvergleich**: Zwei Excel-Kataloge (aktuelles vs. älteres Quartal) vergleichen – zeigt neu hinzugekommene und entfernte APIs mit PowerPoint-Export

## Aktueller Stand
Version 1.0 – vollständig funktionsfähig

**Features:**
- Zwei Modi: Suche & Auswahl / Quartalsvergleich
- Excel-Upload und Parsing via SheetJS (xlsx@0.18.5)
- Volltextsuche über alle Spalten
- Checkbox-Selektion mit Live-Statistiken (Anzahl, Gesamtkosten, Paket-Verteilung)
- Availability-Badges: Standard / Individualisierung / In Entwicklung
- Quartalsvergleich mit Diff (neu / entfernt / unverändert)
- PowerPoint-Export via PptxGenJS (pptxgenjs@3.12.0) – Titelfolie, Übersicht, Tabellen für neue/entfernte APIs
- Kein Backend, kein Login – rein clientseitig

**Tech-Stack:**
- Single-file HTML
- Vanilla JS + Vanilla CSS
- SheetJS (CDN): Excel lesen
- PptxGenJS (CDN): PowerPoint generieren
- Keine API-Anbindung

**Erwartetes Datenformat (Excel-Spalten):**
- `Name` – API-Name (Pflichtfeld, wird als eindeutiger Key genutzt)
- `Paket` – Paket-Kategorie
- `Jährl. Lizenzgebühr` – numerischer Wert in €
- `Verfügbarkeit` – Standard / Individualisierung / In Entwicklung
- `Ab` – Verfügbar ab
- `Lizenziert` – Lizenzstatus

## Geplante Änderungen
- [ ] [Eigene Ergänzungen hier]

## Bekannte Probleme / Einschränkungen
- API-Vergleich basiert ausschließlich auf dem Feld `Name` – doppelte Namen können zu Fehlern führen
- Kein persistentes Speichern der Selektion (Session-basiert)
- PowerPoint-Export enthält keine Lizenzgebühren-Summierung in der Übersicht

## Wichtige Code-Stellen
- `handleMainFile()` / `handleCompareFile()` – Excel-Parsing
- `performComparison()` – Diff-Logik zwischen zwei Katalogen
- `exportToPowerPoint('selection')` / `exportToPowerPoint('comparison')` – PowerPoint-Generierung
- `updateStats()` – Berechnung der Auswahl-Statistiken
- `formatAvailability()` – Badge-Rendering

## Nächste Session – Kontext für Claude
"Ich arbeite am API-Katalog Manager (api-katalog-manager.html).
Das ist ein Single-file HTML Tool ohne Backend – es liest Excel-Dateien clientseitig
und exportiert Auswahlen oder Quartalsvergleiche als PowerPoint.
Heute möchte ich: [HIER EINFÜGEN]"
