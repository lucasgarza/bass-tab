# Songs folder

Song files placed here show up in every device's library with a **Repo** badge.

1. Add a song file, e.g. `my-riff.json` (format below).
2. List its file name in `index.json`: `["my-riff.json"]`.
3. Push. GitHub Pages republishes in about a minute.

A file that fails validation is skipped silently, so check it by importing it once in the app first. The import screen lists every problem.

## Song format (schemaVersion 1)

All `beat` and `dur` values are measured in **quarter notes**, whatever the time signature.

| Field | Type | Required | Rule |
|---|---|---|---|
| `schemaVersion` | number | yes | Always `1`. |
| `id` | string | yes | Lowercase slug you choose, e.g. `"my-riff"`. Never auto-generated at random. Must be unique in the library. |
| `title` | string | yes | |
| `artist` | string | no | |
| `source.type` | string | no | `guitarpro`, `musicxml`, `midi`, `json`, `repo` or `builtin`. Set by the app on import. |
| `source.file` | string | no | Original file name. |
| `tuning.name` | string | no | Filled in by the app if missing (`Standard`, `Drop D`, `5-string standard`, …, otherwise `Custom`). |
| `tuning.strings` | string[] | yes | 4–6 note names, **highest string first**, e.g. `["G2","D2","A1","E1"]`. |
| `timeSignature.beats` | integer | yes | 1–32. |
| `timeSignature.beatUnit` | integer | yes | 1, 2, 4, 8, 16 or 32. |
| `tempo.bpm` | number | yes | Quarter-note BPM, 20–400. |
| `tempo.changes[]` | object[] | no | `{ "measure": 9, "beat": 0, "bpm": 100 }`. `measure` is 1-based. |
| `sections[]` | object[] | no | `{ "name": "Verse", "startMeasure": 5 }`. Shown on the tab and in the loop picker. |
| `measures[].n` | integer | yes | 1, 2, 3, … in order. |
| `measures[].timeSignature` | object | no | Changes the time signature from this measure onward. |
| `measures[].notes[]` | object[] | yes | An empty list is a full-bar rest. Rests are just gaps. |
| `notes[].string` | integer | yes | 0 = first entry of `tuning.strings` (top line of the tab). |
| `notes[].fret` | integer | yes | 0–30. 0 = open string. |
| `notes[].beat` | number | yes | Start within the measure: 0 = downbeat, 0.5 = the "and", 0.25 / 0.75 = 16ths. |
| `notes[].dur` | number | yes | 1 = quarter, 0.5 = 8th, 0.25 = 16th, 1.5 = dotted quarter. |
| `notes[].tieToNext` | boolean | no | On the first note of a tie. |
| `notes[].tied` | boolean | no | On the continuation note: shown as `(7)`, not re-plucked. |
| `notes[].dead` | boolean | no | Shown as `x`. |
| `notes[].slide` | string | no | `in-below`, `in-above`, `out-down`, `out-up` or `to-next`. |

## Example

```json
{
  "schemaVersion": 1,
  "id": "example-groove",
  "title": "Example Groove",
  "artist": "Example Artist",
  "tuning": { "name": "Standard", "strings": ["G2", "D2", "A1", "E1"] },
  "timeSignature": { "beats": 4, "beatUnit": 4 },
  "tempo": { "bpm": 126, "changes": [] },
  "sections": [{ "name": "Verse", "startMeasure": 2 }],
  "measures": [
    { "n": 1, "notes": [] },
    { "n": 2, "notes": [
      { "string": 2, "fret": 0, "beat": 0,    "dur": 0.5 },
      { "string": 2, "fret": 0, "beat": 0.75, "dur": 0.25 },
      { "string": 3, "fret": 3, "beat": 1,    "dur": 0.5 },
      { "string": 1, "fret": 2, "beat": 2.5,  "dur": 0.5, "slide": "in-below" },
      { "string": 2, "fret": 4, "beat": 3.5,  "dur": 0.5, "tieToNext": true }
    ]},
    { "n": 3, "notes": [
      { "string": 2, "fret": 4, "beat": 0,    "dur": 1, "tied": true },
      { "string": 2, "fret": 0, "beat": 2,    "dur": 0.25, "dead": true },
      { "string": 3, "fret": 0, "beat": 3,    "dur": 1 }
    ]}
  ]
}
```
