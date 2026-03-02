# Victor Tools

Kleine, praktische Browser-Tools für den Arbeitsalltag.  
Kein Login, keine Installation – einfach öffnen und loslegen.

🌐 **[Zur Übersichtsseite](https://schneevoigtvictor-ai.github.io/victor-tools/)**

---

## Verfügbare Tools

| Tool | Beschreibung |
|---|---|
| [Anforderungs-Assistent](anforderungs-assistent/) | Iterativer Sparringspartner zum Erarbeiten von JIRA-Anforderungen |
| [API-Katalog Manager](api-katalog-manager/) | Excel-Kataloge durchsuchen und Quartalsvergleiche als PowerPoint exportieren |
| [KI-Usecase-Einordnung](ki-usecase-einordnung/) | Quiz-Tool zum Einordnen von KI-Use-Cases in 6 Kategorien |

---

## Aufbau des Repos

```
/[tool-name]/
    index.html     ← Das Tool (Single-file HTML)
    NOTES.md       ← Dokumentation, Stand, geplante Änderungen
/_templates/
    NOTES-template.md
    project-instructions-template.md
    workflow-neue-app.md
    workflow-bestehende-app.md
README.md
index.html         ← Übersichtsseite
```

---

## Technologie

Alle Tools sind bewusst einfach gehalten:
- Single-file HTML – alles in einer Datei, kein Build-Schritt
- Vanilla JS + Vanilla CSS – keine Frameworks
- Externe Libraries nur via CDN wenn nötig
- Kein Backend (außer wo explizit angegeben)

---

## Neues Tool hinzufügen

Siehe `_templates/workflow-neue-app.md`
