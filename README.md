# ICC calculator

A single-page power calculator for clustered data: how statistical power depends on the number of animals, the number of cells (repeated measurements) per animal, the intraclass correlation (ICC) and the effect size d. Because cells within an animal are correlated, the ICC sets a ceiling on power that more cells cannot lift; the page shows this and the number of animals needed for 80% power.

## Features

- Two questions: two groups of animals, or one effect measured in every animal
- Sliders for ICC, effect size d, number of animals and cells per animal
- Generic preset chips: ICC 0.05 / 0.1 / 0.3 and d 0.2 / 0.5 / 0.8
- Tiles: power now, animals needed for 80%, power ceiling with endless cells
- Charts: power against animals (with ICC 0.05 / 0.1 / 0.3 reference curves) and power against cells per animal
- Table of animals needed for 80% at each preset ICC
- Light and dark theme, follows the system setting
- Everything is computed in the browser; no data is loaded or sent

## Requirements

- A modern web browser
- To serve the folder: [uv](https://docs.astral.sh/uv/) (it provides Python); any static web server works
- Internet is optional: the page requests Google Fonts and falls back to system fonts offline (checked)

## Setup

No install step. The app is one file, `app\index.html`.

## Run

From the project folder in Windows PowerShell:

```powershell
uv run python -m http.server 8766 --bind 127.0.0.1 --directory app
```

Then open http://127.0.0.1:8766/ . Stop the server with Ctrl+C. Pick another port if 8766 is in use.

## How to use

1. Choose the question (two groups, or one effect across animals).
2. Set the ICC with the slider or a chip, and the effect size d.
3. Set animals (per group in two-group mode) and cells per animal.
4. Read power, animals needed for 80% and the ceiling in the tiles; the charts and table update.

## How it works

Standard error of the difference (Aarts et al., 2014, Box 3):

- two groups of k animals: SE^2 = 2 [1 + (n - 1) ICC] / (k n)
- one effect in each of k animals: SE^2 = [1 + (n - 1) ICC] / (k n)

where n is cells per animal. Power is two-sided with alpha = 0.05, from the noncentral t distribution with 2k - 2 (two groups) or k - 1 (one effect) degrees of freedom, with non-centrality d / SE. The ceiling is the limit n -> infinity, where SE^2 -> 2 ICC / k (or ICC / k). Animals for 80% is found by binary search over k up to 500.

## Project layout

- `app/index.html` - the calculator (the whole app)
- `LICENSE` - GNU GPL v3

## Limitations

- Assumes a roughly normal outcome, equal cells per animal and one effect size
- For a within-animal effect, the ICC that matters is how much the effect itself varies between animals, which can differ from the ICC of the raw measure
- Does not model session-to-session variation inside an animal
- Power uses numerical integration; it is not validated against another package

## License

GNU General Public License v3.0, see `LICENSE`
