# Projektprofil `.claude/feature-profile.md`

Die Agenten des Plugins sind projektneutral. Alles Projektspezifische steht in dieser Datei im Repo des Projekts. Zusätzlich erwarten die Agenten im Projekt:

- **Anforderungsverzeichnis** (Standard `docs/anforderungen/`) mit `README.md` (Index, Pflegeregeln), `produktbeschreibung.md` (Rahmen/Leitplanken gegen erfundene Anforderungen), `annahmen.md`, `aenderungsprotokoll.md`, je Anforderung eine Datei.
- **Prüfliste** (Standard `.claude/requirements-checklist.md`): Format der Anforderung und nummerierte Prüfpunkte.
- **Laufbuch** (Standard `.claude/feature-log.md`): ein kurzer Abschnitt je Lauf.
- `CLAUDE.md` (Konventionen) und `CONTRIBUTING.md` (Commit-Regeln).

## Vorlage

```markdown
# Feature-Profil

## Projekt
<Name und Zweck in einem Satz>

## Pfade
- Anforderungen: docs/anforderungen/
- Prüfliste: .claude/requirements-checklist.md
- Laufbuch: .claude/feature-log.md
- Standard-Branch: main

## Befehle
- Tests gezielt: `<z. B. npx vitest run --reporter=dot <Dateien>>`
- Tests vollständig: `<npm test -- --reporter=dot>`
- Tests mit Abdeckung: `<npm run test:coverage -- --reporter=dot>`
- Build/Typprüfung: `<npm run build>`
- Basis-Messung (optional): `<Befehl und was in eine Datei geschrieben wird, z. B. Dateigrößen aus dist/assets>`

## Aufräumen nach Testläufen
<z. B. `git checkout -- .vitest/json/output.json`; sonst „keine“>

## Dokumentationspflichten (Implementierer)
- <z. B. neues Feld → Benutzerhandbuch>

## Review-Schwerpunkte (Code-Review)
- Korrekt: <fachliche Besonderheiten, Vorzeichenregeln, Datenformate>
- Performant: <z. B. Bibliothek X nur nachladen>
- Sicher: <z. B. Ausgaben escapen, CSP>
- Struktur: <wo was hingehört>

## QA-Kriterien (zusätzlich zu Tests, Abdeckung, Build, Test-Fachlichkeit)
- Mindestabdeckung: <Quelle/Werte>
- <Performance/Größe: Vergleich mit Basis-Messung, Schwelle>
- <Barrierefreiheit/Browser-Prüfung: Bedingung und Vorgehen>
- <Dokumentation/Schnittstellenvertrag>

## Besonderheiten
<alles Weitere, z. B. Mitnutzer-Hinweise>
```
