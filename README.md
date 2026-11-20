# claude-chk-sdlc

Marketplace mit dem Claude-Code-Plugin **chk-sdlc**: eine Agentenkette für Features.

```
/chk-sdlc:feature <Beschreibung>   Anforderung → Review → Freigabe → Umsetzung → Code-Review → QA
/chk-sdlc:retro                    Retrospektive über das Laufbuch
```

| Agent | Modell | Aufgabe |
|---|---|---|
| `requirements-author` | Haiku | formuliert die Anforderung, stellt Rückfragen |
| `requirements-reviewer` | Opus | prüft gegen Bestand und Leitplanken |
| `implementer` | Haiku | Code, Tests, Doku |
| `code-reviewer` | Sonnet | Diff-Review (korrekt, performant, sicher, sinnvoll) |
| `qa` | Sonnet | Coverage, Build, projektspezifische Kriterien, Tests ↔ Anforderung |
| `process-retro` | Sonnet | wertet das Laufbuch aus |

Der Nutzer gibt zweimal frei (Kurzfassung der Anforderung, geprüfte Anforderung), bevor implementiert wird; Push und PR nur auf Wunsch.

## Einbinden in ein Projekt

`.claude/settings.json` des Projekts:

```json
{
  "extraKnownMarketplaces": {
    "claude-chk-sdlc": { "source": { "source": "github", "repo": "chkania/claude-chk-sdlc" } }
  },
  "enabledPlugins": { "chk-sdlc@claude-chk-sdlc": true }
}
```

Beim Öffnen des Projekts fragt Claude Code, ob der Marketplace vertraut werden soll. Danach `.claude/feature-profile.md` anlegen – Format und Vorlage in [docs/PROFILE.md](docs/PROFILE.md).

Manuell: `claude plugin marketplace add chkania/claude-chk-sdlc` und `claude plugin install chk-sdlc@claude-chk-sdlc --scope project`.

## Pflege

- Änderungen an Agenten/Skills hier machen, `version` in `plugins/chk-sdlc/.claude-plugin/plugin.json` erhöhen, pushen.
- In den Projekten: `claude plugin update chk-sdlc@claude-chk-sdlc` (Auto-Update ist für eigene Marketplaces standardmäßig aus) und `/reload-plugins`.
- Lokal testen: `claude --plugin-dir ./plugins/chk-sdlc`.
- Prüfen: `claude plugin validate .`
- Projektspezifisches gehört **nicht** hierher, sondern in `.claude/feature-profile.md` des Projekts.
