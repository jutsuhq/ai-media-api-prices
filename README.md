# AI video API prices

**What one second of AI video costs at every API provider, in US dollars. Updated daily.**

Seedance, Veo, Kling, Wan, Hailuo, Sora, Runway, Luma Ray, PixVerse, Vidu, LTX and more, priced at 30+ providers: the model makers' own APIs and resellers such as fal, Replicate, WaveSpeed, Runware and kie. Every price is converted to the same unit, so one table compares them all.

Browse it as a table: **[jutsustudio.com/video-api-pricing](https://jutsustudio.com/video-api-pricing/)**

## Use the data

One file, [`prices.json`](prices.json). No key, no rate limit beyond GitHub's.

```bash
curl -sL https://raw.githubusercontent.com/jutsuhq/video-api-prices/main/prices.json
```

The five cheapest 1080p Seedance offers:

```bash
curl -sL https://raw.githubusercontent.com/jutsuhq/video-api-prices/main/prices.json \
  | jq '[.rows[] | select(.family == "Seedance" and .res == "1080p" and (.gone | not))]
        | sort_by(.usdPerSec) | .[:5] | map({model, provider, usdPerSec, source})'
```

```js
const { updated, rows } = await (await fetch(
  'https://raw.githubusercontent.com/jutsuhq/video-api-prices/main/prices.json'
)).json();
```

## Fields

`updated` is the date of the last check. `rows` has one entry per offer: a model, at one provider, in one mode and resolution.

| Field | Meaning |
| --- | --- |
| `family`, `model`, `variant` | Model family (`Kling`), model (`Kling 2.1`) and variant, if any |
| `provider` | Who sells it; `official` is `true` for the model maker's own API |
| `mode` | `generate`, `video input` (edit, extend, restyle) or `motion / avatar` |
| `res`, `audio` | Output resolution and whether sound is generated |
| `usdPerSec` | US dollars per second of output video |
| `usdPer10s` | Price of a 10-second clip; `clipNote` explains when 10 s isn't allowed or is billed differently |
| `listed` | The price as the provider states it, before conversion |
| `listBefore` | List price before a running discount |
| `promo`, `promoUntil` | Temporary discount and its end date, when known |
| `source` | Page the price was read from |
| `checked`, `verified` | Date the price was last confirmed, and whether it was read from the provider directly |
| `gone` | The provider no longer offers it |
| `slug` | Model page id on jutsustudio.com |

## How prices are compared

- **Per-second** prices are used as listed.
- **Per-video** prices are divided by the clip length they buy.
- **Token** prices are converted at 16:9 and 24 fps for the stated resolution.
- **Credits** use the provider's own credit pack rate.
- **Yuan** use the day's exchange rate.

Prices are checked every day. Providers with a machine-readable price list are read automatically. The rest keep the last price confirmed by hand, with its date in `checked`. The commit history is the price history.

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Use it anywhere, credit "Jutsu Studio, [jutsustudio.com](https://jutsustudio.com/video-api-pricing/)".

Found a wrong or missing price? [Open an issue](https://github.com/jutsuhq/video-api-prices/issues).
