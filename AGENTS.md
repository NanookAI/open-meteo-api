# Agent Instructions

This repository contains an Agent Skill that teaches AI agents how to use the
Open-Meteo weather API (https://open-meteo.com/). The deliverable is
documentation, not application code.

## Layout

- `skills/open-meteo-api/SKILL.md` — skill entry point (frontmatter, workflow, forecast + geocoding APIs, WMO codes)
- `skills/open-meteo-api/scripts/get_weather.py` — zero-dependency CLI covering the common case (place name → current + daily forecast)
- `skills/open-meteo-api/references/weather-variables.md` — full current/hourly/daily variable catalog and complete WMO weather code table
- `skills/open-meteo-api/references/other-apis.md` — archive, air quality, marine, elevation, flood, climate, and ensemble APIs

## Conventions

- **English only in all files.** No Chinese or other non-English text in any
  committed file, including examples and comments.
- **Verify before documenting.** Every request/response example and every
  variable name must be checked against the live API
  (`curl "https://api.open-meteo.com/v1/forecast?..."`) before it goes into the
  docs. Do not copy variable names or response shapes from memory — the API
  evolves and invalid variable names fail with HTTP 400.
- **Keep SKILL.md lean** (well under 500 lines). SKILL.md carries the workflow
  (geocode → fetch → interpret weather_code) plus the most-used variables;
  exhaustive catalogs and secondary endpoints belong in `references/`, with a
  clear pointer from SKILL.md telling the reader when to open them.
- **Explain the why.** When documenting a pitfall or rule, state the reason
  (e.g. "daily aggregation without `timezone` uses GMT day boundaries") so
  agents can generalize instead of pattern-matching.
- **Free tier is the default stance.** Document the keyless non-commercial API.
  Mention paid/commercial options only in passing, never as a prerequisite.
- **The script stays dependency-free.** `get_weather.py` must run on a bare
  Python 3.8+ standard library (urllib, json, argparse) so it works in any
  agent sandbox without a pip install. It covers only the common case; niche
  needs are served by documentation, not more script flags.

## Known live-API facts worth preserving

These were discovered by testing and are worth keeping accurate:

- Geocoding with no match returns a body **without a `results` key** (not an
  empty array) — check `"results" in data`.
- `daily` variables require the `timezone` parameter; `timezone=auto` resolves
  it from the coordinates.
- Hourly/daily data are parallel arrays (`time[i]` pairs with `variable[i]`),
  and units for every requested variable are echoed in `*_units`.
- Errors are HTTP 400 with `{"error": true, "reason": "<message>"}`.
- The response echoes the model grid-cell center as `latitude`/`longitude`,
  which differs slightly from the request.
- The archive endpoint lags real time by ~5 days; the forecast endpoint covers
  the recent past via `past_days` (up to 92).
- Multiple locations can be batched: comma-separated `latitude`/`longitude`
  return a JSON array of result objects.

## Validation checklist before committing

1. `grep -rnP '[\x{4e00}-\x{9fff}]' . --exclude-dir=.git` returns nothing (no CJK text).
2. Any changed example URL or variable name was re-run against the live API
   (a 200 status is enough for variable-name checks).
3. SKILL.md frontmatter still has valid `name` and `description` fields, and the
   description stays short (~1-2 sentences) while keeping trigger keywords:
   weather, forecast, temperature, rain, air quality, historical weather.
