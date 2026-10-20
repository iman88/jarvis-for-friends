---
name: onboard
description: First-run intake. Ask the user about themselves and the jobs they want, then create me/profile.md, me/applications.md and a starting CV. Use when me/profile.md is missing or the user says "start over" / "update my preferences".
---

# Onboarding

Goal: understand who the user is and what job they want, so every later search and CV edit fits them. Be kind; many people running this just lost their job.

## 1. Set up `me/`

Create `me/` and copy `templates/profile.md` → `me/profile.md` and `templates/applications.md` → `me/applications.md` if they don't exist.

## 2. Ask, in small groups (use AskUserQuestion where options fit, free text otherwise)

Don't dump all questions at once. Three or four rounds:

**About you**
- Name, city and country you live in, citizenship / work permits (EU citizen? UK/US visa?)
- Current or last role, years of experience, main skills and tools
- Languages and level
- What you're proud of in your last 2-3 jobs (raw material for the CV)

**Where you can work**
- Countries you'd accept jobs in
- Region type: only my country / EU-wide remote / Europe incl. UK / worldwide remote / willing to relocate (where?)
- Work mode: fully remote / hybrid (how many days, which cities) / on-site
- Travel: how many weeks a year is fine

**What job**
- Target job titles (2-5) and seniority
- Industries to target or avoid; companies to never apply to (past employers, bad experiences)
- Contract type: permanent / contract / freelance (own company? VAT registered?)
- Pay: minimum salary or day/hour rate, currency, gross or net
- Earliest start date, notice period
- Must-have and deal-breaker tech or duties

**Material**
- Do you have a CV file? Ask them to put it in `me/` (PDF, DOCX or MD) and read it.
- LinkedIn profile URL. Offer to read it in the browser after they log in themselves (see CLAUDE.md, never ask for the password).

## 3. Save

Write the answers to `me/profile.md` using the template's headings. Read it back in a short summary and ask the user to correct anything.

## 4. Next step

Suggest the order: CV first (`/cv`), then LinkedIn profile tidy-up, then job search (`/job-search`). Let them choose.
