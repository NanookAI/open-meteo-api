# CLAUDE.md

Follow the repository conventions in [AGENTS.md](AGENTS.md) — it is the single
source of truth for layout, style rules, and the pre-commit validation checklist.

Quick reminders for Claude Code sessions:

- All committed files are English-only; conversation language is unrestricted.
- Test every documented API example and variable name against the live endpoints
  (`https://api.open-meteo.com/v1/forecast?...`,
  `https://geocoding-api.open-meteo.com/v1/search?...`) before writing it down.
- The skill lives in `skills/open-meteo-api/`; edit `SKILL.md` for workflow-level
  guidance and `references/` for the variable catalog and secondary APIs.
- Default stance is the free, keyless, non-commercial API — never document a
  paid feature as if it were required.
