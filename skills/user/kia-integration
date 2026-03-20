---
name: kia-integration
description: >
  Stellt die korrekte API-Anbindung an KIA (SBK-interne KI-Plattform, OpenAI-kompatibel) sicher.
  Verwende diesen Skill IMMER, wenn ein HTML-Tool, eine HTML-App oder ein Prototyp für SBK erstellt wird,
  der KIA als Backend nutzt. Trigger-Signale: "KIA anbinden", "KIA-API", "SBK Tool mit KI",
  "HTML Tool mit KIA", "neues Tool für SBK", "Anforderungs-Tool", "Sparring-Tool",
  "Chat mit KIA", "KIA-Integration", oder wenn der Nutzer ein HTML-basiertes Tool mit
  KI-Anbindung für SBK erstellen möchte. Auch verwenden, wenn ein bestehendes HTML-Tool
  um KIA-Anbindung erweitert werden soll oder wenn die KIA-Verbindung debuggt wird.
---

# KIA-Integration Skill

Dieses Skill stellt sicher, dass jede HTML-App mit KIA-Anbindung das gleiche, getestete API-Pattern verwendet. KIA ist die SBK-interne KI-Plattform – ein OpenAI-kompatibler Chat-Completions-Endpunkt.

## KIA-Eckdaten

| Parameter | Wert |
|-----------|------|
| **Base-URL** | `https://kia.apps.test.sbk.de` |
| **Endpunkt** | `/api/chat/completions` |
| **Modell** | `openai/gpt-oss-120b` |
| **Auth** | Bearer Token (vom Nutzer, gespeichert in `localStorage`) |
| **Format** | OpenAI Chat Completions (messages-Array mit system/user/assistant) |
| **Streaming** | `stream: false` (Standard), `stream: true` optional |

## Wann welchen Modus verwenden

Der Skill bietet zwei Modi:

1. **Komplettes HTML-Grundgerüst** – Wenn ein neues Tool von Grund auf gebaut wird. Enthält Token-Eingabe, UI-Grundstruktur, API-Anbindung und Fehlerbehandlung.
2. **Nur API-Snippet** – Wenn KIA in ein bestehendes HTML-Tool eingebaut werden soll. Enthält nur die JS-Funktionen für Config, Token-Handling und API-Call.

Frage den Nutzer, welchen Modus er braucht, wenn es nicht eindeutig ist.

## Modus 1: Komplettes HTML-Grundgerüst

Verwende dieses Grundgerüst als Startpunkt für jedes neue SBK-HTML-Tool mit KIA-Anbindung. Die Struktur folgt dem bewährten Pattern aus dem Anforderungs-Assistenten.

