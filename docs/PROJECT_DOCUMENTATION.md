# DrinkX ROI Calculator — Project Documentation

Last updated: 2026-05-21

## Purpose

The calculator compares the economics of a classic coffee-machine park against DrinkX for retail networks and other multi-location operators.

The current production page is:

```text
https://globalsasha.github.io/drinkx-calculator/index-ares.html
```

The main implementation file is:

```text
/Users/aleksandrhvastunov/drinkx-calculator/index-ares.html
```

## Architecture

This is a static frontend project. There is no backend, database, build step, or package manager.

The app is implemented as a single HTML file with embedded CSS and JavaScript:

- HTML is generated through template strings into `#app`.
- Styling uses inline CSS plus Tailwind CDN utility classes.
- Charts use Chart.js from CDN.
- All calculations happen locally in the browser.
- Data is not sent to any external server.

## Production file

Use `index-ares.html` for current work.

Older/alternative files:

- `index.html` — baseline version.
- `index-nexus.html` — Nexus design variation.

Do not start new feature work from `index.html` unless intentionally rebuilding from the older version.

## State model

The main state object is in `index-ares.html` near the `STATE` section.

There are two calculation columns:

- `state.std` — classic coffee machine.
- `state.drx` — DrinkX.

Each side has these fields:

- `name`
- `basePrice`
- `parkSize`
- `coffeePrice`
- `coffeeBlackDose`
- `coffeeMilkDose`
- `milkPrice`
- `milkDose`
- `priceAmericano`
- `priceCappuccino`
- `milkShare`
- `cupsPerDay`
- `opex`
- `fraud`

## Core formulas

All core financial math lives in:

```js
function calc(d)
```

Current formulas:

```text
discount =
  10% if parkSize >= 50
  5% if parkSize >= 10
  0% otherwise

capexTotal = basePrice * parkSize * (1 - discount)

costAmericano = coffeePrice / 1000 * coffeeBlackDose

costCappuccino =
  coffeePrice / 1000 * coffeeMilkDose
  + milkPrice / 1000 * milkDose

avgCost =
  costAmericano * blackDrinkShare
  + costCappuccino * milkDrinkShare

avgPrice =
  priceAmericano * blackDrinkShare
  + priceCappuccino * milkDrinkShare

avgMargin = avgPrice - avgCost

monthlyCups = cupsPerDay * 30 * parkSize

fraudLoss = monthlyCups * avgPrice * fraudPercent

profitPark =
  monthlyCups * avgMargin
  - opex * parkSize
  - fraudLoss

paybackMonths = capexTotal / profitPark
```

Important interpretation:

`profitPark` is monthly operating profit before CAPEX payback, not net profit after CAPEX.

CAPEX is included in payback, NPV, IRR, and Cash Flow.

## Tax mode

The optional tax mode is controlled by:

```js
window.__taxesOn
```

Current simplified tax behavior:

```text
VAT is estimated as gross revenue * 22 / 122.
Then profit tax is approximated as 20% of remaining positive profit.
```

This is intentionally simplified and should not be treated as a full accounting model.

## Period selector

The period selector is controlled by:

```js
window.__periodMo
```

Supported values:

- `36`
- `48`
- `60`

The selected period affects:

- hero period label;
- hero accumulated impact;
- Cash Flow chart horizon;
- NPV;
- IRR;
- CSV export period rows.

## Shareable calculation links

Added on 2026-05-20.

Button:

```text
↗ ССЫЛКА
```

Implementation:

- Current state is serialized into JSON.
- JSON is UTF-8 encoded.
- Payload is converted to URL-safe base64.
- It is stored in `location.hash` as:

```text
#calc=<encoded_payload>
```

On page load:

```js
applySharedStateFromHash()
```

restores:

- `state.std`;
- `state.drx`;
- selected period;
- tax mode.

This approach requires no backend and works on GitHub Pages.

## CSV export

CSV export is implemented in:

```js
buildExportRows()
window.exportCSV()
```

The export includes:

- input parameters;
- calculated metrics;
- comparison rows;
- current period;
- tax mode state.

## Known formula notes

The current formulas are arithmetically consistent with the exported CSV.

The largest perceived DrinkX advantage usually comes from assumptions, not from arithmetic:

- DrinkX may have higher cups/day than the classic machine.
- Classic machine may have fraud while DrinkX has zero fraud.
- DrinkX has lower OPEX.

If a strict apples-to-apples comparison is needed later, add a mode that synchronizes:

- cups/day;
- park size;
- fraud assumption;
- prices.

## Known limitations

- Static single-file architecture makes the file large.
- Tailwind CDN shows a production warning in browser console.
- No automated test suite.
- No backend storage for calculations.
- Share links can become long because all parameters are stored in the URL hash.
- Tax calculations are simplified.
- Cups/lids/syrups/service consumables are not modeled separately.
- Month length is fixed at 30 days.

## Recommended next improvements

1. Add "fair comparison" toggle to sync cups/day and optionally fraud between columns.
2. Add breakdown of DrinkX advantage by factor:
   - traffic/cups;
   - gross margin;
   - OPEX;
   - fraud;
   - CAPEX.
3. Add a small "copied" toast for share links instead of changing button text only.
4. Add favicon to remove 404 console noise.
5. Split JS/CSS into separate files if the project grows.
6. Add a formula audit block visible in the UI.

