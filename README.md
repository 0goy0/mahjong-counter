# Mahjong Chip Counter

A simple, self-contained chip counter for mahjong — for when you're playing without physical chips. Track each player's running total, record wins and side-payments, and settle up at the end.

**Live:** https://0goy0.github.io/mahjong-counter/

It's a single `index.html` file (HTML + CSS + JavaScript, no dependencies, no backend). It runs entirely in the browser and works offline if you open the file directly.

## Features

- **Setup** — name four players (seats 東 / 南 / 西 / 北) and set starting chips.
- **Record a hand** — Hu (discard) or Zimo (self-draw), pick winner/shooter, tai, and an optional Bao (one player liable for all three zimo shares).
- **Instant payments** — during-play side-payments: Yao (targeted / everyone), Anyao/Angang, Gang, with a count multiplier.
- **Standings** — live per-player totals with colored +/- pills, a leader highlight, and a win/loss pulse animation.
- **Hide totals** — a spoiler blur so a player can avoid seeing if they're up or down; tap a card to peek.
- **History** — every hand and payment logged; delete any entry (totals recalculate) or undo the last.
- **Settle up** — ranked final results plus the minimal list of who-pays-whom.
- **Scoring presets** — switch between scoring systems or edit your own:
  - **1/2 payout** (default) — payment doubles per tai.
  - **3/6 payout** (san liu ban).
  - **Custom** — edit min/max tai, the per-tai Hu/Zimo table, and instant amounts.
- **Auto-save** — the in-progress game and your custom scoring are saved in the browser, so a refresh, lock, or restart won't lose anything. (Saved per device/browser only — not shared across phones.)

## Scoring reference

### 1/2 payout (default)

| Tai | Hu (shooter pays) | Zimo (each pays) |
|-----|-------------------|------------------|
| 1   | 4                 | 2                |
| 2   | 8                 | 4                |
| 3   | 16                | 8                |
| 4   | 32                | 16               |
| 5   | 64                | 32               |

### 3/6 payout (san liu ban)

| Tai | Hu (shooter pays) | Zimo (each pays) |
|-----|-------------------|------------------|
| 1   | 4                 | 2                |
| 2   | 7                 | 3                |
| 3   | 11                | 5                |
| 4   | 20                | 10               |
| 5   | 40                | 20               |

### Instant payments (both presets)

| Event                        | Who pays           | Chips |
|------------------------------|--------------------|-------|
| Yao (targeted)               | one player         | 2     |
| Yao / Gang (everyone)        | each opponent      | 2     |
| Anyao / Angang (everyone)    | each opponent      | 4     |
| Anyao (targeted)             | one player         | 4     |
| Gang (one player pays)       | one player         | 6     |

## Running locally

Just open `index.html` in a browser. Or serve it:

```sh
python3 -m http.server 8000
# then open http://localhost:8000
```

## Settlement logic

Every event adjusts a running per-player balance; the table always nets to zero.

- **Hu:** winner += amount; shooter -= amount.
- **Zimo:** winner += 3 × amount; each of the other three -= amount.
- **Zimo + Bao:** the liable player pays all three shares alone.
- **Instant payments:** either one chosen player pays, or each of the three opponents pays.
