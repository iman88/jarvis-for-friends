---
name: setup
description: Step-by-step setup wizard that gets the Playwright MCP browser working (Jarvis needs it to navigate LinkedIn), installing Node and picking Edge or Chrome as needed. No-browser mode is only a last resort. Use on first run (no me/setup.md), when the browser tools are missing or fail, or when the user says "setup", "help me install", "it doesn't work".
---

# Setup wizard

Assume the user is **not technical**. One step at a time, wait for them after every step, and never paste a wall of commands. Explain each step in one plain sentence ("This installs the browser I use to read LinkedIn for you."). When you need a command run, run it yourself with the shell tool if you can. Only ask the user to type something when it needs them (an installer window, admin rights, restarting Claude Code).

Progress lines look like: **Step C of D: the browser** ✅ / ❌.

Write results to `me/setup.md` (OS, what works, mode chosen) so later sessions skip what's done.

## Step A: what computer

Check it yourself (`$PSVersionTable` / `uname -a`). Windows, Mac or Linux decides the commands below. Tell the user what you found.

## How to talk while you fix things

The user may already feel low. A wall of red errors can make them think they broke something or that this isn't for them. So:
- **Before each action, say what you're doing and why, in one plain sentence.** For example: "I'm installing Node.js now. It's the engine the browser tool runs on. This takes about a minute."
- **Never paste raw errors or long command output.** Translate them: "Windows blocked the installer the first time. That's normal, so I'll try another way."
- **Frame failures as normal and fixable.** It's never their fault. Don't use words like "fatal", "critical", "broken" or "corrupted", or list everything that could go wrong.
- **Say what happens next, and what they need to do (usually nothing).** If they must act (click Yes, restart Claude Code), say exactly what they'll see and what to click.
- **Celebrate when a step works.** "Node is installed ✅. One step left."
- If something could change their computer in a way they'd notice (an installer, an admin prompt, an antivirus setting), ask first and explain why.

## When the straightforward way doesn't work

Don't stop at the first failure. Work out why it failed (permissions, missing program, network, antivirus, path) and try the next workaround below. Tell the user calmly that you're trying another way. Only after the workarounds run out, offer the screen share with Ionut and no-browser mode.

**The browser is required.** Jarvis can't navigate LinkedIn (search, read jobs, Easy Apply) without the Playwright MCP browser, so steps B and C are worth the effort. Be patient and encouraging, and keep going until it works.

## Step B: Node.js (the browser tool runs on it)

Run `node -v`.
- Version 18 or higher: ✅, go on.
- **Windows, missing or old:** install it yourself with winget, which comes with Windows 10/11:
  `winget install --id OpenJS.NodeJS.LTS --exact --silent --accept-package-agreements --accept-source-agreements`
  Tell them first: "A Windows permission window will pop up. Click **Yes**." When it finishes, they must **close Claude Code and open it again** in the same folder (`/exit`, then `claude`), because only new windows see Node. Then re-check.
  If `winget` isn't found or fails: ask them to open https://nodejs.org, download the **LTS** Windows installer, run it and click Next with the defaults. Then restart Claude Code.
- **Mac, missing or old:** ask them to open https://nodejs.org, download the **LTS** macOS installer (.pkg) and click Continue through it. Then restart Claude Code.
- **Workarounds if that fails:**
  - *Windows, no admin rights or the installer is blocked:* use the portable Node, which needs no install and no admin rights. Download the Windows x64 `.zip` of the current LTS from https://nodejs.org/dist/latest-v24.x/ (check the actual file name on that page) with `Invoke-WebRequest`. Unzip it to `$env:LOCALAPPDATA\node` with `Expand-Archive`, and add that folder to the **user** PATH: `[Environment]::SetEnvironmentVariable("Path", $env:Path + ";<folder>", "User")`. Restart Claude Code.
  - *Download blocked or very slow:* check whether they're on a company VPN or proxy. Try again off the VPN, or have them download the file in their browser and tell you where it is.
  - *Node installed but `node` still not found:* the PATH hasn't refreshed. Make sure they restarted Claude Code, not just the chat. If needed, use the full path (`C:\Program Files\nodejs\node.exe`) and the full `npx.cmd` path in the MCP command.
- Windows, error "running scripts is disabled on this system": run
  `Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force`, then re-check.

## Step C: the browser (Playwright MCP)

