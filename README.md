# Git Lab
# Git & GitHub Learning Lab

A hands-on repository documenting my Git and GitHub practice with CoderCo as I work towards becoming a DevOps engineer.

This lab focuses on version control, branching, collaboration workflows, recovering from mistakes, and introducing automated checks with pre-commit. The text files are practice material used to explore Git behaviour; this repository is a learning project rather than a production application.

## Learning objectives

- Understand the working directory, staging area, and local repository.
- Create focused commits and inspect changes and history.
- Create branches and work on features independently.
- Merge changes and resolve merge conflicts.
- Connect a local repository to GitHub and authenticate using SSH.
- Push local commits and pull remote changes.
- Exclude suitable files using `.gitignore`.
- Undo changes with an understanding of the difference between revert and reset.
- Introduce automated checks through pre-commit hooks.

## Repository contents

The following files are visible in the current repository snapshot:

```text
git-labs/
├── .gitignore
├── .pre-commit-config.yaml
├── README.md
├── app.txt
├── feature.txt
├── hotfix.txt
├── meta.txt
├── note.txt
└── undo.txt
```

| File | Purpose in the lab |
| --- | --- |
| `.gitignore` | Defines patterns for files Git should leave untracked. |
| `.pre-commit-config.yaml` | Declares automated checks to run through pre-commit. |
| `README.md` | Explains the project, exercises, and workflow. |
| `app.txt` | Practice file used in a merge-conflict resolution exercise. |
| `feature.txt` | Practice file for feature-branch changes. |
| `hotfix.txt` | Practice file for a simulated hotfix workflow. |
| `meta.txt` and `note.txt` | Practice files for making and tracking changes. |
| `undo.txt` | Practice file associated with reverting an accidental commit. |

## Tools and environment

- Git for local version control.
- GitHub for remote repository hosting.
- Ubuntu through Windows Subsystem for Linux (WSL) for terminal exercises.
- Visual Studio Code for editing and Source Control integration.
- pre-commit for automated checks.
- Terraform and TFLint when running the configured Terraform hooks against Terraform files.

## Getting started

### Clone the repository

Using HTTPS:

```bash
git clone https://github.com/christiandev132233/git-labs.git
cd git-labs
```

Or, if SSH authentication is configured:

```bash
git clone git@github.com:christiandev132233/git-labs.git
cd git-labs
```

Cloning creates a directory named `git-labs` by default. An existing local copy may have a different name, such as `git-lab`; use the directory containing your copy.

### Inspect the repository

```bash
git status
git remote -v
git branch -a
git log --oneline --graph --decorate --all
```

These commands show the working-tree status, configured remotes, known branches, and commit history.

## Exercises and workflows

### 1. Stage and commit changes

```bash
git status
git diff
git add note.txt
git diff --staged
git commit -m "docs: update practice notes"
