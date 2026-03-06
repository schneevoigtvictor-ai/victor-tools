# Workflow – Bestehende App weiterentwickeln

## Übersicht

```
Phase 1: KONZEPT (eigener Chat)
  notes.md hochladen → Feature besprechen → Konzept abstimmen
      ↓
Phase 2: REFERENZDATEI (gleicher oder kurzer Folge-Chat)
  Alle Code-Bausteine als Markdown generieren lassen
      ↓
Phase 3: UMSETZUNG (neuer Chat)
  index.html + Referenzdatei + notes.md hochladen
  → Block für Block per str_replace umsetzen → testen
      ↓
Phase 4: ABSCHLUSS
  GitHub committen → notes.md aktualisieren → Project ersetzen
```

---

## Phase 1: Konzept erarbeiten (eigener Chat)

Hier wird nur gedacht, nicht gebaut. Im Claude Project → "New Chat":

```
Ich arbeite am [App-Name].
Hier die aktuelle notes.md für Kontext.
[notes.md als Upload oder Inhalt reinkopieren]

Ich möchte folgendes neues Feature:
[Beschreibung in 3-5 Sätzen]

Bitte noch NICHT umsetzen – erst gemeinsam durchdenken.
```

**Was in diesem Chat passiert:**
- UX-Varianten besprechen (z.B. Karten vs. Chips, welche Reihenfolge)
- Klären welche bestehenden Code-Stellen betroffen sind
- Neue System-Prompts, State-Variablen, Parser-Änderungen planen
- Praxisbeispiele und Texte abstimmen

**Ergebnis:** Ein abgestimmtes Konzept – was genau gebaut wird, wo es im Code eingreift, welche Entscheidungen getroffen wurden.

---

## Phase 2: Referenzdatei erstellen (gleicher oder Folge-Chat)

Sobald das Konzept steht:

```
Bitte erstelle eine Referenzdatei mit allen Code-Bausteinen für die Umsetzung.
Getrennt nach Änderungsblöcken (CSS, HTML, JS, Prompts).
Für jeden Block angeben WO GENAU im bestehenden Code er eingefügt/geändert wird.
```

**Die Referenzdatei enthält:**
- Fertige Code-Fragmente (kein Pseudocode)
- Klare Angabe der Einfügestelle (z.B. "vor dem @media-Block", "nach function selType()")
- Hinweise was NICHT geändert werden darf
- Reihenfolge der Blöcke (A, B, C, D ...)

**Ergebnis:** Eine `.md`-Datei zum Download, die alle Bausteine enthält.

---

## Phase 3: Umsetzung (neuer Chat – frischer Kontext)

**Warum neuer Chat?** Der Konzept-Chat hat viel Kontext verbraucht. Die Umsetzung braucht maximalen Platz für den echten Code.

**Drei Dateien hochladen:**
1. Die **funktionierende Code-Datei** (z.B. index.html) aus GitHub
2. Die **Referenzdatei** aus Phase 2
3. Die **notes.md** für Kontext

**Chat starten:**
```
Ich arbeite am [App-Name] (index.html im Upload).
Die hochgeladene Datei ist die funktionierende Basis.
Bitte NUR per str_replace ändern, nichts komplett neu schreiben.
Die Referenzdatei [Name] enthält alle fertigen Code-Bausteine.

Bitte mit Block A beginnen, dann validieren. Danach Block B, usw.
```

**Wichtige Regeln für die Umsetzung:**
- **str_replace statt Neuschreiben** – Nur gezielt ändern, nie die ganze Datei ersetzen
- **Ein Block nach dem anderen** – Nicht alles auf einmal
- **Validierung nach jedem Block** – Claude prüft ob die Datei noch valide ist
- **Bestehende Funktionen erhalten** – Keine vorhandenen Prompts, Parser oder Funktionen ändern die nicht im Konzept stehen

**Ergebnis:** Die fertige, getestete Datei zum Download.

---

## Phase 4: Abschluss

### Schritt 1: Lokal testen
Die geänderte Datei im Browser öffnen und alle Funktionen durchprüfen – sowohl neue als auch bestehende.

### Schritt 2: Code auf GitHub committen
1. In GitHub die Datei öffnen → Stift-Icon (Edit)
2. Alten Code komplett ersetzen durch neuen Code
3. Commit-Nachricht nach Schema:

```
[Typ]: [Kurze Beschreibung]
```

**Typen:**
- `feat:` → neues Feature  
  Beispiel: `feat: Vereinfachter IST-Modus mit Auswahlkarten`
- `fix:` → Bugfix  
  Beispiel: `fix: parseBewertung Regex korrigiert`
- `style:` → nur Design/CSS geändert  
  Beispiel: `style: Mobile-Ansicht verbessert`
- `content:` → Inhalte geändert (z.B. neue Beispiele)  
  Beispiel: `content: Praxisbeispiel Dokumentenklassifizierung ergänzt`
- `refactor:` → Code umgebaut, keine neue Funktion  
  Beispiel: `refactor: IST-Prompt-Routing über istSubMode`

4. "Commit directly to main" → **"Commit changes"**

### Schritt 3: notes.md aktualisieren

**Option A: Manuell** (für kleine Änderungen)
- Abgehakte Punkte mit `[x]` markieren
- Neue Features, Code-Stellen, bekannte Probleme eintragen
- Version hochzählen wenn sinnvoll

**Option B: Via Claude** (empfohlen nach größeren Änderungen)
```
Bitte aktualisiere die notes.md basierend auf dem was wir heute gemacht haben.
Hier ist die aktuelle notes.md:
[notes.md Inhalt reinkopieren]
```

Dann in GitHub committen:
```
docs: notes.md aktualisiert
```

### Schritt 4: notes.md im Claude Project ersetzen
1. Im Project auf das Zahnrad / "Project settings"
2. Die alte notes.md anklicken → löschen
3. Neue notes.md hochladen

**Faustregel:** Ersetzen wenn sich etwas Wesentliches geändert hat (neue Features, neue Code-Stellen, geänderte Prompts). Bei kleinen Fixes nicht nötig.

---

## Wann welche Phase nötig ist

| Änderung | Phase 1 (Konzept) | Phase 2 (Referenz) | Phase 3 (Umsetzung) |
|---|---|---|---|
| Neues Feature | Ja | Ja | Ja, neuer Chat |
| Größeres Refactoring | Ja | Ja | Ja, neuer Chat |
| Kleiner Bugfix | Nein | Nein | Direkt im Chat, str_replace |
| CSS-Anpassung | Nein | Nein | Direkt im Chat, str_replace |
| Prompt-Optimierung | Kurz besprechen | Nein | Direkt im Chat, str_replace |
| Neues Praxisbeispiel | Kurz besprechen | Nein | Direkt im Chat, str_replace |

**Faustregel:** Wenn die Änderung mehr als ~3 str_replace-Operationen braucht oder neue State-Variablen/Prompts einführt → Konzept-Chat + Referenzdatei nutzen.

---

## Checkliste nach jeder Session

- [ ] Lokal getestet (neue UND bestehende Funktionen)
- [ ] Code in GitHub committen (mit sinnvoller Commit-Nachricht)
- [ ] notes.md aktualisieren wenn nötig
- [ ] notes.md im Claude Project ersetzen wenn aktualisiert
- [ ] Referenzdatei aufbewahren (für spätere Nachvollziehbarkeit)
