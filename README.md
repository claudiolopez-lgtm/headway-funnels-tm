# Headway Funnels TM

Translation memory files for the **Headway Web Funnels Localization** Figma plugin.

The plugin fetches these JSON files at runtime (one per locale). Updates here go live in the plugin within seconds — no plugin re-release needed.

## Files

```
tm-es.json     — Spanish (Spain)
tm-ptbr.json   — Brazilian Portuguese  (coming soon)
tm-it.json     — Italian               (coming soon)
tm-de.json     — German                (coming soon)
tm-pl.json     — Polish                (coming soon)
tm-ja.json     — Japanese              (coming soon)
tm-fr.json     — French                (coming soon)

tools/build_tm.py — converts .numbers files → tm-<locale>.json
```

Each `tm-*.json` is a flat object mapping English source strings to their approved translations:

```json
{
  "Continue": "Continuar",
  "Select your gender": "¿Con qué género te identificas?",
  "Start free trial": "Empieza la prueba gratuita"
}
```

## Editing a translation

1. Open the relevant `tm-<locale>.json` file on GitHub
2. Click the pencil icon to edit in the browser
3. Find the line you want to change
4. Commit on the main branch — the plugin will pick up the change on next translation run (or after clearing local cache)

## Adding a new locale

1. Get a `.numbers` TM file for the locale
2. Run `python tools/build_tm.py path/to/your.numbers <locale>` (where `<locale>` is one of `ptbr`, `it`, `de`, `pl`, `ja`, `fr`)
3. Commit the resulting `tm-<locale>.json`
4. Enable the locale tab in the plugin's `ui.html`

## Updating from a fresh .numbers file

```bash
pip install numbers-parser
python tools/build_tm.py path/to/Web-Growth_ES_TM.numbers es
git add tm-es.json
git commit -m "Refresh ES TM from latest export"
git push
```

The script picks the **most common** Target translation when the same English source has multiple translations across rows. Run output shows you any inconsistencies it had to resolve.

## How the plugin uses these files

On locale switch, the plugin:
1. Tries `figma.clientStorage` first (instant if cached)
2. Falls back to `https://raw.githubusercontent.com/<your-username>/headway-funnels-tm/main/tm-<locale>.json`
3. Stores the result in `clientStorage` with a 1-hour TTL

When you commit a change here, users will see it after their local cache expires (≤1 hour) — or immediately if they hit the "Refresh TM" button in the plugin.
