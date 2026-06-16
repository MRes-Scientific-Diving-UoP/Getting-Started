<div align="center">

# 🤿 Getting Started

### Your guide to using GitHub for the MRes Scientific Diving dissertation

[![MRes Scientific Diving](https://img.shields.io/badge/MRes-Scientific%20Diving-4ecdc4?style=for-the-badge&labelColor=0c2340)](https://github.com/MRes-Scientific-Diving-UoP)
[![SCIDIV5000](https://img.shields.io/badge/Module-SCIDIV5000-1e6091?style=flat-square)]()
[![120 Credits](https://img.shields.io/badge/Credits-120-2196a4?style=flat-square)]()

---

*This repository will help you set up and manage your dissertation project on GitHub — even if you've never used Git before.*

</div>

## 🚦 Start Here — Step-by-Step Guides

New to Git? Work through these three short guides **in order**. They take you from zero to a backed-up, well-organised project.

<div align="center">

| | Guide | What you'll do |
|--|-------|----------------|
| 🔑 | **[1. Connecting to GitHub with SSH](guides/01-ssh-setup.md)** | One-time key setup so pushing never asks for a password |
| 🚀 | **[2. Create Your First Repository](guides/02-create-your-first-repo.md)** | Turn a local folder into a GitHub repo — `init`, `add`, `commit`, `push` |
| 🧹 | **[3. Tidy R Project Structure](guides/03-tidy-r-project-structure.md)** | Lay out a reproducible, tidy-R project your supervisor can rerun |

</div>

The rest of this README is the **reference handbook** — read it once, then come back to it when you need detail.

---

## Table of Contents

- [Why GitHub?](#why-github)
- [1. Setting Up](#1-setting-up)
- [2. Your Project Repository](#2-your-project-repository)
- [3. Daily Workflow](#3-daily-workflow)
- [4. Writing Your Dissertation](#4-writing-your-dissertation)
- [5. Data Management](#5-data-management)
- [6. Getting Help](#6-getting-help)
- [Templates](#templates)
- [Key Dates](#key-dates)

---

## Why GitHub?

Using GitHub for your dissertation gives you several advantages over just keeping files on your laptop:

- **Version control** — every change is saved, and you can always go back to a previous version. No more `dissertation_v3_FINAL_v2_ACTUALLY_FINAL.docx`.
- **Backup** — your work is automatically backed up to the cloud every time you push.
- **Collaboration** — supervisors can review your work, leave comments, and suggest changes directly.
- **Reproducibility** — anyone can see exactly how you processed your data and reached your conclusions.
- **Portfolio** — your GitHub profile becomes a portfolio of your scientific work for future employers.

---

## 1. Setting Up

### Install Git

| Platform | How to install |
|----------|----------------|
| **Windows** | Download from [git-scm.com](https://git-scm.com/download/win) or install [GitHub Desktop](https://desktop.github.com/) |
| **Mac** | Run `xcode-select --install` in Terminal, or install [GitHub Desktop](https://desktop.github.com/) |
| **Linux** | Run `sudo apt install git` |

### Configure Git (one time only)

Open a terminal and run:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@students.plymouth.ac.uk"
```

### Connect to GitHub with SSH

So that pushing your work never asks for a password, set up an **SSH key** once at the start of the year. Full walkthrough: **[Guide 1: Connecting to GitHub with SSH](guides/01-ssh-setup.md)**.

### GitHub Desktop (recommended for beginners)

If you're new to Git, **GitHub Desktop** is the easiest way to get started — it gives you a visual interface instead of the command line, and handles login for you (no SSH setup needed). Download it from [desktop.github.com](https://desktop.github.com/).

---

## 2. Your Project Repository

Each student gets their own repository within the organisation. To create it for the first time — connecting a folder on your laptop to GitHub with `git init`, `add`, `commit`, and `push` — follow **[Guide 2: Create Your First Repository](guides/02-create-your-first-repo.md)**.

Your repo should follow the structure below. For the full reproducible, tidy-R version (RStudio Projects, the `here` package, tidy data, `renv`), see **[Guide 3: Tidy R Project Structure](guides/03-tidy-r-project-structure.md)**.

```
your-project-name/
│
├── README.md                  ← Project overview (use the template below)
├── .gitignore                 ← Files Git should ignore
│
├── data/
│   ├── raw/                   ← Original, unmodified data (NEVER edit these)
│   ├── processed/             ← Cleaned/transformed data
│   └── README.md              ← Data dictionary — what each file contains
│
├── scripts/
│   ├── 01-data-cleaning.R     ← Number scripts in order of execution
│   ├── 02-analysis.R
│   └── 03-figures.R
│
├── figures/
│   ├── fig01-study-site.png
│   └── fig02-results.png
│
├── docs/
│   ├── proposal.md            ← Your project proposal
│   ├── field-notes/           ← Scanned field notebooks, dive logs
│   └── references.bib         ← Bibliography file (if using LaTeX/R Markdown)
│
└── dissertation/
    ├── dissertation.Rmd       ← or .tex, .docx — your main document
    └── sections/              ← Optional: split into chapters
```

### Important rules

1. **Never modify raw data.** Always read from `data/raw/`, process in a script, and save to `data/processed/`.
2. **Number your scripts** so anyone can run them in order (`01-`, `02-`, `03-`...).
3. **Write a README** for your project. Use the template in [`templates/PROJECT-README.md`](templates/PROJECT-README.md).

---

## 3. Daily Workflow

This is the cycle you repeat *after* your repo is set up. (Setting it up the first time — `git init` through your first `push` — is covered in [Guide 2](guides/02-create-your-first-repo.md).) Whether you use the command line or GitHub Desktop, the cycle is always the same:

```
  ┌──────────┐     ┌──────────┐     ┌──────────┐     ┌──────────┐
  │  1. Pull  │ ──▶ │ 2. Work  │ ──▶ │ 3. Stage │ ──▶ │ 4. Push  │
  │  (fetch   │     │ (edit    │     │ & Commit │     │ (upload  │
  │  latest)  │     │  files)  │     │ (save    │     │  to      │
  │           │     │          │     │  point)  │     │  GitHub) │
  └──────────┘     └──────────┘     └──────────┘     └──────────┘
```

### Command line version

```bash
# 1. Pull the latest changes
git pull

# 2. Do your work — edit files, run scripts, etc.

# 3. Stage and commit your changes
git add .
git commit -m "Add species abundance analysis for site A"

# 4. Push to GitHub
git push
```

### Writing good commit messages

Your commit messages should describe **what** you did and **why**. Think of them as a lab notebook for your code.

| Good commit messages | Bad commit messages |
|---------------------|---------------------|
| `Add quadrat data from dive 14-Mar` | `update` |
| `Fix species ID error in reef fish counts` | `stuff` |
| `Run GLM for depth vs. abundance` | `changes` |
| `Add Figure 3 — PCA biplot` | `asdfgh` |

---

## 4. Writing Your Dissertation

### Format options

| Format | Best for | Tools needed |
|--------|----------|-------------|
| **R Markdown** (.Rmd) | R users, stats-heavy projects | RStudio |
| **Quarto** (.qmd) | R or Python users, modern option | RStudio / VS Code |
| **LaTeX** (.tex) | Full control over formatting | Overleaf / TeXShop |
| **Word** (.docx) | Simplicity | Microsoft Word |

**R Markdown** or **Quarto** are recommended because they let you embed your analysis code directly in your dissertation — when your data changes, your figures and tables update automatically.

### Dissertation structure (SCIDIV5000)

Your final submission is a **5,000-word scientific paper** (±10%) following this structure:

1. **Title page** — title, name, student ID, date, supervisor, partner org
2. **Abstract** — 250 words max
3. **Introduction** — background, rationale, aims & hypotheses
4. **Methods** — study site, data collection, analysis approach
5. **Results** — figures, tables, statistical outputs
6. **Discussion** — interpretation, comparison with literature, limitations
7. **References** — Harvard style
8. **Appendices** — supplementary data, extra figures

---

## 5. Data Management

### What to commit to GitHub

| Commit | Don't commit |
|--------|-------------|
| Scripts (.R, .py, .m) | Very large data files (>50 MB) |
| Processed data (<50 MB) | Sensitive/personal data |
| Figures and plots | API keys or passwords |
| Documentation and notes | Temporary/cache files |
| Your `.gitignore` file | `.DS_Store`, `Thumbs.db` |

### Use a `.gitignore` file

Create a `.gitignore` in your repo root to keep unwanted files out. Here's a starter:

```gitignore
# OS files
.DS_Store
Thumbs.db

# R
.Rhistory
.RData
.Rproj.user/

# Python
__pycache__/
*.pyc
.ipynb_checkpoints/

# Large data (track with Git LFS or store externally)
*.tif
*.mp4
*.mov

# Temporary
*.tmp
*~
```

### Large files?

If your project involves large datasets (video, photogrammetry, GIS rasters), don't commit them directly. Instead:

- Use **Git LFS** (`git lfs track "*.tif"`) for files up to ~2 GB
- Store very large files on **OneDrive/SharePoint** and link to them in your README
- Ask your supervisor about the university's research data storage

---

## 6. Getting Help

| Question | Where to go |
|----------|-------------|
| Git/GitHub basics | [GitHub Docs](https://docs.github.com/en/get-started) |
| R / RStudio | [R for Data Science](https://r4ds.hadley.nz/) |
| Python | [Python Data Science Handbook](https://jakevdp.github.io/PythonDataScienceHandbook/) |
| Statistics help | Dr Richard Preziosi (SCIDIV5002) |
| Project issues | Your dissertation supervisor |
| This GitHub org | Dr Keiron Fraser |

---

## Templates

This repo includes templates to help you get started:

- [`templates/PROJECT-README.md`](templates/PROJECT-README.md) — Template README for your project repo
- [`templates/.gitignore`](templates/.gitignore) — Starter .gitignore file

### Step-by-step guides

- [`guides/01-ssh-setup.md`](guides/01-ssh-setup.md) — Connect to GitHub with SSH
- [`guides/02-create-your-first-repo.md`](guides/02-create-your-first-repo.md) — Create your first repository (init / add / commit / push)
- [`guides/03-tidy-r-project-structure.md`](guides/03-tidy-r-project-structure.md) — Structure a project the tidy R way

---

## Key Dates

| Date | Milestone |
|------|-----------|
| **24 April** | Project proposal (formative) |
| **25 November** | Project presentation (15%) |
| **1 December** | Dissertation draft (formative feedback) |
| **8 January** | Final dissertation submission (85%) |

---

<div align="center">

**MRes Scientific Diving · University of Plymouth**

*Coxside Marine Station · School of Biological & Marine Sciences*

[![Back to Organisation](https://img.shields.io/badge/←%20Back%20to-Organisation-0c2340?style=flat-square)](https://github.com/MRes-Scientific-Diving-UoP)

</div>
