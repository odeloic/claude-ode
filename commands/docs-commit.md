---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(gh pr list:*), Bash(gh pr create:*), Bash(git push:*)
description: Create a git commit with a summary
---

Review the current git status and staged changes using `git status` and `git diff --staged`.
If no changes are staged, check unstaged changes with `git diff`.
Analyze all modifications and create a concise, descriptive commit message following best practices: keep it short, specific, and meaningful.
Execute `git commit -m "<generated summary>"` with the summary, please conventional commit style `feat`, `fix`, `chore`, `docs`, ..

After committing, check if the current branch is a feature branch (not main/master/develop).
If it's a feature branch, verify if a PR already exists using `gh pr list --head <branch-name>`.
If no PR exists, create one using `gh pr create --fill` to automatically populate title and body from commits.