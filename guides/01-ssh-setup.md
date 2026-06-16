<div align="center">

# 🔑 Connecting to GitHub with SSH

### A one-time setup so you never have to type a password when you push

[![MRes Scientific Diving](https://img.shields.io/badge/MRes-Scientific%20Diving-4ecdc4?style=for-the-badge&labelColor=0c2340)](https://github.com/MRes-Scientific-Diving-UoP)
[![Guide 1 of 3](https://img.shields.io/badge/Guide-1%20of%203-1e6091?style=flat-square)]()

</div>

---

## Why SSH?

When you `push` your work to GitHub, GitHub needs to know it's really you. There are two ways to prove it:

| Method | What happens |
|--------|--------------|
| **HTTPS** | You log in with a username + a personal access token (a long password you have to generate and re-enter periodically). |
| **SSH** *(this guide)* | You set up a pair of keys **once**. After that, pushing "just works" with no password prompts. |

SSH is the recommended approach for the programme — set it up once at the start of the year and forget about it.

> **How SSH keys work, in one sentence:** you create two matching files — a **private key** that stays secret on your laptop, and a **public key** that you give to GitHub. GitHub uses the public key to recognise pushes signed by your private key.

---

## Step 1 — Open a terminal

| Platform | Terminal to use |
|----------|-----------------|
| **Mac** | **Terminal** (Applications → Utilities → Terminal) |
| **Windows** | **Git Bash** (installed with [Git for Windows](https://git-scm.com/download/win)) — *not* PowerShell or CMD |
| **Linux** | Your usual terminal |

---

## Step 2 — Check whether you already have a key

```bash
ls -al ~/.ssh
```

If you see a file called `id_ed25519.pub` (or `id_rsa.pub`), you already have a key — **skip to [Step 4](#step-4--add-your-key-to-the-ssh-agent)**. Otherwise carry on.

---

## Step 3 — Generate a new SSH key

Replace the email with your **university email** (the one you use for GitHub):

```bash
ssh-keygen -t ed25519 -C "your.email@students.plymouth.ac.uk"
```

When prompted:

1. **"Enter file in which to save the key"** → press **Enter** to accept the default location.
2. **"Enter passphrase"** → optional. A passphrase adds a second layer of protection if your laptop is stolen. Press Enter twice to skip it, or type one (you'll need it occasionally).

This creates two files in `~/.ssh/`:

- `id_ed25519` → your **private** key. **Never share this or commit it to a repo.**
- `id_ed25519.pub` → your **public** key. This is the one you give to GitHub.

---

## Step 4 — Add your key to the ssh-agent

The ssh-agent is a small background program that holds your key so you don't re-enter the passphrase constantly.

**Mac / Linux:**

```bash
# Start the agent
eval "$(ssh-agent -s)"

# Add your key
ssh-add ~/.ssh/id_ed25519
```

On **Mac**, also store it in your keychain so it survives restarts:

```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

**Windows (Git Bash):**

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

---

## Step 5 — Copy your public key

You need the **contents** of the `.pub` file.

**Mac:**

```bash
pbcopy < ~/.ssh/id_ed25519.pub
# ↑ this copies it straight to your clipboard
```

**Windows (Git Bash):**

```bash
cat ~/.ssh/id_ed25519.pub | clip
```

**Linux:**

```bash
cat ~/.ssh/id_ed25519.pub
# then select and copy the output manually
```

The key looks like one long line starting with `ssh-ed25519` and ending with your email. Copy the **whole** line.

---

## Step 6 — Add the public key to GitHub

1. Go to **[github.com/settings/keys](https://github.com/settings/keys)** (or: your profile photo → **Settings** → **SSH and GPG keys**).
2. Click **New SSH key**.
3. **Title:** something memorable, e.g. `MRes laptop 2026`.
4. **Key type:** leave as *Authentication Key*.
5. **Key:** paste the line you copied.
6. Click **Add SSH key**.

---

## Step 7 — Test it

```bash
ssh -T git@github.com
```

The first time, you'll be asked to confirm GitHub's fingerprint — type `yes`. You should then see:

```
Hi your-username! You've successfully authenticated, but GitHub does not provide shell access.
```

That message means **success** — the "does not provide shell access" part is normal and expected.

---

## Step 8 — Use the SSH address for your repos

For SSH to be used, your repository's remote must use the **SSH address** (starts with `git@`), not the HTTPS address (starts with `https://`).

When you clone, choose the **SSH** tab and use the `git@` URL:

```bash
git clone git@github.com:MRes-Scientific-Diving-UoP/your-repo-name.git
```

If you already cloned with HTTPS, switch the remote over:

```bash
git remote set-url origin git@github.com:MRes-Scientific-Diving-UoP/your-repo-name.git

# Confirm it changed
git remote -v
```

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| `Permission denied (publickey)` | Your key isn't added to GitHub (Step 6) or not loaded in the agent (Step 4). Re-run `ssh-add ~/.ssh/id_ed25519`. |
| `Could not open a connection to your authentication agent` | Run `eval "$(ssh-agent -s)"` first, then `ssh-add` again. |
| GitHub still asks for a username/password on push | Your remote is still HTTPS. Switch it with `git remote set-url` (Step 8). |
| Multiple GitHub accounts | Ask Dr Favoretto — you'll need an `~/.ssh/config` entry. |

---

<div align="center">

**Next:** [→ Create your first repository](02-create-your-first-repo.md)

[![Back to Getting Started](https://img.shields.io/badge/←%20Back%20to-Getting%20Started-0c2340?style=flat-square)](../README.md)

</div>
