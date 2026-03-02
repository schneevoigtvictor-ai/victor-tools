# Workflow – Neue App entwickeln

## Übersicht

```
Idee & Konzept
    ↓
App entwickeln (normaler Claude Chat)
    ↓
Version 1.0 fertig + in GitHub hochladen
    ↓
Übersichtsseite (index.html) anpassen
    ↓
NOTES.md erstellen
    ↓
Neues Claude Project anlegen
    ↓
Ab jetzt alle Weiterentwicklungen im neuen Project
```

---

## Schritt 1: App entwickeln (normaler Chat)

Kein Claude Project nötig – einfach neuen Chat starten.

Prompt-Schema für die erste Session:

```
Ich bin Business Analyst bei der SBK und entwickle Browser-Tools 
als Single-file HTML für unser Team.

Ich möchte ein neues Tool entwickeln:
[Beschreibung was das Tool machen soll]

Zielgruppe: [z.B. Business Analysten, Sachbearbeiter]
Kernfunktion: [z.B. Excel hochladen und auswerten]

Tech-Stack wie bei meinen anderen Tools:
- Single-file HTML (alles in einer Datei)
- Vanilla JS + Vanilla CSS
- Libraries nur via CDN wenn nötig
- Kein Backend
- Kommentare auf Deutsch

Bitte starte mit Version 1.0.
```

Iterativ weiterentwickeln bis Version 1.0 steht.

---

## Schritt 2: Neue App in GitHub hochladen

### Ordner und Datei anlegen:
1. Im Repo "victor-tools" → **"Add file" → "Create new file"**
2. Im Namensfeld eintippen: `[app-name]/index.html`
   - Beispiel: `meeting-protokoll/index.html`
   - GitHub erstellt den Ordner automatisch durch das `/`
3. Den kompletten Code reinkopieren
4. Commit-Nachricht: `feat: [App-Name] Version 1.0`
5. **"Commit changes"**

### Namenskonvention für Ordner:
- Kleinschreibung
- Wörter mit `-` trennen
- Kurz und beschreibend
- Beispiele: `meeting-protokoll`, `user-story-generator`, `gap-analyse`

---

## Schritt 3: Übersichtsseite anpassen (index.html)

Die Übersichtsseite im Root des Repos muss manuell oder via Claude aktualisiert werden.

### Option A: Via Claude (empfohlen)
Im Project "Victor Tools – Übersicht" → neuer Chat:

```
Ich habe ein neues Tool fertiggestellt und möchte es auf der 
Übersichtsseite ergänzen.

Neues Tool:
- Name: [App-Name]
- Beschreibung: [1 Satz was es macht]
- Link: [app-name]/index.html
- Icon: [Emoji das passt]

Hier ist der aktuelle Code der Übersichtsseite:
[index.html Code reinkopieren]

Bitte füge das neue Tool als neue Kachel hinzu.
```

### Option B: Manuell in GitHub
1. `index.html` im Root öffnen → Stift-Icon
2. Den Kachel-Block eines bestehenden Tools kopieren und anpassen:
   - Name ändern
   - Beschreibung ändern
   - Link anpassen (`href="[app-name]/index.html"`)
   - Icon/Emoji anpassen
3. Commit-Nachricht: `feat: [App-Name] zur Übersicht hinzugefügt`

---

## Schritt 4: NOTES.md erstellen

1. `_templates/NOTES-template.md` öffnen → Inhalt kopieren
2. In GitHub: **"Add file" → "Create new file"**
3. Dateiname: `[app-name]/NOTES.md`
4. Template einfügen und ausfüllen:
   - Was die App macht
   - Features der Version 1.0
   - Tech-Stack
   - Geplante nächste Schritte
   - Wichtige Code-Stellen
5. Commit-Nachricht: `docs: NOTES.md für [App-Name] erstellt`

### Tipp: Claude kann die NOTES.md befüllen
Am Ende der Entwicklungs-Session:

```
Bitte erstelle eine NOTES.md für das Tool das wir entwickelt haben.
Hier ist der fertige Code: [Code reinkopieren]
Nutze dieses Template: [NOTES-template.md Inhalt reinkopieren]
```

---

## Schritt 5: Neues Claude Project anlegen

1. claude.ai → **"Projects" → "New Project"**
2. Name: `[App-Name]` (z.B. "Meeting Protokoll")
3. `_templates/project-instructions-template.md` öffnen → kopieren
4. Bei **"Set project instructions"** einfügen und ausfüllen:
   - App-Name
   - Ordnerpfad
   - Was das Tool macht
   - Tech-Stack Besonderheiten
5. **"Add content" → "Upload files"** → NOTES.md hochladen
6. Speichern

---

## Schritt 6: Ab jetzt im neuen Project arbeiten

Alle weiteren Chats zur neuen App laufen im neuen Project.
Workflow wie in `workflow-bestehende-app.md` beschrieben.

---

## Checkliste neue App

- [ ] App entwickelt und lokal getestet
- [ ] `[app-name]/index.html` in GitHub hochgeladen
- [ ] Commit: `feat: [App-Name] Version 1.0`
- [ ] Übersichtsseite `index.html` aktualisiert
- [ ] Commit: `feat: [App-Name] zur Übersicht hinzugefügt`
- [ ] `[app-name]/NOTES.md` erstellt und befüllt
- [ ] Commit: `docs: NOTES.md für [App-Name] erstellt`
- [ ] Neues Claude Project angelegt
- [ ] Project Instructions ausgefüllt
- [ ] NOTES.md ins Project hochgeladen

---

## Namenskonventionen auf einen Blick

| Was | Schema | Beispiel |
|---|---|---|
| GitHub Ordner | kleinschreibung-mit-bindestrich | `meeting-protokoll` |
| Datei | immer index.html | `meeting-protokoll/index.html` |
| NOTES | immer NOTES.md | `meeting-protokoll/NOTES.md` |
| Claude Project | Lesbarer Name | "Meeting Protokoll" |
| Commit neue App | `feat: [Name] Version 1.0` | `feat: Meeting Protokoll Version 1.0` |
| Commit Übersicht | `feat: [Name] zur Übersicht hinzugefügt` | |
| Commit NOTES | `docs: NOTES.md für [Name] erstellt` | |
