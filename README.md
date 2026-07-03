# Open-Meteo API Skill

An [Agent Skill](https://docs.claude.com/en/docs/agents-and-tools/agent-skills) that
teaches AI agents (Claude Code, Claude.ai, and other skill-compatible agents) how to
use the free [Open-Meteo](https://open-meteo.com/) weather API.

Open-Meteo blends national weather models (NOAA GFS, DWD ICON, ECMWF, Météo-France,
JMA...) into a single API — **no API key, no signup** for non-commercial use, with
forecasts up to 16 days and historical data back to 1940.

## What the skill enables

Once loaded, an agent can reliably:

- Answer "weather in X" in one step with the bundled `get_weather.py` script
  (zero dependencies, Python stdlib only)
- Resolve a place name to coordinates with the Geocoding API
- Fetch current conditions (temperature, feels-like, humidity, wind, weather code)
- Get hourly and daily forecasts up to 16 days (rain probability, UV, sunrise/sunset...)
- Interpret WMO weather codes into human-readable conditions and icons
- Pull historical weather from 1940 onward via the Archive API
- Check air quality (PM2.5, US/European AQI, pollen)
- Query marine (waves, sea temperature), elevation, flood, and climate projection APIs
- Avoid real-world pitfalls (GMT default timezone breaking daily aggregation,
  missing `results` key on failed geocoding, parallel-array response format,
  archive's ~5-day delay)

All request/response examples in the skill were verified against the live API.

## Repository layout

```
skills/
└── open-meteo-api/
    ├── SKILL.md                     # Entry point: workflow, forecast + geocoding, WMO codes, tips
    ├── scripts/
    │   └── get_weather.py           # Zero-dependency CLI: place name → formatted weather (stdlib only)
    └── references/
        ├── weather-variables.md     # Full current/hourly/daily variable catalog + WMO code table
        └── other-apis.md            # Archive, air quality, marine, elevation, flood, climate, ensemble
```

## Installation

### Claude Code (plugin marketplace — recommended)

This plugin is distributed through the [NanookAI/skills](https://github.com/NanookAI/skills)
marketplace. Inside Claude Code, run:

```
/plugin marketplace add NanookAI/skills
/plugin install open-meteo-api@nanookai-skills
```

The skill is then available in every project, and `/plugin marketplace update`
picks up new versions.

### Claude Code (per-project)

Copy the skill folder into your project's `.claude/skills/` directory:

```bash
cp -r skills/open-meteo-api /path/to/your-project/.claude/skills/
```

### Claude Code (personal, all projects)

```bash
cp -r skills/open-meteo-api ~/.claude/skills/
```

### Claude.ai / other agents

Package the skill folder (respecting `.skillignore`) into a `.skill` bundle, or
upload the `skills/open-meteo-api/` folder contents wherever your agent platform
accepts skills. The skill is self-contained; the bundled script needs only a
Python 3.8+ standard library (no packages to install), and everything else is
plain documentation.

## Try it

After installing, ask your agent things like:

- "What's the weather in Taipei right now?"
- "Will it rain in Tokyo this weekend?"
- "Compare this July's temperatures in Kaohsiung with last year's"
- "What's the UV index and air quality in Taichung today?"
- "When is sunset in Reykjavik tomorrow?"

## About the API

- Forecast endpoint: `https://api.open-meteo.com/v1/forecast`
- Free for **non-commercial** use (fair-use limit ~10,000 requests/day);
  commercial use requires a paid API plan — see [open-meteo.com/en/pricing](https://open-meteo.com/en/pricing)
- Open source (AGPLv3) and self-hostable — see [github.com/open-meteo/open-meteo](https://github.com/open-meteo/open-meteo)
- Weather data licensed under CC BY 4.0 (attribution required)

## License

The skill documentation in this repository is provided as-is. Open-Meteo itself
is an independent open-source project — see [open-meteo.com](https://open-meteo.com/)
for its license and terms of use.
