# SC Cargo Grid Planner

A deterministic cargo-hauling planner for Star Citizen multi-grid ships (Railen, etc.).
It assigns each mission's boxes to specific cargo grids, builds a stop-by-stop
load/unload plan, and verifies that every box is delivered to the correct station
with no grid ever over capacity.

**It runs entirely in your browser** — no server, no install, no account. Open the
page on a tablet, phone, or desktop, paste your missions, and get a color-coded plan.

## Why

Asking an AI chatbot to track 30+ cargo boxes by hand is unreliable — they miscount,
mis-deliver, and then confidently claim they checked. This tool does the bookkeeping
with actual code (JavaScript in your browser), so the arithmetic can't be fudged:
every plan is checked against hard rules and won't render if anything is wrong.

## Use it

Open the hosted page (GitHub Pages) or download `index.html` and open it in any browser.

1. Enter your ship's grids (`name = capacity`, one per line).
2. Enter your current location and the route order (one stop per line).
3. Paste your missions in the format shown.
4. Press **Build plan**.

### Mission format

```
# Any label for the mission
Pickup: LOCATION
Deliver 7 Pressurized Ice to L3
Deliver 5 Processed Food to L1
```

For a mission whose boxes are collected at several stations, put the source on each
line with `from`:

```
# Waste and Scrap
Deliver 1 Waste to Baijini from L5
Deliver 2 Waste to Baijini from L1
Deliver 1 Scrap to Baijini from L2
Deliver 1 Scrap to Baijini from L4
```

Mission codes (`L2-96`, `BP-20`, …) are generated automatically from pickup + total SCU.

## What it checks

Every plan must pass, or it shows an error instead:

- every cargo stack is loaded once and pulled off once;
- each stack is pulled from the same grid it was loaded onto;
- each stack is delivered to its correct destination (no mis-delivery);
- no grid ever exceeds its capacity at any point in the route;
- the stack count matches the missions entered.

Capacity is the hard limit; the planner keeps each big haul on its own grid where it
can and only combines the small drops onto a shared grid when there aren't enough
grids to go around.

## Tips

- Screenshots of contract screens can't be read here (no OCR). Type the missions, or
  have any AI transcribe screenshots into the format above and paste that in.
- Route ordering is your call — the planner guarantees the *cargo* is correct for the
  order you give; it doesn't yet optimize travel distance.
