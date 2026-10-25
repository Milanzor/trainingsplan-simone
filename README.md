# trainingsplan-simone

Public GitHub Pages site (Dutch) showing Simone's running progress, today's session and
the outlook for the coming days. Maintained by the running-coach agent
(`Milanzor/agent.postvak7.nl.running-coach`); the source of truth is `simone/PLAN.md`
there. This repo only holds the published view.

Site: https://milanzor.github.io/trainingsplan-simone/

## How it works

`index.html` is static and renders everything from **`data.json`**. Each morning the
routine edits only `data.json`, then commits and pushes. Pages deploys `main` from the
root folder (`.nojekyll`, no build step).

## data.json

```jsonc
{
  "bijgewerkt": "2026-10-26T07:20:00+01:00",   // ISO timestamp of this update
  "melding": "…",                              // shown when `vandaag` is null (optional)
  "doel": { "titel": "…", "toelichting": "…" },
  "weekdoel": { "runs": 3, "km": 20 },         // or null; km may be null
  "racedatum": "2027-04-18",                   // countdown in the "Opbouw" section
  "fases": [ { "naam": "Fase 1 · …", "van": "2026-10-26", "tot": "2026-12-20", "omschrijving": "…" } ],
  "vandaag": {                                 // or null
    "datum": "2026-10-26", "titel": "Rustige duurloop 30 min", "type": "easy",
    "beschrijving": "Rustig tempo, je moet kunnen praten.", "doel": "Basis opbouwen",
    "status": "gepland", "in_garmin": true   // in_garmin optional
  },
  "vooruitblik": [ { "wanneer": "Morgen", "titel": "Rust of wandelen", "type": "rust" } ],
  "logboek": [                                 // every day with a prescription; newest last or first
    { "datum": "2026-10-26", "titel": "Rustige duurloop", "type": "easy",
      "afstand_km": 4.8, "duur_min": 31.5, "tempo": "6:34", "status": "gedaan",
      "terugblik": "Alle acht stukjes hardlopen netjes uitgelopen, en in een rustig tempo. Precies goed! Volgende keer hetzelfde, daarna een stapje verder." }
  ]
}
```

- `type`: `easy` · `kwaliteit` · `lang` · `rust` · `kracht` · `anders`
- `status`: `gepland` · `gedaan` · `overgeslagen` · `geschrapt`
- Progress stats and the weekly-km chart are computed in the page from `logboek`
  (entries with `status: "gedaan"` and `afstand_km > 0`).

## Rules (public page)

- **Dutch, written for Simone**: short, friendly, no jargon (no "Z2", "LTHR", "ACWR").
  Describe effort in plain words ("rustig, je kunt praten").
- **Garmin:** set `vandaag.in_garmin: true` only when today's session is actually scheduled in
  her Garmin; the page then shows a "Staat in je Garmin" badge. Omit it on rest days or if the push failed.
- **Keep it short.** Titles ≤ ~40 characters, `beschrijving` one or two short sentences,
  `vooruitblik.titel` a few words. The page shows only the current phase's `omschrijving`;
  `doel.toelichting` and `vandaag.doel` are kept in the data but not shown.
- **Terugblik (coach's assessment) on every completed run.** When the routine closes out a
  run, it writes `logboek[].terugblik`: 2–3 short sentences in Dutch, addressed to Simone
  ("je"), from her Garmin data only. Say what went well (all run parts completed, steady pace,
  relaxed effort), give at most one concrete tip, and what comes next ("volgende keer
  hetzelfde" / "volgende keer een stapje verder"). Warm and encouraging, never patronizing, no
  jargon, no heart-rate numbers, nothing about weight. For a skipped day leave it out:
  no comment on missed sessions. The page shows it under the entry as a "Coach" note.
- **Training data only.** Never publish HRV, resting heart rate, sleep, readiness, Body
  Battery, weight, illness/injury details, or her surname. A reason like "na een drukke
  week rustig aan" is fine; "HRV laag" is not.
- Activity IDs and Garmin links stay out of the page.
