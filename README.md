# IT PMO Kanban

![HTML5](https://img.shields.io/badge/HTML5-E34F26?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?logo=githubactions&logoColor=white)
![GitHub Pages](https://img.shields.io/badge/GitHub_Pages-222222?logo=github&logoColor=white)

A single-page Kanban board for a fictitious bank's internal IT PMO. It is a demo and training tool, built with vanilla HTML, CSS and JavaScript in one file, with no framework, build step or dependencies.

**Live demo:** https://manellfab.github.io/mnl/

## Features
- Four columns: Backlog, In Progress, Blocked, Done, each with a live task count
- Drag and drop between columns, plus a keyboard-friendly "Move ▸" control
- Priority colour coding, overdue badges, inline delete confirmation
- Add Task form with inline validation
- Filters by project, assignee and priority; live summary strip
- Email notification for new tasks through FormSubmit (optional)
- In-memory only: refreshing the page resets the board to the demo data

## How to install
No installation or dependencies are needed.

```bash
git clone https://github.com/manellFab/mnl.git
cd mnl
```

## How to run
Open `index.html` in any modern browser (double-click the file). No server is required.

## Configuration
New-task email notifications use [FormSubmit](https://formsubmit.co). Replace `YOUR_EMAIL@example.com` in `FORMSUBMIT_ENDPOINT` near the top of the script in `index.html`. FormSubmit needs a one-time activation: the first submission sends a confirmation email, and notifications are delivered only after you click the link in it. Without this change the board works, and the app shows a warning toast when it cannot send the email.

## Project structure
```
index.html                      the whole app (markup, styles, script)
.github/workflows/pages.yml     CI/CD: validate and deploy to GitHub Pages
.claude/commands/               Claude Code project commands
```

## CI/CD
A GitHub Actions workflow validates `index.html` and deploys it to GitHub Pages on every push to `main`.

## Disclaimer
Fictitious demo. It is not affiliated with any real bank and uses no real logos or trademarks.
