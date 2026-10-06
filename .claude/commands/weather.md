---
description: Get my current local weather
argument-hint: "[location]"
allowed-tools: Bash(curl:*)
disable-model-invocation: true
---

Get the current weather from wttr.in (no API key needed).

Location: "$ARGUMENTS" — if empty, omit it so wttr.in auto-detects the location by IP. Otherwise replace spaces with `+`.

Run:

```bash
curl -fsS -m 20 "https://wttr.in/<location>?format=%l|%C|%t|%f|%h|%w|%p"
```

The output fields are: location | conditions | temperature | feels like | humidity | wind | precipitation.

Reply with one short line, e.g. "Bogotá: light rain shower, 19°C (feels 18°C), humidity 70%, wind 10 km/h."

If curl fails, say the weather service was unreachable — don't guess the weather.
