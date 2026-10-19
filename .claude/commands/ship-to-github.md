---
description: Security-scan, document, push and deploy this project to GitHub Pages with CI/CD
argument-hint: [owner/repo=manellFab/mnl] [branch=main]
allowed-tools: Bash, Read, Write, Edit, Glob, Grep
---

Ship this project to GitHub. The target is `$ARGUMENTS` (`owner/repo`, optional branch); with no arguments it defaults to `manellFab/mnl` (https://github.com/manellFab/mnl) on branch `main`.

Work through the steps in order. Stop and report if any step fails; never skip the security scan.

## 1. Security scan (do this FIRST, before anything is committed or pushed)
Goal: make sure nothing sensitive leaves the machine.
- List every file that would be committed (`git status --porcelain`, `git ls-files`, plus untracked files not ignored).
- Grep those files for: private keys (`BEGIN .* PRIVATE KEY`), tokens and keys (`ghp_`, `github_pat_`, `AKIA`, `sk-`, `xox[bap]-`, `api[_-]?key`, `secret`, `password`, `token`), `.env` files, credentials JSON, `*.pem`, `*.p12`, and real personal email addresses or phone numbers.
- Check that no local-only or private files are staged (scratch files, logs, node_modules, build output, OS files).
- Review any outbound network calls in the code (`fetch`, `XMLHttpRequest`, `<script src=http`, `<link href=http`, CDN URLs) and list the hosts each one contacts. Flag any that send user data somewhere unexpected.
- Check git history for the same patterns if the repo already has commits (`git log -p -S` on suspicious strings).
- Add a `.gitignore` covering `.env*`, `*.pem`, `*.key`, `node_modules/`, logs, and OS files if one is missing.
- Report findings as a short table (file, line, issue, action). If anything sensitive is found, STOP, tell the user, and do not push. Placeholders such as `YOUR_EMAIL@example.com` are fine; a real address is not.

## 2. README
Create or update `README.md` with:
- Title and one-line description
- Technology badges (shields.io image links, e.g. HTML5, CSS3, JavaScript, GitHub Actions, GitHub Pages), chosen from the real stack
- Live demo link (the GitHub Pages URL `https://<owner>.github.io/<repo>/`)
- Features
- **How to install** (clone the repo; list prerequisites, or state none)
- **How to run** (exact commands or "open index.html")
- Configuration notes (for example, any placeholder that must be replaced)
- Project structure, CI/CD summary, and licence/disclaimer if relevant
Keep it accurate to the code. Do not invent features.

## 3. GitHub "About" section
Set the repo description (one sentence), topics (tags matching the tech stack), and homepage (the Pages URL).
- Prefer `gh repo edit <owner/repo> --description "..." --homepage "<url>" --add-topic <t>`.
- If `gh` is not installed, use the REST API (`PATCH /repos/{owner}/{repo}` and `PUT /repos/{owner}/{repo}/topics`) with a token from the `GITHUB_TOKEN` environment variable. Never print or write the token anywhere.
- If neither is available, give the user the exact description, topics and homepage to paste into the repo's About gear icon.

## 4. CI/CD with GitHub Actions
Create `.github/workflows/pages.yml` (or update it) that:
- Triggers on push to the branch and on `workflow_dispatch`
- Uses minimal permissions (`contents: read`, `pages: write`, `id-token: write`)
- Runs a validation job first (for example, check the HTML file exists and is non-empty; add lint/tests when the project has them)
- Then builds and deploys with `actions/configure-pages`, `actions/upload-pages-artifact`, `actions/deploy-pages`
- Pins actions to major versions and never uses secrets it does not need

## 5. Push
- Initialise git if needed; set the remote to `https://github.com/<owner>/<repo>`.
- Set a repo-local `user.name`/`user.email` only if none is configured (never `--global`).
- Commit with a clear message and push the branch. Use `GIT_TERMINAL_PROMPT=0` so it never hangs, and never force-push unless the user explicitly asks.
- If push fails on authentication, tell the user what credential is needed; do not ask them to paste a token into chat.

## 6. GitHub Pages
- Set Pages source to "GitHub Actions" (`gh api -X POST repos/<owner>/<repo>/pages -f build_type=workflow`, or tell the user to set Settings > Pages > Source).
- Wait for the workflow run to finish (check `gh run list` or the public Actions API) and confirm the site returns HTTP 200.

## 7. Report
Finish with:
- Repo link and Pages link
- Workflow run status
- Security scan result (what was checked, what was found, what was changed)
- Anything the user still has to do by hand (for example enabling Pages, or the About fields if no token was available)
