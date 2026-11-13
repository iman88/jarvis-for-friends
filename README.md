# Jarvis for friends

Jarvis is an assistant that helps you write your CV, fix up your LinkedIn profile, find jobs that fit you and apply to them. It runs in Claude Code on your own computer. Nothing is sent without your OK.

Getting started is three steps: **A, B, C**. After that, Jarvis guides you through the rest.

## A. Get Claude

You need a paid Claude plan (**Pro** is enough). Sign up at https://claude.ai.

## B. Install Claude Code

**Windows:** press the Windows key, type `PowerShell`, open it, paste this line and press Enter:

```powershell
irm https://claude.ai/install.ps1 | iex
```

**Mac:** open **Terminal** (Cmd+Space, type Terminal), paste this and press Enter:

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Close the window when it finishes. If it fails, see the official guide: https://docs.claude.com/en/docs/claude-code/setup

On Windows, if the installer or Claude Code says it needs **Git for Windows**, install it from https://git-scm.com/download/win, clicking Next with the default options, then try again.

You don't need anything else: no Python, no code editor, no programming. To navigate LinkedIn, Jarvis needs a browser tool that runs on Node.js. Jarvis installs it for you, with your OK, and walks you through it.

## C. Get Jarvis and start it

1. Open https://github.com/iman88/jarvis-for-friends, click the green **Code** button → **Download ZIP**. No GitHub account needed.
   (If you use git: `git clone https://github.com/iman88/jarvis-for-friends.git`)
2. Unzip it somewhere easy, e.g. `Documents\jarvis-for-friends`.
3. Open that folder, then:
   - Windows: click the address bar of the folder window, type `powershell`, press Enter.
   - Mac: right-click the folder → **New Terminal at Folder**.
4. Type `claude` and press Enter. Log in with your Claude account the first time.
5. If it asks whether to trust the folder or use the **playwright** server, say **yes**.
6. Type: **Hi Jarvis, let's find my next job**

Jarvis checks your computer and installs what's missing, one step at a time. The main piece is the browser tool it uses to navigate LinkedIn for you. You'll see a browser window open, and you log in to LinkedIn there yourself, once. If your computer fights the install, Jarvis tells you exactly what to do. You can still work on your CV in the meantime.

Commands you can type later:
- `/setup`: run the installation checks again
- `/onboard`: update your preferences (countries, remote/hybrid, roles, pay)
- `/cv`: build, review or tailor your CV
- `/job-search`: find jobs and apply

## Getting updates

Jarvis gets better over time. To update:
- **ZIP:** download the new ZIP, unzip it to a new folder, then **copy your `me` folder** from the old folder into the new one. Your CV, preferences and applications are all in `me`. Then delete the old folder.
- **git:** run `git pull` in the folder. Your `me` folder is never touched.

## Your data and your password

- Everything about you stays in the `me/` folder on your computer. Nobody else sees it, Ionut included.
- **Jarvis never asks for your LinkedIn password.** It opens LinkedIn in a browser window and you log in there yourself. Don't paste passwords into the chat.
- Jarvis fills in forms, but only you approve Submit.
- LinkedIn doesn't like automation, so Jarvis works at a human pace. Use it for your own search, and don't ask it to mass-apply.
- This repository is public. If you open a GitHub issue or share a fork, **never include anything from `me/`**, screenshots of your CV, or your contact details.

## Problems and ideas

Open an [issue](https://github.com/iman88/jarvis-for-friends/issues) (describe the problem, without personal data), or just message Ionut.

## License

Free to use, share and adapt under [CC BY 4.0](LICENSE). If you reuse it, please credit it with a link to https://github.com/iman88/jarvis-for-friends.

## For the maintainer: 🔄 Maintenance & planned updates

> You don't need this section to use Jarvis. It is the maintainer's checklist: what this kit depends on and
> when each part needs a look. Checked on 2026-11-13.

| Component | Current version | End of life / event | Action needed | Urgency |
|---|---|---|---|---|
| Playwright MCP (the browser tool) | `@playwright/mcp@0.0.85`, pinned in `.mcp.json` and the setup skill (2026-11-13) | Pre-1.0 and released often (about every 2 weeks); each release builds on an alpha Playwright | Check for a newer version, test it (`--help` starts, a LinkedIn search, one Easy Apply form), then move the pin in both files | Monthly ✅ |
| Node.js minimum | The setup wizard requires Node 22+; the portable fallback downloads Node 26 LTS | Node 22 support ends 2027-04-30; Node 24 ends 2028-04-30; Node 26 ends 2029-04-30 | Before April 2027, raise the minimum to Node 24+ | 2027-03 ✅ |
| Claude Code install commands | `install.ps1` / `install.sh` in the README | Can change | Check both commands still work on a clean machine | Every 3 months |
| LinkedIn pages | Read and filled by the job-search skill. On 2026-11-13 search opened on the new `/jobs/search-results` page (drops Remote and newest-first from the link, adds a salary filter, mixes in hybrid jobs); the skill now checks each job's work type itself | LinkedIn changes its pages without notice | Do a test search and open one Easy Apply form (never submit) | Monthly |
| CV to PDF | `templates/cv-page.html`, printed with Edge or Chrome | Browsers update themselves | Build one CV PDF from the template and look at it | Every 3 months |
| Personal data | — | Every push | Run the personal-data check before and after every push | Every push |
