<div align="center">

# 🚀 Create Your First Repository

### Turn a folder on your laptop into a backed-up, version-controlled project on GitHub

[![MRes Scientific Diving](https://img.shields.io/badge/MRes-Scientific%20Diving-4ecdc4?style=for-the-badge&labelColor=0c2340)](https://github.com/MRes-Scientific-Diving-UoP)
[![Guide 2 of 3](https://img.shields.io/badge/Guide-2%20of%203-1e6091?style=flat-square)]()

</div>

---

## Before you start

You need:

- ✅ **Git installed** — check with `git --version`
- ✅ **Git configured** with your name and email (one time only):

  ```bash
  git config --global user.name "Your Name"
  git config --global user.email "your.email@students.plymouth.ac.uk"
  ```

- ✅ **SSH set up** — see [Guide 1: Connecting to GitHub with SSH](01-ssh-setup.md)

> **Naming your repo:** use lowercase letters, numbers and hyphens — no spaces or capitals.
> Good: `cabo-pulmo-fish-biomass`, `reef-eDNA-2026`. Bad: `My Project (final)`.

---

## The big picture

A *repository* ("repo") is just a folder that Git is watching. There are two halves:

```
   YOUR LAPTOP                              GITHUB (the cloud)
   ┌─────────────────┐                      ┌─────────────────┐
   │  local repo     │  ──── git push ───▶  │  remote repo    │
   │  (you work here)│  ◀─── git pull ────  │  (backup +      │
   │                 │                      │   sharing)      │
   └─────────────────┘                      └─────────────────┘
```

You do all your work locally, then `push` to send your saved changes up to GitHub. This guide connects the two for the first time.

---

## Step 1 — Create the empty repo on GitHub

> **Note:** student repos live inside the **MRes-Scientific-Diving-UoP** organisation. If you can't create one there, ask Dr Fraser or Dr Favoretto to create it for you (or to add you to the organisation), then continue from Step 2.

1. Go to **[github.com/organizations/MRes-Scientific-Diving-UoP/repositories/new](https://github.com/organizations/MRes-Scientific-Diving-UoP/repositories/new)**
2. **Repository name:** e.g. `surname-dissertation-2026`
3. **Description:** a one-line summary of your project
4. **Visibility:** **Private** (you can make it public later if you publish)
5. **Important:** leave **"Add a README", "Add .gitignore" and "Add a license" UNTICKED.** You'll push your own. *(Ticking these creates files on GitHub that aren't on your laptop, which makes the first push awkward.)*
6. Click **Create repository**.

GitHub now shows you a page with setup commands — keep it open, you'll use the SSH address from it (`git@github.com:MRes-Scientific-Diving-UoP/...`).

---

## Step 2 — Get your project folder ready locally

Put your project files in one folder. A good starting layout (see [Guide 3](03-tidy-r-project-structure.md) for the full version):

```
surname-dissertation-2026/
├── README.md          ← copy from templates/PROJECT-README.md
├── .gitignore         ← copy from templates/.gitignore
├── data/
│   └── raw/
├── R/
└── output/
```

> **Always add a `.gitignore` first.** It stops Git from tracking junk (`.DS_Store`, `.Rhistory`) and — importantly — huge or sensitive files. Copy the starter from [`../templates/.gitignore`](../templates/.gitignore).

---

## Step 3 — Connect the folder to Git and push

Open a terminal **inside your project folder** (in RStudio: *Tools → Terminal*; on Mac: drag the folder onto the Terminal icon; on Windows: right-click the folder → *Git Bash Here*).

Then run these commands **once**, in order:

```bash
# 1. Turn this folder into a Git repository
git init

# 2. Set the default branch name to "main"
git branch -M main

# 3. Stage everything (the "." means "all files in this folder")
git add .

# 4. Save your first snapshot ("commit") with a message
git commit -m "Initial project structure"

# 5. Tell Git where the GitHub repo lives (use YOUR repo's SSH address)
git remote add origin git@github.com:MRes-Scientific-Diving-UoP/surname-dissertation-2026.git

# 6. Push your work up to GitHub
git push -u origin main
```

That's it. Refresh the GitHub page — your files are now online. 🎉

The `-u origin main` in step 6 only needs the `-u` part the **first** time; after that, plain `git push` is enough.

---

## Step 4 — The everyday cycle (do this from now on)

After the one-time setup, your daily rhythm is just four moves:

```bash
git pull                              # 1. get the latest (esp. if working on >1 machine)
# ... do your work: edit files, run scripts ...
git add .                             # 2. stage your changes
git commit -m "Add site-A quadrat data and cleaning script"   # 3. save a snapshot
git push                              # 4. back it up to GitHub
```

**Commit little and often** — a good rule is to commit each time you finish a meaningful chunk (a cleaned dataset, a working figure, a paragraph). Each commit is a point you can rewind to.

### Writing good commit messages

A commit message is a one-line note to your future self. Describe **what** changed.

| ✅ Good | ❌ Bad |
|--------|-------|
| `Add quadrat data from dive 14-Mar` | `update` |
| `Fix species ID error in reef fish counts` | `stuff` |
| `Run GLM for depth vs. abundance` | `asdf` |
| `Add Figure 3 — PCA biplot` | `changes` |

---

## Common first-push problems

| Message | What it means | Fix |
|---------|---------------|-----|
| `Permission denied (publickey)` | SSH isn't set up | Do [Guide 1](01-ssh-setup.md) |
| `remote origin already exists` | You ran step 5 twice | `git remote set-url origin git@github.com:...` to correct the address |
| `Updates were rejected... fetch first` | GitHub has commits your laptop doesn't (usually because you ticked "Add a README" in Step 1) | `git pull --rebase origin main` then `git push` |
| `src refspec main does not match any` | You haven't committed yet | Run step 4 (`git commit`) before pushing |
| `failed to push some refs` after adding a huge file | A file is over GitHub's 100 MB limit | Add it to `.gitignore`, see [Guide 3 → large files](03-tidy-r-project-structure.md#handling-large-files) |

---

## Prefer not to use the command line?

[**GitHub Desktop**](https://desktop.github.com/) does all of the above with buttons:

1. **File → Add local repository** → choose your folder → *"create a repository"* if prompted.
2. **Publish repository** → choose the **MRes-Scientific-Diving-UoP** organisation → keep **Private** ticked → *Publish*.
3. From then on: write a summary, click **Commit to main**, then **Push origin**.

GitHub Desktop uses HTTPS by default and logs you in through the app, so you can use it *without* the SSH setup — but learning the command-line workflow above will serve you better long-term.

---

<div align="center">

**Next:** [→ Structure your project the tidy R way](03-tidy-r-project-structure.md)

[![Back to Getting Started](https://img.shields.io/badge/←%20Back%20to-Getting%20Started-0c2340?style=flat-square)](../README.md)

</div>
