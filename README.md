# Open-source Issue Notifier (for GSoC & software-engineering contributors)

Every 10 minutes it checks ~78 open-source repos (Go, TypeScript, React, Next.js, Svelte, Node, databases,
plus GSoC regulars like Zulip, Oppia, Rocket.Chat, Mattermost), scores new/updated issues against **your**
skills, and e-mails you the good ones. Runs free on GitHub Actions. Gemini AI scoring is optional.

## Setup (about 15 minutes)
1. Create a **new public** GitHub repo and push this folder to it (public = unlimited free Actions minutes).
2. Open `config.json` and **rewrite `profile`** so it describes *your* skills (this is what Gemini uses to judge issues).
   Edit `skillKeywords` too if your stack is different.
3. Gmail: turn on 2-Step Verification, then create an **App Password**: https://myaccount.google.com/apppasswords
4. Gemini key (optional): https://aistudio.google.com/apikey
5. Your repo → Settings → Secrets and variables → Actions → New repository secret:
   - `GMAIL_USER` (your Gmail address)
   - `GMAIL_APP_PASSWORD` (the 16-character app password)
   - `GEMINI_API_KEY` (optional)
   Use **your own** keys. Never commit them and never share them.
6. Actions tab → **Validate repo list** → Run workflow. Read the log: `✗` = archived/missing, `→` = renamed.
   Fix `config.json` by hand, or locally: `GITHUB_TOKEN=xxx npm run validate -- --fix`
7. Actions tab → **Issue notifier** → Run workflow (use "Run workflow", not "Re-run", so the newest code runs).
   After that it runs by itself every ~10 minutes.

## Pick your focus repos (important)
Put **2-3** repos in `focusRepos` in `config.json`. Their issues get a score bonus and a ⭐ in the e-mail.
GSoC and similar programs choose people who already helped *that* project: depth beats breadth.
When Google publishes the GSoC organization list (in 2026 it was Feb 19; contributor applications ran
March 16-31 - check the official timeline for the new year), put 2-3 of those orgs' repos in `focusRepos`.
Eligibility: 18+, and a student or new to open source. Some orgs forbid AI-written proposals/code - read their rules.

## Using the e-mails
- The first person to politely ask usually gets the issue. Check the "opened X min ago" line and the ⚠ warning.
- Read `CONTRIBUTING.md` first. Many projects need `git commit -s` (DCO) or a CLA.
- Comment to ask to be assigned, then send a small, finished PR and answer review comments quickly.
- 5-10 merged PRs in 2-3 projects beat 50 typo fixes. Join the project's chat and review other people's PRs.
- Put merged PRs on your CV with links: what was broken, what you changed, the result.

## Settings (`config.json`)
`repos`, `focusRepos`, `skillKeywords`, `minScore`, `preferLabels`, `maxAiPerRun` (protects Gemini free quota),
`noAiMinScore` (e-mail very strong keyword matches even if Gemini is down).

## Good to know
- No Gemini key? It still works: beginner-labeled issues and strong keyword matches are e-mailed without the AI line.
- If Gemini shows `HTTP 404`, the model name was retired: set `GEMINI_MODELS` (comma-separated) in the workflow `env:`.
- GitHub may disable scheduled workflows after 60 idle days; a daily keep-alive step tries to prevent it.
  If you get a "workflow disabled" e-mail, press Enable in the Actions tab.
- The repo names were written from memory - run **Validate repo list** once.
- Local test: `npm install && DRY_RUN=1 GITHUB_TOKEN=xxx npm start`
