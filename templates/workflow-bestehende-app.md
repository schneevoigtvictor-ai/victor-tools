# Workflow – Bestehende App weiterentwickeln

## Übersicht

```
GitHub (aktueller Code)
    ↓ Code reinkopieren
Claude Project Chat
    ↓ Änderung entwickeln + testen
GitHub committen
    ↓
NOTES.md aktualisieren (manuell oder via Claude)
    ↓
NOTES.md im Claude Project ersetzen
```

---

## Schritt 1: Chat starten

Im richtigen Claude Project → "New Chat" → so einsteigen:

```
Ich arbeite heute an [App-Name].

Was ich vorhabe:
[Konkrete Aufgabe, z.B. "Neues Feature X hinzufügen"]

Hier ist der aktuelle Code:
[Kompletten Code aus GitHub reinkopieren]
```

---

## Schritt 2: Änderung entwickeln

Claude gibt die komplette geänderte Datei aus.
Lokal im Browser testen (einfach die HTML-Datei öffnen).

---

## Schritt 3: Commit auf GitHub

1. In GitHub die Datei öffnen → Stift-Icon (Edit)
2. Alten Code komplett ersetzen durch neuen Code
3. Scroll runter zu "Commit changes"
4. **Commit-Nachricht schreiben** (immer nach diesem Schema):

```
[Typ]: [Kurze Beschreibung]
```

**Typen:**
- `feat:` → neues Feature  
  Beispiel: `feat: Aktionssteuerung-Wizard implementiert`
- `fix:` → Bugfix  
  Beispiel: `fix: Token-Validierung korrigiert`
- `style:` → nur Design/CSS geändert  
  Beispiel: `style: Mobile-Ansicht verbessert`
- `content:` → Inhalte geändert (z.B. neue Use Cases)  
  Beispiel: `content: 5 neue Use Cases hinzugefügt`
- `refactor:` → Code umgebaut, keine neue Funktion  
  Beispiel: `refactor: Parsing-Logik vereinfacht`

5. "Commit directly to main" → **"Commit changes"**

---

## Schritt 4: NOTES.md aktualisieren

### Option A: Manuell (für kleine Änderungen)
Einfach selbst in GitHub bearbeiten:
- Abgehakte Punkte unter "Geplante Änderungen" mit `[x]` markieren
- Neue geplante Änderungen eintragen
- Bekannte Probleme ergänzen oder entfernen
- Version hochzählen wenn sinnvoll (z.B. 1.0 → 1.1)

### Option B: Via Claude (empfohlen nach größeren Änderungen)
Am Ende der Session Claude fragen:

```
Bitte aktualisiere die NOTES.md basierend auf dem was wir heute gemacht haben.
Hier ist die aktuelle NOTES.md:
[NOTES.md Inhalt reinkopieren]
```

Claude gibt die aktualisierte NOTES.md aus → kopieren → in GitHub einfügen → committen:
```
docs: NOTES.md aktualisiert
```

---

## Schritt 5: NOTES.md im Claude Project ersetzen

**Ja, die alte Datei ersetzen** – sonst hat Claude veralteten Kontext.

So geht's im Project:
1. Im Project auf das Zahnrad / "Project settings"
2. Die alte NOTES.md anklicken → löschen
3. Neue NOTES.md hochladen

**Faustregel:** NOTES.md im Project ersetzen wenn sich etwas Wesentliches geändert hat:
- Neue Features implementiert
- Geplante Änderungen abgehakt
- Neue bekannte Probleme aufgetaucht
- Wichtige Code-Stellen dazugekommen

Bei kleinen Fixes (Bugfix, CSS-Anpassung) ist es nicht nötig.

---

## Kurzübersicht: Was passiert wo

| Was | Wo | Wann |
|---|---|---|
| Geänderter Code | GitHub committen | Nach jeder Session |
| Commit-Nachricht | GitHub "Commit changes" | Mit Typ-Präfix (feat/fix/...) |
| NOTES.md ändern | GitHub (manuell oder via Claude) | Nach größeren Änderungen |
| NOTES.md im Project ersetzen | Claude Project Settings | Wenn NOTES.md geändert wurde |

---

## Checkliste nach jeder Session

- [ ] Code in GitHub committen (mit sinnvoller Commit-Nachricht)
- [ ] NOTES.md aktualisieren wenn nötig
- [ ] NOTES.md im Claude Project ersetzen wenn aktualisiert
