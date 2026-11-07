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
```

`git add` stages the selected content. `git commit` records a snapshot of staged changes in local history. It does not upload that commit to GitHub.

### 2. Create a feature branch

```bash
git switch main
git switch -c feature-example
```

After editing a file:

```bash
git add feature.txt
git commit -m "feat: update feature exercise"
git push -u origin feature-example
```

The first push establishes the branch's upstream tracking relationship. A pull request can then be opened on GitHub to review the branch before merging it into `main`.

### 3. Merge a branch locally

After committing the feature work:

```bash
git switch main
git merge feature-example
```

If a conflict occurs:

1. Run `git status` to identify the conflicted files.
2. Open each file and review the conflicting sections.
3. Edit the content to the intended result and remove conflict markers.
4. Stage the resolved files.
5. Complete the merge commit if Git requires one.

For example, when resolving `app.txt`:

```bash
git add app.txt
git commit
```

To cancel an in-progress merge before completing it:

```bash
git merge --abort
```

### 4. Synchronise with GitHub

```bash
git fetch origin
git pull --ff-only
git push
```

- `fetch` updates your local knowledge of remote branches without integrating their changes into your working branch.
- `pull --ff-only` updates the current tracking branch only when no merge commit is needed. It stops if local and remote history have diverged.
- `push` uploads local commits to the configured upstream branch.

If histories have diverged, inspect them before choosing an appropriate merge or rebase workflow.

### 5. Undo a committed change

```bash
git revert <commit-hash>
```

Replace `<commit-hash>` with the commit to undo. Revert creates a new commit that reverses the selected change while preserving the earlier history, making it useful for shared branches.

Other recovery commands explored during practice include:

| Command | Purpose |
| --- | --- |
| `git restore <file>` | Discard unstaged changes to a tracked file. |
| `git restore --staged <file>` | Unstage a file while keeping its working-directory changes. |
| `git commit --amend` | Replace the latest commit with an updated commit. |
| `git reflog` | Inspect local reference movements to help locate previous states. |

Restoring working files can discard edits. Amending an already published commit rewrites history, so it requires care when collaborating.

### 6. Ignore local files

`.gitignore` helps keep local configuration and generated files out of version control. For example, a project might exclude:

```gitignore
.env
*.log
```

These are illustrative patterns; review this repository's actual `.gitignore` for its rules. Ignore patterns do not automatically stop Git from tracking files that were already committed.

## Pre-commit checks

The repository includes `.pre-commit-config.yaml` to define checks that run before commits. The configuration used during this learning exercise includes general file checks and Terraform-related hooks.

Examples shown during setup include:

| Hook | Purpose |
| --- | --- |
| `check-added-large-files` | Detect unexpectedly large added files. |
| `detect-private-key` | Detect supported private-key patterns in checked files. |
| `terraform_fmt` | Apply Terraform's standard formatting. |
| `terraform_validate` | Validate Terraform configuration. |
| `terraform_tflint` | Run TFLint checks against Terraform code. |

The committed `.pre-commit-config.yaml` is the source of truth for enabled hooks and pinned versions. A hook may be skipped when no matching files are present. The current file listing does not show a Terraform `.tf` file.

### Install pre-commit on Ubuntu

If pipx is not installed:

```bash
sudo apt update
sudo apt install -y pipx
```

Then:

```bash
pipx ensurepath
pipx install pre-commit
```

Open a new terminal and verify:

```bash
pre-commit --version
```

### Enable and run the hooks

From the repository root:

```bash
pre-commit install
pre-commit validate-config
pre-commit run --all-files
```

Installing the hook is a local setup step: cloning a repository does not automatically install its Git hooks.

Terraform and TFLint must also be available in the environment running the relevant hooks. With WSL, installing a tool in Windows does not necessarily make its Linux executable available inside Ubuntu.

Check dependencies with:

```bash
terraform version
tflint --version
```

Use the official installation instructions linked below for any missing tools. Some Terraform configurations also require initialisation before validation.

If a formatting hook modifies files, review the changes, stage the updated files, and retry the commit:

```bash
git diff
git add <updated-file>
git commit -m "style: apply formatting fixes"
```

### Status of the setup

The presence of a configuration file does not establish that every hook passes. This lab includes the process of installing dependencies and troubleshooting hook failures; run the checks locally to verify the current state.

## Troubleshooting notes

| Message or situation | What to check |
| --- | --- |
| `Permission denied (publickey)` | Confirm the matching public key is added to GitHub and the private key is available to SSH in the current environment. |
| `src refspec main does not match any` | Check the local branch name and confirm the repository has an initial commit. |
| `No such file or directory` | Check spelling, the current working directory, and relative paths. |
| `Expected a Config map but got a NoneType` | Save the YAML file and check that the configuration is not empty and has a top-level `repos:` mapping. |
| Terraform or OpenTofu binary cannot be found | Install the required executable in the environment running pre-commit and check its PATH. |
| Changes do not appear on GitHub | Confirm the changes were staged, committed, and pushed to the intended remote and branch. |

For SSH troubleshooting:

```bash
ssh -T git@github.com
```

GitHub's successful authentication message also states that it does not provide shell access. That is expected for GitHub's Git-over-SSH service.

## Lessons learned

- Git and GitHub have different roles: Git manages version history; GitHub hosts repositories and supports collaboration.
- Saving a file, staging it, committing it, and pushing it are separate actions.
- A branch name must match the branch being pushed.
- Clear commit messages make the history easier to understand.
- Merge conflicts require reviewing the intended content, not simply removing markers.
- Windows and Ubuntu can have separate Git settings, SSH keys, and installed tools.
- Automated checks depend on both a valid configuration and the tools needed to execute it.
- Error messages provide useful evidence for choosing the next troubleshooting step.

## Next steps

- Add more detailed exercise notes and examples.
- Improve commit-message consistency.
- Verify all configured pre-commit hooks with appropriate sample files.
- Practise pull-request review and branch protection.
- Explore running checks in GitHub Actions.

These are future learning goals, not features already implemented by this repository.

## References

- [Git documentation](https://git-scm.com/doc)
- [GitHub documentation](https://docs.github.com/)
- [pre-commit documentation](https://pre-commit.com/)
- [pre-commit hooks](https://github.com/pre-commit/pre-commit-hooks)
- [Terraform pre-commit hooks](https://github.com/antonbabenko/pre-commit-terraform)
- [Terraform installation](https://developer.hashicorp.com/terraform/install)
- [TFLint](https://github.com/terraform-linters/tflint)

## Author

**Christian Osadume** — documenting my learning journey towards DevOps engineering.

[GitHub profile](https://github.com/christiandev132233)
