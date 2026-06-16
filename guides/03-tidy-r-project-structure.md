<div align="center">

# 🧹 Structuring a Project the Tidy R Way

### A reproducible, self-contained layout that your supervisor (and future you) will thank you for

[![MRes Scientific Diving](https://img.shields.io/badge/MRes-Scientific%20Diving-4ecdc4?style=for-the-badge&labelColor=0c2340)](https://github.com/MRes-Scientific-Diving-UoP)
[![Guide 3 of 3](https://img.shields.io/badge/Guide-3%20of%203-1e6091?style=flat-square)]()

</div>

---

## The one rule that matters most

> **Your project is a single self-contained folder. Everything it needs is inside it, and every path is relative to it.**

If you can zip the folder, send it to your supervisor, and they can run your scripts top to bottom without changing a single line — your project is reproducible. That is the goal. The rest of this guide is how to get there.

This follows the [tidyverse style guide](https://style.tidyverse.org/) and the *project-oriented workflow* taught in [R for Data Science](https://r4ds.hadley.nz/).

---

## 1. Use an RStudio Project (`.Rproj`)

Always work inside an **RStudio Project**, not a loose collection of scripts.

- **Create one:** *File → New Project → New Directory → New Project*, or *Existing Directory* to wrap your current folder.
- It creates a `yourproject.Rproj` file. **Double-click that file to open your project** — it sets the working directory to the project root automatically.

### ⛔ Never use `setwd()`

```r
setwd("C:/Users/keiron/Desktop/my stuff/diving project")   # ❌ breaks on every other computer
```

This is the single biggest reason a script "works on my machine" but nowhere else. Instead, use the [`here`](https://here.r-lib.org/) package, which always builds paths from the project root:

```r
library(here)

read_csv(here("data", "raw", "fish_counts.csv"))   # ✅ works on any machine
ggsave(here("output", "figures", "fig01-biomass.png"))
```

---

## 2. The recommended folder structure

```
title-dissertation-2026/
│
├── title-dissertation-2026.Rproj   ← open the project from here
├── README.md                         ← what this project is (use the template)
├── .gitignore                        ← what Git should ignore
├── renv.lock                         ← exact package versions (optional, see §6)
│
├── data/
│   ├── raw/          ← original data, TREATED AS READ-ONLY. Never edit by hand.
│   └── processed/    ← cleaned data your scripts produce (safe to delete & regenerate)
│
├── R/                ← reusable functions (sourced by your scripts)
│   └── functions.R
│
├── scripts/          ← the analysis pipeline, NUMBERED in run order
│   ├── 01-clean-data.R
│   ├── 02-analysis.R
│   └── 03-figures.R
│
├── output/
│   ├── figures/      ← generated plots
│   └── tables/       ← generated tables / model summaries
│
├── docs/             ← proposal, field notes, dive logs, references.bib
│
└── report/           ← your dissertation: report.qmd / .Rmd / .tex
```

> Small project? You can merge `R/` into `scripts/`. The non-negotiables are: **raw data is read-only**, **scripts are numbered**, and **outputs are separate from inputs**.

---

## 3. Raw data is sacred

```
data/raw/  →  [ 01-clean-data.R ]  →  data/processed/  →  [ 02-analysis.R ]  →  output/
```

- **Never edit files in `data/raw/`** — not in Excel, not by hand. Treat them as the immutable record of what you collected.
- Every cleaning or transformation step happens **in a script**, reading from `raw/` and writing to `processed/`.
- This means anyone can trace any number in your results back to the original observation. That is reproducibility.
- Add a short **`data/README.md`** (a *data dictionary*) describing each column: its name, units, and meaning.

---

## 4. Tidy data

Wickham's three rules for *tidy data* — get your data into this shape early and every downstream step gets easier:

1. **Each variable is a column.**
2. **Each observation is a row.**
3. **Each value is a cell** (one value per cell — no "12 (3 juv)" entries).

| ❌ Messy (wide, mixed) | ✅ Tidy (long) |
|---|---|
| `site`, `2024`, `2025`, `2026` columns of counts | `site`, `year`, `count` columns |

Reshape with `tidyr::pivot_longer()` / `pivot_wider()`. Other quick wins:

- One observation unit per table (don't mix transect-level and site-level data in one sheet).
- Consistent, machine-friendly column names: `lower_snake_case`, no spaces, no units in the name (put units in the data dictionary).
- Use real `NA` for missing values, not blanks, `-999`, or `"n/a"`.

---

## 5. Naming and code style (the tidyverse style guide)

### File names

- Lowercase, end in `.R` (or `.qmd`/`.Rmd`).
- Use `-` or `_` instead of spaces.
- **Number scripts** so the run order is obvious: `01-clean-data.R`, `02-analysis.R`.
- Pad with a leading zero (`01`, not `1`) so they sort correctly past nine scripts.

### Inside your code

```r
# snake_case for objects and functions
mean_biomass <- fish_data |>
  filter(depth_m < 15) |>
  group_by(site) |>
  summarise(biomass_g_m2 = mean(biomass, na.rm = TRUE))
```

| Convention | Rule |
|------------|------|
| **Assignment** | use `<-`, not `=` |
| **Names** | `snake_case` for variables and functions |
| **Pipe** | use the native `|>` (or `%>%`); start each step on a new line |
| **Spacing** | spaces after commas and around operators (`x + y`, not `x+y`) |
| **Line length** | aim for ≤ 80 characters |
| **Comments** | explain *why*, not *what*; start with `# ` |

> Let the machine do it: install the [`styler`](https://styler.r-lib.org/) package and run *Addins → Style active file* in RStudio to auto-format, and [`lintr`](https://lintr.r-lib.org/) to flag style issues.

### Script header template

Start each script with a short header and load packages at the top:

```r
# 02-analysis.R
# Fits GLMs of fish biomass vs. depth and protection status.
# Reads:  data/processed/fish_clean.csv
# Writes: output/tables/glm-summary.csv
# Author: Your Name | Last edited: 2026-03-14

library(tidyverse)
library(here)
```

---

## 6. Make it reproducible

- **Don't save your workspace.** In RStudio: *Tools → Global Options → uncheck "Restore .RData"* and set *"Save workspace to .RData on exit" → Never*. A clean R session every time catches hidden bugs.
- **Restart R often** and re-run from script `01`. If it only works after running things out of order, it isn't reproducible yet.
- **Pin your packages** with [`renv`](https://rstudio.github.io/renv/): run `renv::init()` once, then `renv::snapshot()` whenever you add a package. This writes `renv.lock` so anyone can reproduce your exact package versions.
- **Record your environment:** end an analysis with `sessionInfo()` so the R and package versions are captured in your output.

---

## Handling large files

Git and GitHub are built for text (code, small CSVs), **not** for big binaries. GitHub rejects any single file over **100 MB**.

| File type | What to do |
|-----------|------------|
| Small data (< ~50 MB CSV) | Commit it normally |
| Large data, video, photogrammetry, GIS rasters (`.tif`, `.mp4`, `.mov`) | **Don't commit.** Add the pattern to `.gitignore`, store the file on **OneDrive/SharePoint**, and link to it in your README |
| Files you must version (up to ~2 GB) | Use [**Git LFS**](https://git-lfs.com/): `git lfs track "*.tif"` |

This keeps your repo small and fast to clone. The provided [`../templates/.gitignore`](../templates/.gitignore) already ignores the common large-file types.

---

## Quick checklist

- [ ] Project opens from a `.Rproj` file
- [ ] No `setwd()` anywhere — paths use `here()`
- [ ] `data/raw/` is read-only; cleaning happens in scripts
- [ ] Data is tidy (one variable per column, one observation per row)
- [ ] Scripts are numbered and run top-to-bottom on a fresh R session
- [ ] A `README.md` and a `data/README.md` data dictionary exist
- [ ] Large/sensitive files are `.gitignore`d, not committed

---

<div align="center">

**You're set up.** Start from [`../templates/PROJECT-README.md`](../templates/PROJECT-README.md) and make it yours.

[![Back to Getting Started](https://img.shields.io/badge/←%20Back%20to-Getting%20Started-0c2340?style=flat-square)](../README.md)

</div>