```html
<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>[TOOL-NAME] · SBK</title>
<style>
  /* === Grundlegendes Reset & SBK-Styling === */
  *, *::before, *::after { margin:0; padding:0; box-sizing:border-box; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    background: #F7FAFC; color: #1A202C; font-size: 15px; line-height: 1.65;
  }

  /* === Token-Eingabe === */
  .token-card {
    background: #fff; border: 1px solid #E2E8F0; border-radius: 12px;
    padding: 24px; box-shadow: 0 1px 3px rgba(0,0,0,.06); max-width: 480px; margin: 40px auto;
  }
  .token-card h4 { font-size: 14px; font-weight: 700; margin-bottom: 10px; }
  .tok-row { display: flex; gap: 8px; }
  .tok-row input {
    flex: 1; padding: 10px 14px; border: 1.5px solid #E2E8F0; border-radius: 8px;
    font-family: inherit; font-size: 14px; outline: none;
  }
  .tok-row input:focus { border-color: #3182CE; box-shadow: 0 0 0 3px rgba(49,130,206,.1); }
  .tok-hint { font-size: 12px; color: #A0AEC0; margin-top: 6px; }
  .tok-err { font-size: 12px; color: #C53030; margin-top: 5px; display: none; }
  .tok-err.on { display: block; }

  /* === Buttons === */
  .btn-primary {
    background: #2B6CB0; color: #fff; border: none; border-radius: 8px;
    padding: 10px 22px; font-family: inherit; font-size: 14px; font-weight: 700;
    cursor: pointer; transition: background .2s;
  }
  .btn-primary:hover { background: #2C5282; }
  .btn-primary:disabled { opacity: .4; cursor: not-allowed; }

  /* === Lade-Indikator === */
  .loading { display: none; align-items: center; gap: 10px; padding: 12px; font-size: 13px; color: #718096; }
  .loading.on { display: flex; }
  .spin {
    width: 16px; height: 16px; border: 2.5px solid #E2E8F0;
    border-top-color: #2B6CB0; border-radius: 50%; animation: spin .7s linear infinite;
  }
  @keyframes spin { to { transform: rotate(360deg); } }

  /* === Toast === */
  .toast {
    position: fixed; bottom: 20px; right: 20px; background: #002B52; color: #fff;
    border-radius: 8px; padding: 11px 18px; font-size: 13px; font-weight: 500;
    box-shadow: 0 10px 25px rgba(0,0,0,.06); transform: translateY(44px);
    opacity: 0; transition: all .3s cubic-bezier(.34,1.56,.64,1); z-index: 600;
  }
  .toast.on { transform: translateY(0); opacity: 1; }
  .toast.ok { background: #16A34A; }
  .toast.warn { background: #C05621; }
  .toast.err { background: #C53030; }
</style>
</head>
<body>

<!-- Token-Eingabe -->
<div class="token-card" id="tokenCard">
  <h4>KIA Token eingeben</h4>
  <div class="tok-row">
    <input type="password" id="tokIn" placeholder="Token aus KIA → Einstellungen → Konto"
           onkeydown="if(event.key==='Enter')saveToken()" autocomplete="off">
    <button class="btn-primary" onclick="saveToken()">Verbinden</button>
  </div>
  <div class="tok-hint">Das Token wird nur lokal in deinem Browser gespeichert.</div>
  <div class="tok-err" id="tokErr"></div>
</div>

<!-- Hauptbereich (nach Token-Eingabe sichtbar) -->
<div id="mainApp" style="display:none;">
  <!-- HIER: Tool-spezifisches UI -->
</div>

<!-- Toast -->
<div class="toast" id="toast"></div>

<script>
/* ══════════════════════════════════════════
   KIA API – Konfiguration
   ══════════════════════════════════════════ */
const KIA_BASE = 'https://kia.apps.test.sbk.de';
const KIA_MODEL = 'openai/gpt-oss-120b';

/* Standard-Stilanweisung für KIA-Prompts (optional, anpassbar) */
const KIA_STYLE = `Schreibe in einem klaren, strukturierten und praxisnahen Stil. Verständlich und menschlich, ohne formell oder bürokratisch zu sein. Einfache, direkte Sätze. Lösungsorientiert. Nummerierte Listen für Rückfragen. Sachlich, ruhig, kollegial, wertschätzend. Immer Du-Form. Keine Emojis. Verwende **Fettdruck** für wichtige Begriffe.`;

/* ══════════════════════════════════════════
   Token-Verwaltung
   ══════════════════════════════════════════ */
let apiToken = '';

function saveToken() {
  const v = document.getElementById('tokIn').value.trim();
  const err = document.getElementById('tokErr');
  if (!v) {
    err.textContent = 'Bitte KIA Token eingeben.';
    err.classList.add('on');
    return;
  }
  apiToken = v;
  err.classList.remove('on');
  try { localStorage.setItem('kia-token', v); } catch(e) {}
  showToast('Verbunden', 'ok');
  document.getElementById('tokenCard').style.display = 'none';
  document.getElementById('mainApp').style.display = '';
}

/* Token aus localStorage wiederherstellen */
try {
  const saved = localStorage.getItem('kia-token');
  if (saved) {
    document.getElementById('tokIn').value = saved;
    apiToken = saved;
    /* Optional: Direkt zum Hauptbereich springen */
    // document.getElementById('tokenCard').style.display = 'none';
    // document.getElementById('mainApp').style.display = '';
  }
} catch(e) {}

/* ══════════════════════════════════════════
   KIA API-Aufruf
   ══════════════════════════════════════════ */

/**
 * Ruft die KIA Chat Completions API auf.
 *
 * @param {Array} messages  - Array von {role, content}-Objekten (user/assistant)
 * @param {string} systemPrompt - System-Prompt für den Kontext
 * @returns {Promise<string>} - Die Textantwort von KIA
 * @throws {Error} bei Netzwerk- oder API-Fehlern
 */
async function callKIA(messages, systemPrompt) {
  const response = await fetch(KIA_BASE + '/api/chat/completions', {
    method: 'POST',
    headers: {
      'Authorization': 'Bearer ' + apiToken,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      model: KIA_MODEL,
      messages: [
        { role: 'system', content: systemPrompt },
        ...messages
      ],
      stream: false
    })
  });

  if (!response.ok) {
    const errText = await response.text();
    throw new Error('KIA ' + response.status + ': ' + errText.substring(0, 200));
  }

  const data = await response.json();
  const content = data.choices?.[0]?.message?.content;
  if (!content) throw new Error('Leere Antwort von KIA.');
  return content;
}

/* ══════════════════════════════════════════
   Hilfsfunktionen
   ══════════════════════════════════════════ */
function showToast(msg, type) {
  const el = document.getElementById('toast');
  el.textContent = msg;
  el.className = 'toast on' + (type ? ' ' + type : '');
  clearTimeout(el._t);
  el._t = setTimeout(() => el.classList.remove('on'), 3500);
}

function showLoading(id) {
  const el = document.getElementById(id);
  if (el) el.classList.add('on');
}

function hideLoading(id) {
  const el = document.getElementById(id);
  if (el) el.classList.remove('on');
}
</script>
</body>
</html>
```

## Modus 2: Nur API-Snippet (zum Einbauen)

