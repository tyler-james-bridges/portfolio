# Engineering Ledger — a portfolio built from your git history and your issue tracker

A single-page, editorially-designed engineering portfolio generated from your **actual commit and ticket history** instead of a hand-written résumé. Point an agent at your repositories and issue tracker; get a self-contained site you can deploy to any static host.

**Live example:** https://tyler-james-bridges.github.io/portfolio/

> **How it was made:** built with an agent that has file, shell, and browser access — in this case **Cowork** (the agentic mode in the Claude desktop app): it read the repos, ran git locally, and committed to GitHub through the browser. The recipe is **agent-agnostic** — it works with any coding agent that can read your repositories and run shell commands.

---

## Why two data sources (plus you)

- **Git** — the recent, quantitative record: commits by year, repo, language, and domain. Local clones are often shallow, so this usually only covers your last few years.
- **Your issue tracker (Jira / Linear / etc.)** — extends the timeline back to its earliest records, before your local clones begin. **Anchor dates** (your earliest ticket, the first ticket in each major project/era) are reliable; *complete* ticket counts may be limited by the connector.
- **You** — for whatever predates even the tracker. Neither source is guaranteed to reach your true start, so the agent should reconcile the data with your memory and let you correct it — never letting the earliest data point silently become your start date.

---

## What you get

- A career timeline reconstructed from real data (commits by year/month, repo, language, and domain).
- The pre-git years recovered from your issue tracker (anchor dates), plus anything earlier you supply yourself.
- A "where the work lives" breakdown so breadth is visible, not just claimed.
- A commit-activity heatmap rendered from your own data (not an embed of GitHub's graph).
- Concise flagship project write-ups.
- One self-contained `index.html` — no build step, deployable to any static host.

---

## The prompt

Copy this into an agent that has access to your code directory and a shell. Replace the bracketed bits.

```
You have access to my code directory at <PATH, e.g. ~/code> and a shell with git.

Goal: build a single-file HTML portfolio that tells my engineering story from
evidence in my own history, then help me publish it as a static site.

1. IDENTITY. I commit under several names/emails. Find every author identity that
   is me by listing top authors across all repos, then confirm the set with me
   before counting. Exclude teammates with similar names.

2. MINE (git). Across every git repo in the directory, for my identities only
   (exclude merge commits), compute:
     - commits per year and per month
     - commits per repository (with first/last commit dates)
     - files touched by language/extension
     - issue-tracker ticket prefixes in commit messages (to infer focus areas)
   Treat local clones as possibly shallow — say so, and never present these as
   my complete history.

3. ISSUE TRACKER — DO NOT SKIP. My career predates my local git history, so git
   alone will undercount me by years. Query my issue tracker (Jira/Linear/etc.)
   for: the date of my earliest assigned/reported ticket (my real start), and the
   first ticket in each major project/era (e.g. when QA work began, when a
   dedicated automation or platform project opened). Use these as verifiable
   anchors that extend the timeline back before any commit. Call out which eras
   come from the tracker vs. from git. Expect the connector to limit full ticket
   counts — anchor dates are the reliable part. If the tracker doesn't reach my
   true start date, ASK me: I'll supply it, and you should mark it as a
   human-confirmed anchor, not invent one.

4. CATEGORIZE. Group repositories into a handful of domains (product, test
   automation, platform/CI, backend, AI, infra, …) and total commits per domain
   so breadth is visible. Assign each repo one primary domain; note the method.

5. DESIGN. Pick a deliberate visual direction with a point of view — NOT the
   default dark-mode-neon-gradient look. Use real typography and a restrained
   palette. Charts should match the design (flat, no stock widgets).

6. BUILD. Produce ONE self-contained index.html (inline CSS/JS; fonts and a chart
   library may load from a CDN). Sections: intro/about (first person), the record
   (headline stats), career arc, by-the-numbers charts, an activity heatmap built
   from the mined data, concise flagship project write-ups, and a toolbox.

7. PUBLISH. Create a public repo, commit index.html, and enable GitHub Pages
   (deploy from main / root). If you publish through my browser, confirm I'm
   signed into the account that owns the repo first; otherwise give me the exact
   git commands to run.
```

---

## Guardrails (the part that makes it honest)

- **Confirm identities before counting.** Commit-author data is messy; one wrong email skews everything.
- **Scope the numbers.** Local clones are often shallow, and a work email may not map to a personal contribution graph. Label commit metrics as the "git era" and lean on the issue tracker for earlier years.
- **Don't let the earliest data point become your start date.** If neither git nor the tracker reaches your true beginning, ask and use a human-confirmed date — and say which anchors are git, which are tracker, and which are you.
- **Don't fabricate impact.** Volume and range come from data; outcomes (flake reduction, coverage, deploy frequency, adoption) must come from you. Leave them out rather than inventing them — and never ship `[bracketed placeholders]` to a public page.
- **Verify before publishing.** Re-read the committed file and load the live page before calling it done.
- **Privacy.** Decide explicitly what's public (contact links, employer names, internal ticket IDs) and strip anything private.

---

## Publishing

- A single static file deploys anywhere: GitHub Pages, Netlify, Cloudflare Pages.
- If you publish through a browser session, make sure you're signed into the account that **owns** the repo. With multiple accounts signed in (e.g. work + personal) it's easy to be on the wrong one — GitHub silently disables uploads when you lack push access, with no obvious error.

---

## Customize

- **Design:** swap the typeface pairing and the single accent color; keep one accent and one strong display face.
- **Data source:** any issue tracker works for the timeline anchors; any static host works to deploy.
- **Scope:** point it at one repo, one org, or everything — the mining steps are the same.

---

## Stack

Plain HTML/CSS/JS in a single file. Fonts and a chart library load from a CDN; everything else is inline. No framework, no build step, no tracking.
