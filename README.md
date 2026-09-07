# kurunthogai-api

The 401 poems of **_Kurunthogai_** (குறுந்தொகை) — one of the *Ettuthogai* (எட்டுத்தொகை,
"Eight Anthologies") of classical Tamil **Sangam literature** — published as a single,
static JSON document.

This is a **data-only** repository. There is no server, build step, test suite, or
dependency. The "API" is the JSON file itself: fetch it, cache it, ship it.

## About the text

*Kurunthogai* is an anthology of 401 short *akam* (அகம், "interior" / love) poems,
ranging from 4 to 8 lines each, composed by roughly 205 poets. Each poem is set in one
of the five *thinai* (திணை) — poetic landscapes that pair a physical terrain with a
phase of love.

| Thinai | Landscape | Mood / phase of love | Poems |
|--------|-----------|----------------------|------:|
| குறிஞ்சி (Kurinji) | mountains | union of lovers | 149 |
| பாலை (Paalai) | wasteland / drought | separation, elopement | 87 |
| நெய்தல் (Neithal) | seashore | anxious waiting, pining | 73 |
| மருதம் (Marutham) | farmland / river valley | lovers' quarrels, infidelity | 48 |
| முல்லை (Mullai) | forest / pastoral | patient waiting | 44 |

> The Neithal count above is 72 + 1: one record is mis-encoded (see [Known data issues](#known-data-issues)).

## Repository layout

```
json/KurunthogaiPoems.json   the entire dataset (all 401 poems)
CLAUDE.md                     guidance for AI coding assistants
LICENSE                       GNU GPL v3.0
```

## Data model

The file is a single JSON object with one key, `KurunthogaiPoems`, whose value is an
array of 401 poem objects. Every object has **exactly these four keys**:

| Key | Type | Description |
|-----|------|-------------|
| `index` | integer | Poem number, `1`–`401`. Contiguous and in order. |
| `poem_verses` | string | Full Tamil verse text. **Line breaks are not preserved** — lines run together. |
| `poet_name` | string | Attributed poet, in Tamil. Empty string `""` when the poet is unknown or unattributed. |
| `poem_thinai_type` | string | One of five *thinai*: `குறிஞ்சி`, `முல்லை`, `மருதம்`, `நெய்தல்`, `பாலை`. |

### Example object

```json
{
  "index": 1,
  "poem_verses": "செங்களம் படக் கொன்று அவுணர்த் தேய்த்தசெங் கோல் அம்பின், செங் கோட்டு யானை,கழல் தொடி, சேஎய் குன்றம்குருதிப் பூவின் குலைக் காந்தட்டே.",
  "poet_name": "திப்புத்தோளார்",
  "poem_thinai_type": "குறிஞ்சி"
}
```

### Top-level shape

```json
{
  "KurunthogaiPoems": [
    { "index": 1, "poem_verses": "...", "poet_name": "...", "poem_thinai_type": "..." },
    { "index": 2, "poem_verses": "...", "poet_name": "...", "poem_thinai_type": "..." }
  ]
}
```

## Known data issues

Verify before relying on exact counts:

- **Six thinai buckets instead of five.** One record (`index` 324) uses `நெய்தல`
  — missing the final pulli (்) — instead of `நெய்தல்`. A naive group-by will split
  Neithal into two buckets.
- **11 poems have an empty `poet_name`** (`""`), where the poet is unknown or
  unattributed.

## Usage

### Fetch the raw file

```bash
curl -L -o KurunthogaiPoems.json \
  https://raw.githubusercontent.com/velram/kurunthogai-api/main/json/KurunthogaiPoems.json
```

### JavaScript

```js
const res = await fetch(
  "https://raw.githubusercontent.com/velram/kurunthogai-api/main/json/KurunthogaiPoems.json"
);
const { KurunthogaiPoems } = await res.json();

const poem = KurunthogaiPoems.find((p) => p.index === 1);
console.log(poem.poem_verses);
```

### Python

```python
import json, urllib.request

url = "https://raw.githubusercontent.com/velram/kurunthogai-api/main/json/KurunthogaiPoems.json"
poems = json.load(urllib.request.urlopen(url))["KurunthogaiPoems"]

by_thinai = {}
for p in poems:
    by_thinai.setdefault(p["poem_thinai_type"], []).append(p["index"])
```

## Contributing

When editing `json/KurunthogaiPoems.json`:

- Preserve **UTF-8** encoding.
- Keep `index` values contiguous and ordered (`1`–`401`).
- Keep all **four keys** on every object.
- Validate before committing:

  ```bash
  python3 -m json.tool json/KurunthogaiPoems.json > /dev/null
  ```

Corrections to the verse text, poet attributions, or the `நெய்தல` typo are welcome via
pull request.

## License

Released under the **GNU General Public License v3.0**. See [LICENSE](LICENSE).

The underlying poems of *Kurunthogai* are classical works in the public domain.
