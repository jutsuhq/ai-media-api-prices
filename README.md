# AI video API prices

**What one second of AI video costs at every API provider, in US dollars. Updated daily.**

Seedance, Veo, Kling, Wan, Hailuo, Sora, Runway, Luma Ray, PixVerse, Vidu, LTX and more, priced at 75+ providers: the model makers' own APIs (including the mainland-China platforms of Kling, MiniMax, PixVerse, Vidu, Volcengine and Alibaba), resellers such as fal, Replicate, WaveSpeed, Runware and kie, and the API relays built on new-api. Every price is converted to the same unit, so one table compares them all.

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
| `provider` | Who sells it; `official` is `true` only for the model maker's own API (Runway reselling Veo is not official) |
| `mode` | `generate`, `video input` (edit, extend, restyle) or `motion / avatar` |
| `res`, `audio` | Output resolution and whether sound is generated |
| `usdPerSec` | US dollars per second of output video |
| `usdPer10s` | Price of a 10-second clip; `clipNote` explains when 10 s isn't allowed or is billed differently |
| `listed` | The price as the provider states it, before conversion |
| `listBefore` | List price before a running discount |
| `promo`, `promoUntil` | Temporary discount and its end date, when known |
| `source` | Page the price was read from |
| `checked`, `verified` | Date the price was last confirmed, and whether it was read from the provider directly |
| `changed`, `was` | Date the shown price last changed, and the price per second before that change |
| `gone` | The provider no longer offers it |
| `slug` | Model page id on jutsustudio.com |

## How prices are compared

- **Per-second** prices are used as listed.
- **Per-video** prices are divided by the clip length they buy.
- **Token** prices are converted at 16:9 and 24 fps for the stated resolution.
- **Credits** use the provider's own credit pack rate.
- **Yuan** are converted at 7.1 CNY per US dollar.
- **Relay quota** (new-api relays): a relay's "$1" of quota is often sold for ¥1, not for a dollar. Prices are converted at the top-up rate the relay publishes in `/api/status` (yuan per quota dollar ÷ 7.1), so they are what you actually pay.
- **Relay channels**: an offer more than 70% below the model maker's own price is tagged `variant: "relay channel"` (usually pooled or reverse-engineered accounts). It stays in the data, but the site doesn't quote it as the cheapest price.

Every source page is re-read every day:

- **Exact feeds** update rows from the provider's own price data: OpenRouter, Higgsfield, xAI, Runware, Atlas Cloud and CometAPI (their public pricing JSON), and every new-api relay (`/api/pricing`, where the price is recomputed from the model and group ratios).
- **Every other page** is checked for the numbers it shows as prices. A row is confirmed when its price is still on the page, or when no price on the page changed. When exactly one price on a page changed, the rows listed at the old price are updated to the new one.
- Anything else keeps its last confirmed date in `checked` and is flagged for a person to read.

## Price changes

[`changes.jsonl`](changes.jsonl) logs every price the daily run changed, one JSON object per line: `date`, `provider`, `model`, `mode`, `resolution`, `audio`, `usd_per_second_before`, `usd_per_second`, `source_url`, and `how` (`feed` or `page`). The commit history of `prices.json` holds everything before the log started on 2026-10-31.

## Related

Other open-source projects from [Jutsu Studio](https://jutsustudio.com/):

- [Jutsu Studio](https://github.com/jutsuhq/jutsu-studio): the open-source Higgsfield desktop app for Mac · [jutsustudio.com](https://jutsustudio.com/)
- [Swapjutsu](https://github.com/jutsuhq/swapjutsu-seedance-character-swap): free character swap for Mac, no watermark · [jutsustudio.com/swapjutsu](https://jutsustudio.com/swapjutsu/)
- [Kawarimi](https://github.com/jutsuhq/kawarimi-local-character-swap): free local AI character swap for Windows and Linux, no cloud · [jutsustudio.com/kawarimi](https://jutsustudio.com/kawarimi/)
- [Content Factory](https://github.com/jutsuhq/content-factory): open-source faceless video generator for Shorts, TikTok and Reels · [jutsustudio.com/content-factory](https://jutsustudio.com/content-factory/)
- [EdgeTAM Core ML](https://github.com/jutsuhq/edgetam-coreml): EdgeTAM video object tracking in Core ML, on the Apple Neural Engine
- [edgetam-onnx](https://github.com/jutsuhq/edgetam-onnx): the same tracker in Rust on ONNX Runtime CUDA, for Windows and Linux
- [Higgsfield Gateway](https://github.com/jutsuhq/higgsfield-gateway): a budget-capped Higgsfield API gateway on Cloudflare Workers · [jutsustudio.com/blog/higgsfield-api-production](https://jutsustudio.com/blog/higgsfield-api-production/)
- [Seedance 2.5 Prompt Skill](https://github.com/jutsuhq/higgsfield-seedance-prompt-skill): a Claude skill that writes Seedance 2.5 prompts for Higgsfield · [jutsustudio.com/blog/seedance-prompt-skill](https://jutsustudio.com/blog/seedance-prompt-skill/)
- [Open Source Higgsfield](https://github.com/jutsuhq/open-source-higgsfield): every #OpenSourceHiggsfield challenge project, by category · [jutsustudio.com/open-source-higgsfield](https://jutsustudio.com/open-source-higgsfield/)
- [awesome-higgsfield](https://github.com/jutsuhq/awesome-higgsfield): tools, SDKs, agent skills and apps for Higgsfield

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Use it anywhere, credit "Jutsu Studio, [jutsustudio.com](https://jutsustudio.com/video-api-pricing/)".

More Higgsfield tools, SDKs and agent skills: [awesome-higgsfield](https://github.com/jutsuhq/awesome-higgsfield).

Found a wrong or missing price? [Open an issue](https://github.com/jutsuhq/video-api-prices/issues).
