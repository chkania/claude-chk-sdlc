---
name: retro
description: Retrospektive der Agentenkette – wertet das Laufbuch aus und schlägt höchstens drei Verbesserungen vor.
disable-model-invocation: true
---

1. Lies `.claude/feature-profile.md` (Pfad des Laufbuchs). Starte den Agenten `chk-sdlc:process-retro` und nenne ihm den Pfad des Plugins: `${CLAUDE_PLUGIN_ROOT}` (dort liegen `agents/` und `skills/feature/SKILL.md`, die er lesen darf).
2. Lege dem Nutzer seine Vorschläge vor – je Vorschlag Muster, Einordnung (Plugin oder Projekt), Datei und Änderung – und frage per AskUserQuestion (Mehrfachauswahl), welche übernommen werden.
3. Übernimm nur die gewählten Vorschläge mit Edit:
   - **Projekt** (Profil, Prüfliste, Laufbuch): direkt im Projekt.
   - **Plugin:** Die installierte Plugin-Kopie ist ein Cache und wird überschrieben – ändere sie nicht. Frage nach dem Pfad des lokalen Clones von `claude-chk-sdlc` (Standard: `~/Documents/Development/claude-chk-sdlc`), ändere dort und erinnere an Versionserhöhung in `plugin.json`, Commit/Push und `claude plugin update`.
   Steht eine Änderung im Widerspruch zu `CLAUDE.md` oder zu einer anderen Agentendatei, sag das, statt sie umzusetzen.
4. Hänge die Zeile „Retro <Datum>: …“ ans Laufbuch an, ergänzt um die übernommenen Vorschläge.
5. Nicht committen; nenne dem Nutzer die geänderten Dateien.
