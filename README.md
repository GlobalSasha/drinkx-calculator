# DrinkX ROI Calculator

Static GitHub Pages calculator for comparing a classic coffee-machine park with DrinkX.

## Main links

- Live Ares version: https://globalsasha.github.io/drinkx-calculator/index-ares.html
- Repository: https://github.com/GlobalSasha/drinkx-calculator
- Main working file: `index-ares.html`

## Project files

- `index-ares.html` — current production version with the Ares visual design.
- `index-nexus.html` — alternative Nexus visual version.
- `index.html` — earlier baseline version.
- `.nojekyll` — keeps GitHub Pages from running Jekyll processing.
- `docs/PROJECT_DOCUMENTATION.md` — technical and product documentation.
- `docs/STATUS_2026-05-21.md` — current session status and next-start context.

## Current production features

- ROI comparison between classic coffee machines and DrinkX.
- Separate editable input columns for classic machine and DrinkX.
- CAPEX discount logic by park size.
- Weighted drink mix by black coffee vs milk drinks.
- Monthly park profit, payback, yearly profit, NPV, IRR, profit per machine.
- Period selector: 36, 48, 60 months.
- Optional simplified tax mode: VAT 22% + profit tax 20%.
- CSV export for Excel.
- PDF via browser print.
- Shareable calculation links via URL hash.

## Local development

Open the project folder:

```bash
cd /Users/aleksandrhvastunov/drinkx-calculator
```

Run a local static server:

```bash
python3 -m http.server 5175
```

Then open:

```text
http://localhost:5175/index-ares.html
```

## Deploy

GitHub Pages serves the `main` branch root. To deploy:

```bash
git add .
git commit -m "Describe change"
git push origin main
```

GitHub Pages usually updates within 1-3 minutes.

