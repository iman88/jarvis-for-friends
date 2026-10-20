---
name: job-search
description: Search LinkedIn for jobs that match me/profile.md, shortlist them with reasons, help apply (Easy Apply or company site) and log every application in me/applications.md. Use when the user asks to find jobs, check alerts, or apply.
---

# Job search

Always read `me/profile.md` and `me/applications.md` first. Never suggest a job that's already in the tracker or on an excluded-companies list.

## 1. Log in

Open `https://www.linkedin.com/login` in the Playwright browser and let the user sign in themselves. Never ask for the password.

## 2. Search

Build a LinkedIn search from the profile:
- `keywords`: boolean query from target titles and must-have skills, e.g. `("Financial Analyst" OR "FP&A") AND (Excel OR SAP)`
- `geoId`: European Union `91000000`, Europe `100506914`, United Kingdom `101165590`, worldwide `92000000`. For one country or city, find the geoId by typing it into LinkedIn's location box and reading the URL.
- `f_WT`: 1 on-site, 2 remote, 3 hybrid (comma-separate for several)
- `f_TPR=r604800` past week (`r86400` past 24 h), `f_AL=true` Easy Apply only, `sortBy=DD` newest first

Logged-in search: `https://www.linkedin.com/jobs/search/?keywords=...&geoId=...&f_WT=2&f_TPR=r604800&sortBy=DD`.

To collect many results quickly, fetch from the logged-in page (about 1 request/second):
- list: `/jobs-guest/jobs/api/seeMoreJobPostings/search?keywords=...&geoId=...&f_WT=2&f_TPR=r604800&start=N` (10 per page; split the HTML on `urn:li:jobPosting:` and regex out title, company, `job-search-card__location`, `datetime`). This endpoint ignores boolean operators, so it returns a broader list. Filter it yourself.
- details: `/jobs-guest/jobs/api/jobPosting/<id>` (strip tags and read the whole text).

Offer to create LinkedIn job alerts with the same search, so new jobs arrive by email.

## 3. Filter: read the full description, not the title

Check each candidate against the profile:
- **Location:** a country tag like "Germany (Remote)" usually means residents of that country only. "European Union" / "EMEA" / "Europe" usually means anywhere in that region. If unsure, read the description and the company careers page; the careers page wins over LinkedIn.
- **Work mode:** hybrid or on-site hidden in the text ("2 days in the London office").
- **Must-haves vs the user's real skills.** Note gaps honestly.
- Language requirements, travel, contract type, pay if shown.
- **Watch for:** reposting agencies (Jobgether, etc.) that list one role per country. Check the Benefits section for "remote within Europe" and treat the copies as one job. Also check for agencies hiding an excluded company as "our client", and for fake "jobs" asking for money or personal documents up front.

## 4. Shortlist

Show a short table: job (link), company, location/mode, why it fits, gaps, Easy Apply or company site. Then list what was rejected and why, in one line each. Let the user pick.

## 5. Apply

- Easy Apply: open the job, click Easy Apply, fill each page from `me/profile.md` and the CV. Untick "Follow <company>". For any question the profile doesn't answer (years with X, salary, notice), ask the user. Never guess numbers.
- Company site: fill what you can, ask the user for the rest, and let them create accounts and passwords themselves.
- On the review page, show every answer and **wait for an explicit OK** before Submit. Screenshot the confirmation.

## 6. Log

Add a row to the top of `me/applications.md`: date, company, role (link), stage, material sent, next step (prep topics, gaps). Stages: Applied → Screening → Take-home → Interviews → Offer / Rejected / Withdrawn.
