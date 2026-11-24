# 🔒 JSON-Bereiniger für n8n-Workflows

Ein kleines Werkzeug, das **n8n-Workflow-Exporte und andere JSON-Dateien von sensiblen Daten befreit**, bevor man sie teilt oder veröffentlicht. Es läuft **komplett lokal im Browser**: Die Datei wird nirgendwohin hochgeladen.

**👉 [Direkt im Browser ausprobieren](https://matthias-ziw.github.io/n8n-json-bereiniger/)**, oder [`index.html`](index.html) herunterladen und per Doppelklick öffnen. Es funktioniert auch offline.

## Warum?

Wer n8n-Workflows teilt, etwa im Forum, im Team oder im Portfolio, gibt oft mehr preis als gedacht. Ein Export enthält zwar keine Passwörter, aber meist trotzdem:

- Namen und IDs von Zugangsdaten, Webhook- und Instanz-IDs
- interne IP-Adressen und Hostnamen
- API-Keys oder Tokens, die direkt in einem HTTP-Knoten stehen
- Testdaten (`pinData`) mit echten Inhalten
- Namen, E-Mail-Adressen oder IBANs in Prompts und Notizen

Jede dieser Stellen von Hand zu suchen ist mühsam und fehleranfällig.

## So funktioniert es

1. JSON-Datei öffnen oder ins Fenster ziehen. Links erscheint das Original mit Zeilennummern.
2. Regeln wählen und auf **Privatisieren** klicken.
3. Rechts erscheint die bereinigte Fassung. **Jede Ersetzung ist farbig markiert**, und darunter steht eine Liste aller Änderungen (Original → Ersatz).
4. Ergebnis kopieren oder speichern.

## Regeln

| Regel | Beispiel | Ersatz |
| --- | --- | --- |
| n8n-Spezifisches | Credential-ID/-Name, `webhookId`, `instanceId`, `errorWorkflow`, `pinData` | Platzhalter bzw. geleert |
| Keys, Tokens, Passwörter | Felder wie `apiKey`, `password`, Header `Authorization` | `[ENTFERNT]` |
| Token-Muster im Text | `Bearer …`, `sk-…`, `ghp_…`, JWT, lange Zufallsketten | `[TOKEN]` |
| IP-Adressen | `192.168.178.42` | `192.0.2.1` (Dokumentationsbereich) |
| Interne Hostnamen | `nas.fritz.box`, `server.local` | `host1.example.lan` |
| E-Mail-Adressen | `max@firma.de` | `nutzer1@example.com` |
| IBANs | `DE89 3704 …` | `DE00 0000 …` |
| Telefonnummern (optional) | `0931 123456` | `+49 000 …` |
| Eigene Begriffe | Namen, Firmen, Servernamen | frei wählbar, z. B. `Max Mustermann` |

## Damit der Workflow danach noch funktioniert

- **Gleicher Wert, gleicher Ersatz:** Die Verbindungen zwischen den Knoten bleiben intakt, auch wenn ein Knotenname einen ersetzten Begriff enthält.
- **n8n-Ausdrücke bleiben unangetastet:** Ein Feld wie `token: "={{ $json.token }}"` wird nicht ersetzt.
- **Gültigkeitsprüfung:** Am Ende prüft das Tool, ob das Ergebnis gültiges JSON ist.

## Grenzen

Das Tool arbeitet mit festen Regeln und ohne KI. Es erkennt typische Muster zuverlässig, aber **keine beliebigen Personen- oder Firmennamen** in Freitext, etwa in KI-Prompts oder Notizen. Dafür ist die Liste der eigenen Begriffe da. Prüf das Ergebnis vor dem Veröffentlichen immer selbst.

## Technik

Eine einzelne HTML-Datei mit reinem JavaScript. Es gibt keine Abhängigkeiten, keinen Build-Schritt und keine Netzwerkzugriffe. Die eigenen Begriffe und Einstellungen werden nur im `localStorage` des eigenen Browsers gespeichert.

---

Entstanden beim Aufbereiten meiner eigenen Workflows fürs Portfolio, zum Beispiel [n8n-briefe-automatisch-ablegen](https://github.com/Matthias-Ziw/n8n-briefe-automatisch-ablegen). Mehr auf meinem [GitHub-Profil](https://github.com/Matthias-Ziw).
