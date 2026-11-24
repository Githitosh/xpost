# xpost

Public **Humor → X** post artifacts for [Githitosh](https://github.com/Githitosh).

[githito.xapi](https://github.com/Githitosh/githito.xapi) **Humor → X** (3×/day JST) encodes a short Phone MP4 + three-line body, stages them here, then posts to X. Stock videos still come from [reply_movies](https://github.com/Githitosh/reply_movies); this repo holds **what was (or will be) tweeted**.

## Layout

```
queued/     staged for the next live post (or dry-run leftovers)
posted/     already tweeted
ledger.json posted clip ids (dedupe; also mirrored on xapi `api` branch)
```

Each clip folder:

| File | Role |
| --- | --- |
| `body.txt` | Exact tweet text (date · `ニュース速報` · status URL) |
| `clip.mp4` | 3.2s Phone encode (preferred) |
| `clip.pointer.json` | Optional instead of/beside MP4 — `{ "url": "…" }` or relative path |
| `meta.json` | Clip id, source URL, handle, timestamps, tweet id |

Folder name: `YYYYMMDD-HHmmssJST_<statusId>/` (Asia/Tokyo wall clock when staged).

## Lifecycle

1. **Humor → X** picks an unposted registered-account clip in the inter-post window.
2. Encodes MP4 → writes `queued/<slug>/`.
3. Live post → moves folder to `posted/`, sets `meta.tweetId`, updates `ledger.json`.
4. Dry-run (`dry_run` on) → leaves the folder in `queued/` (no tweet).

## Secrets (on githito.xapi)

| Secret | What |
| --- | --- |
| `XPOST_TOKEN` | Fine-grained PAT: Contents **read/write** on this repo |
| `X_API_*` | X OAuth (live tweet) — see xapi Humor → X workflow |
| `MEMO_TOKEN` | Recommended: registered-account filter |

Without `XPOST_TOKEN`, Humor → X still posts (when X secrets are set) and updates the xapi `api` ledger; it skips pushing here.

## Cadence

08:00 / 14:00 / 20:00 Asia/Tokyo — see Project `docs/humor-x-post-cadence.md` and [Humor → X](https://github.com/Githitosh/githito.xapi/actions/workflows/humor-x-post.yml).
