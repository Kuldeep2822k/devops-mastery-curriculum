---
title: Prerequisites Primer (Terminal, Files, and Git)
tags:
  - setup
  - primer
  - beginners
---

# Prerequisites Primer — Terminal, Files, and Git

This is the **missing first step** for someone who has never used a command line or Git. The rest of the curriculum assumes you can do the things on this page. If you already can, skim it and move on. If not, spend an hour here — it will save you many hours later.

> **You only need two tools for your first module:** `python3` and `curl`. You do **not** need Docker, Kubernetes, Terraform, or Ansible yet. Install those later, when a module asks for them. (See [00-overview.md](00-overview.md).)

## 1. The terminal (a.k.a. command line, shell)

The terminal is a text window where you type commands instead of clicking buttons. It feels intimidating for about a day, then becomes faster than clicking.

- **macOS:** open the **Terminal** app (Applications → Utilities → Terminal).
- **Linux:** open your **Terminal** application.
- **Windows:** install **WSL2** and use the **Ubuntu** terminal — see [01-workstation-baseline.md](01-workstation-baseline.md#windows-users-use-wsl2). (Plain PowerShell/CMD will *not* match the commands in this guide.)

### The handful of commands you actually need to start

```bash
pwd                 # print working directory: "where am I?"
ls                  # list the files/folders here
ls -la              # list everything, including hidden files, with details
cd ~/work           # change directory into ~/work  ( ~ means your home folder )
cd ..               # go up one folder
mkdir -p ~/work/devops-labs   # make a folder (and parents) if it doesn't exist
cat file.txt        # print a file's contents
```

> **Callout — what is `~`?** The `~` character is shorthand for your **home directory** (e.g. `/home/you` on Linux, `/Users/you` on macOS). So `~/work` means "the `work` folder inside your home folder."

### Trying it: your first 60 seconds

```bash
mkdir -p ~/work/devops-labs
cd ~/work/devops-labs
pwd
```

You should see a path ending in `/work/devops-labs`. That's it — you're navigating the filesystem from the terminal.

## 2. Editing a file

You'll create and edit small text files constantly. Two easy options:

- **A graphical editor** like **VS Code** (`code filename.txt`) — easiest to start.
- **A terminal editor** like **nano** (`nano filename.txt`; save with `Ctrl+O`, exit with `Ctrl+X`).

Some labs create files *for* you using a `cat > file <<'EOF' ... EOF` block. You don't have to understand that syntax yet — just paste the whole block into the terminal and press Enter. It writes the file exactly as shown.

### Trying it

```bash
cd ~/work/devops-labs
echo "hello devops" > hello.txt
cat hello.txt
```

If you see `hello devops`, you just created and read a file from the command line.

## 3. Checking Python and curl

Module 01's lab needs these two. Verify they exist:

```bash
python3 --version
curl --version
```

**Expected signal:** each prints a version number. If you get `command not found`, install them:

- **macOS:** `brew install python3` (curl is preinstalled).
- **Ubuntu/WSL2:** `sudo apt update && sudo apt install -y python3 curl`.

## 4. What is Git (and GitHub)?

**Version control** is a system that records the history of your files — every change, who made it, and why — so you can review it, undo it, and collaborate without emailing `final_v2_FINAL.zip` around. **Git** is the most popular version-control tool. **GitHub** is a website that hosts Git projects (like this curriculum) so people can share them.

You need three ideas:

- A **repository** ("repo") is a project folder that Git is tracking.
- A **commit** is a saved snapshot of your changes, with a message explaining them.
- **Clone / push / pull** move commits between your computer and GitHub.

### Trying it: your first commit

```bash
# one-time identity setup (use your own name/email)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"

mkdir -p ~/work/git-practice && cd ~/work/git-practice
git init                     # start tracking this folder
echo "my notes" > notes.txt
git add notes.txt            # stage the file for the next snapshot
git commit -m "Add my first notes"   # save the snapshot with a message
git log --oneline            # see your commit in the history
```

**Expected signal:** `git log --oneline` shows one commit with your message. Congratulations — that's the core Git loop (`edit → add → commit`) you'll repeat forever.

### Cloning this curriculum

```bash
git clone https://github.com/Kuldeep2822k/devops-mastery-curriculum.git devops-staff-guide
cd devops-staff-guide
```

## 5. A safe place to work

Keep your practice work **separate** from this guide, so you never damage the guide and can wipe experiments freely:

```bash
mkdir -p ~/work/devops-labs ~/work/devops-evidence
```

- `devops-labs/` — things you build in labs.
- `devops-evidence/` — your reports, runbooks, and notes (see [../00-HOW-TO-USE/04-evidence-rubrics.md](../00-HOW-TO-USE/04-evidence-rubrics.md)).

## You're ready when…

- [ ] You can open a terminal and run `pwd`, `ls`, `cd`, `mkdir`.
- [ ] You can create and read a text file.
- [ ] `python3 --version` and `curl --version` both print versions.
- [ ] You made at least one Git commit and saw it in `git log`.

When all four are true, continue to [00-overview.md](00-overview.md) and then [Module 01](../MODULES/01-foundations/00-overview.md). If any term here was fuzzy, the [Glossary](../APPENDICES/glossary.md) is your friend.