Wenn KIA in ein bestehendes HTML-Tool integriert werden soll, verwende diesen JS-Block. Er kann in ein bestehendes `<script>`-Tag eingefügt werden.

```javascript
/* ══════════════════════════════════════════
   KIA API – Konfiguration
   ══════════════════════════════════════════ */
const KIA_BASE = 'https://kia.apps.test.sbk.de';
const KIA_MODEL = 'openai/gpt-oss-120b';
let apiToken = '';

/* Token aus localStorage laden (falls vorhanden) */
try {
  const saved = localStorage.getItem('kia-token');
  if (saved) apiToken = saved;
} catch(e) {}

/* Token speichern */
function saveKiaToken(token) {
  apiToken = token;
  try { localStorage.setItem('kia-token', token); } catch(e) {}
}

/* ══════════════════════════════════════════
   KIA API-Aufruf
   ══════════════════════════════════════════ */

/**
 * Ruft die KIA Chat Completions API auf.
 *
 * @param {Array} messages  - Array von {role, content}-Objekten
 * @param {string} systemPrompt - System-Prompt
 * @returns {Promise<string>} - Textantwort von KIA
 */
async function callKIA(messages, systemPrompt) {
  const response = await fetch(KIA_BASE + '/api/chat/completions', {
    method: 'POST',
    headers: {
      'Authorization': 'Bearer ' + apiToken,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({
      model: KIA_MODEL,
      messages: [
        { role: 'system', content: systemPrompt },
        ...messages
      ],
      stream: false
    })
  });

  if (!response.ok) {
    const errText = await response.text();
    throw new Error('KIA ' + response.status + ': ' + errText.substring(0, 200));
  }

  const data = await response.json();
  const content = data.choices?.[0]?.message?.content;
  if (!content) throw new Error('Leere Antwort von KIA.');
  return content;
}
```

## Wichtige Regeln bei der Verwendung

Diese Regeln gelten immer, egal welcher Modus:

1. **Endpoint-Pfad ist exakt `/api/chat/completions`** – nicht `/v1/chat/completions` wie bei OpenAI direkt. Das ist der häufigste Fehler.

2. **Bearer Token kommt vom Nutzer** – niemals hardcoden. Immer über UI-Eingabe + `localStorage` lösen. Der Nutzer findet sein Token unter: KIA → Einstellungen → Konto.

3. **Modell ist `openai/gpt-oss-120b`** – mit dem `openai/`-Prefix. Nicht einfach `gpt-oss-120b`.

4. **Stream ist standardmäßig `false`** – nur auf `true` setzen, wenn das Tool Streaming-UX braucht (z.B. progressiver Textaufbau). Streaming erfordert zusätzlichen Code für `ReadableStream`-Parsing.

5. **System-Prompt immer als erstes Message** – `{role: 'system', content: systemPrompt}` steht immer an Position 0 im messages-Array, gefolgt von der user/assistant-History.

6. **Fehlerbehandlung ist Pflicht** – Jeder `callKIA`-Aufruf muss in `try/catch` stehen. Fehlermeldungen dem Nutzer als Toast oder Inline-Hinweis anzeigen, nie als `alert()`.

7. **KIA-Stilanweisung (KIA_STYLE)** – Der Standard-Tonfall für SBK-Tools ist: klar, strukturiert, praxisnah, Du-Form, keine Emojis, Fettdruck für Schlüsselbegriffe. Diese Stilanweisung kann als Präfix in System-Prompts eingebaut werden. Anpassbar je nach Tool.

## Anwendungsbeispiel: API-Aufruf in einem Tool

So sieht ein typischer Aufruf in einem SBK-Tool aus:

```javascript
const SYSTEM_PROMPT = `${KIA_STYLE}

Du bist ein Assistent der SBK. Deine Aufgabe: ...`;

let chatHistory = [];

async function handleUserInput(userText) {
  chatHistory.push({ role: 'user', content: userText });

  showLoading('loader');
  try {
    const answer = await callKIA(chatHistory, SYSTEM_PROMPT);
    chatHistory.push({ role: 'assistant', content: answer });
    displayAnswer(answer);
  } catch (e) {
    showToast('Fehler: ' + e.message, 'err');
  } finally {
    hideLoading('loader');
  }
}
```

## Checkliste vor Fertigstellung

Bevor ein Tool an den Nutzer übergeben wird, diese Punkte prüfen:

- [ ] `KIA_BASE` und `KIA_MODEL` sind korrekt gesetzt
- [ ] Token-Eingabe mit `localStorage`-Persist vorhanden
- [ ] `callKIA()` nutzt exakt das Pattern aus diesem Skill
- [ ] Jeder API-Aufruf ist in `try/catch` eingewickelt
- [ ] Lade-Indikator während API-Aufruf sichtbar
- [ ] Fehlermeldungen werden dem Nutzer angezeigt (Toast/Inline)
- [ ] System-Prompt enthält KIA_STYLE oder angepasste Stilanweisung
- [ ] `stream: false` ist explizit gesetzt (oder Streaming korrekt implementiert)