1. **Pick the browser before the first try.** Playwright MCP uses the browser already installed, and by default that's Chrome.
   - Windows without Chrome (`Test-Path "C:\Program Files\Google\Chrome\Application\chrome.exe"` is false, and the `(x86)` path too): switch to Edge, which every Windows has:
     `claude mcp add playwright -s local -- npx @playwright/mcp@latest --browser msedge`
     This local setting overrides the project's `.mcp.json` for this computer only.
   - Mac without Chrome (`ls "/Applications/Google Chrome.app"` fails): ask them to install Chrome from https://www.google.com/chrome, or use Edge if they have it, with `--browser msedge`.
2. Pre-download the server once, so the first start doesn't time out: `npx -y @playwright/mcp@latest --help`.
3. Restart Claude Code (`/exit`, then `claude`). If it asks to use the **playwright** MCP server, they answer **yes**.
4. Test: check that the `mcp__playwright__*` tools are available, then `browser_navigate` to `https://www.linkedin.com`. A browser window should open. ✅
5. Not working? Ask them to type `/mcp` and read you the status next to **playwright**, then fix in this order, restarting Claude Code after each fix:
   - **Not listed / rejected:** they declined the project server. Add it locally (the command from 1, with `--browser msedge` or without the flag for Chrome).
   - **Failed, on Windows:** wrap npx with cmd:
     `claude mcp remove playwright -s local` then `claude mcp add playwright -s local -- cmd /c npx @playwright/mcp@latest --browser msedge`
   - **Error says the browser isn't found:** switch browser (`--browser msedge` or `--browser chrome`).
   - **Antivirus blocking** (EPERM, access denied, the window opens and closes): tell them plainly, and walk them through allowing Node.js in the antivirus. Bitdefender in particular blocks it.
   - If `claude mcp add` says the name already exists, run `claude mcp remove playwright -s local` first.
   - **`npx` itself fails** (network or cache errors): install it once globally with `npm install -g @playwright/mcp@latest`, then add the server with the installed command instead of npx: `claude mcp add playwright -s local -- playwright-mcp --browser msedge`. On Windows, if that fails, wrap it: `cmd /c playwright-mcp --browser msedge`.
   - **Neither Chrome nor Edge works:** install Chrome. On Windows: `winget install --id Google.Chrome --exact --silent --accept-package-agreements --accept-source-agreements`. On Mac: the download from google.com/chrome. Then use `--browser chrome`.
   - **"Browser is already in use" / profile locked:** another Jarvis window or a crashed browser holds the profile. Ask them to close other Claude Code windows, or close leftover browser windows that Jarvis opened. As a last resort, add `--isolated`, but explain that they'll then need to log in to LinkedIn each session.
   - **Window opens but LinkedIn shows a security check or captcha:** that's LinkedIn being careful, not a failure. Ask them to complete it in the window themselves.
6. **Log in to LinkedIn:** once the window opens, navigate to `https://www.linkedin.com/login` and let them sign in themselves, including 2FA. The browser profile remembers it, so they log in only once.
7. Still stuck after these fixes: don't make them feel it's their fault. Suggest they ask Ionut for 10 minutes on a screen share. Meanwhile, offer **no-browser mode** (below) so they can work on the CV and applications today. Note what failed in `me/setup.md`.

## Step D: making PDFs

Nothing to install. The CV skill makes the PDF with Edge or Chrome, which are already there. Just check that one exists:
- Windows: `Test-Path "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe"` (or the `C:\Program Files\...` paths for Edge or Chrome).
- Mac: `ls "/Applications/Google Chrome.app"`.
If neither is found, that's fine. Note `pdf: print-dialog` in `me/setup.md`, and the CV skill will have them use Ctrl+P → Save as PDF instead.

## Done

Summarise in three lines what works. Then go to onboarding (`/onboard`).

---

## No-browser mode (last resort, temporary)

Only until the browser works. Jarvis can't navigate LinkedIn here, so the user does the clicking and you do the thinking:
- **LinkedIn profile:** ask them to open their profile → **More → Save to PDF** and put the file in `me/`. Read it.
- **Job search:** build the search link (see `/job-search`) and give it to them to click. They copy the job title + link + full description into the chat, or "Save as PDF" the job page into `me/jobs/`. You filter and shortlist exactly as normal.
- **Applying:** give them the answers to paste, field by field, and the CV file to upload. They press Submit themselves.
- **Profile edits:** give them the final text for each section to paste.

Record `mode: no-browser` in `me/setup.md`. At the start of each later session, offer to retry Step C, because job search is much faster with the browser.
