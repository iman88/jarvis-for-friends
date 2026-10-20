---
name: cv
description: Build or improve the user's CV in me/cv.md and render me/cv.pdf; also tailor it for a specific job or align the LinkedIn profile with it. Use when the user asks about their CV/resume, or before applying.
---

# CV

`me/cv.md` is the source of truth. The PDF is generated from it; never edit the PDF.

## Building the first version

1. Read `me/profile.md` and any CV file the user put in `me/`. If they're logged in to LinkedIn, read their profile page too (experience, education, skills).
2. If nothing exists, interview them role by role: title, company, dates, what they did, what tools, one thing that went well. Ask for numbers only if they know them.
3. Start from `templates/cv.md`. Keep it to **1-2 pages**:
   - Header: name, title, city + country, phone, email, LinkedIn URL
   - Summary: 2-3 plain sentences
   - Skills: grouped one-line lists
   - Experience: newest first, 2-5 bullets per role, older roles (10+ years) collapsed into one line each
   - Education, certifications, languages
4. Follow the writing rules in CLAUDE.md (short, plain, no AI tone, nothing invented).
5. Show the draft section by section; apply their edits.
6. Render the PDF (see "Making the PDF" below) and open it for them to check.

## Making the PDF (no Python, no extra installs)

1. Convert `me/cv.md` to HTML yourself and write `me/cv.html` from `templates/cv-page.html`, replacing the placeholder comment. Keep the template's CSS.
2. Print it to PDF with a browser that's already on the computer. Use absolute paths.
   - Windows (Edge is always there):
     `& "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" --headless --disable-gpu --no-pdf-header-footer --print-to-pdf="<abs>\me\cv.pdf" "<abs>\me\cv.html"`
     (If Edge isn't at that path, try `C:\Program Files\Microsoft\Edge\Application\msedge.exe` or Chrome at `C:\Program Files\Google\Chrome\Application\chrome.exe`.)
   - Mac with Chrome: `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless --no-pdf-header-footer --print-to-pdf="<abs>/me/cv.pdf" "file://<abs>/me/cv.html"`
   - If the PDF doesn't appear or is not newer than before, it's probably open in a viewer: ask them to close it and retry.
3. **Fallback, works everywhere:** open `me/cv.html` in their browser (Windows `Start-Process`, Mac `open`). Tell them to press Ctrl+P (Cmd+P on Mac), choose **Save as PDF**, turn off **Headers and footers**, and save it as `cv.pdf` in the `me` folder.
4. Open the PDF for them (Windows `Start-Process "<abs>\me\cv.pdf"`, Mac `open`) and ask them to check it page by page.

**Word version** (some recruiters ask for .docx): open `me/cv.html` in Word (right-click → Open with → Word), then File → Save As → Word Document. Or in Google Docs: File → Open → Upload the HTML, then File → Download → .docx.

## Reviewing an existing CV

Be candid. Point out: gaps without explanation, vague bullets ("responsible for testing"), walls of keywords, more than 2 pages, typos, outdated tools at the top, missing contact info, photo/age/marital status (drop them for most EU/UK jobs unless the target country expects them, e.g. Germany often still uses a photo).

## Tailoring for a job

Copy to `me/cv-<company>.md`, reorder skills and bullets so the job's must-haves come first. Don't add skills the user doesn't have. Make its PDF the same way, as `me/cv-<company>.html` → `me/cv-<company>.pdf`.

## LinkedIn profile

Suggest edits that match the CV: headline (short, e.g. "Senior Accountant"), About (3-4 plain sentences plus an optional "Skills: A, B, C" line, which helps recruiter search), experience bullets, Skills section (recruiter search weighs it a lot), "Open to work" settings. Make each edit by hand in the browser, one at a time, with a screenshot, and only after the user approves the text. On "Add position", LinkedIn offers to replace the headline. Pick the option that keeps the current one.
