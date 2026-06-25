# Engineering Ledger — a portfolio built from your git history **and** your issue tracker

A single-page, editorially-designed engineering portfolio generated from your **actual commit and ticket history** instead of a hand-written résumé.

It draws on **three inputs, deliberately**:

- **Git** — the recent, quantitative record: commits by year, repo, language, and domain.
- **Your issue tracker (Jira / Linear / etc.)** — extends the timeline back to its *earliest records*, before your local clones begin. Pulling **anchor dates** (your earliest ticket, the first ticket in each major project/era) is reliable; pulling *complete* ticket counts may be limited by the connector.
- **You** — for whatever predates even the tracker. The agent reconciles the data with your memory and lets you correct it.

In the live example, git only reached back to 2020, and **Jira's earliest record was a manual-QA ticket from August 2017** — already years before any commit, which dated the move from manual testing → test automation → platform work. The **2016** start came one step further back, from **me correcting the agent**: I was doing manual QA at Weedmaps before Jira's records reach. So the timeline is git + tracker + a human correction — and it stays explicit about which is which.

**Live example:** https://tyler-james-bridges.github.io/portfolio/

> **How this one was made:** built interactively with **Cowork** (the agentic mode in the Claude desktop app — it has file, shell, and browser access, so it read the repos, ran git, and pushed to GitHub directly). The recipe below is deliberately **agent-agnostic**: it works with any coding agent that can read your repositories and run shell commands. The Cowork-specific part is just the convenience of doing the GitHub publish through the browser in the same session.

---

## What you get

- A career timeline reconstructed from real data (commits by year/month, by repo, by language, by domain).
- **The pre-git years recovered from your issue tracker** — earliest tickets and era-transition dates (anchor dates; full ticket counts may be connector-limited), plus anything earlier you supply from memory.
- A "where the work lives" breakdown so breadth is visible, not just claimed.
- A commit-activity heatmap rendered from your own data (not a screenshot of GitHub's).
- Flagship project write-ups in problem → approach → impact form.
- One self-contained `index.html` — no build step, deployable to any static host.

---

## The prompt

Copy this into an agent that has access to your code directory and a shell. Replace the bracketed bits.

```
You have access to my code directory at <PATH, e.g. ~/code> and a shell with git.

Goal: build a single-file HTML portfolio that tells my engineering story using
evidence mined from my own git history, then help me publish it to GitHub Pages.

1. IDENTITY. I commit under several names/emails. Find every author identity that
   is me by listing top authors across all repos, then confirm the set with me
   before counting. Exclude teammates with similar names.

2. MINE. Across every git repo in the directory, for my identities only
   (exclude merge commits), compute:
     - commits per year and per month
     - commits per repository (with first/last commit dates)
     - files touched by language/extension
     - issue-tracker ticket prefixes in commit messages (to infer focus areas)
   Treat local clones as possibly shallow — say so, and never present these as
   my complete history.

3. ISSUE TRACKER — DO NOT SKIP. My career predates my local git history, so the
   git numbers alone will undercount me by years. Query my issue tracker
   (Jira/Linear/etc.) for: the date of my earliest assigned/reported ticket
   (my real start), and the first ticket in each major project/era (e.g. when QA
   work began, when a dedicated automation or platform project opened). Use these
   as the verifiable anchors that extend the timeline back before any commit.
   Call out explicitly which eras come from the tracker vs. from git. Expect the
   connector to limit full ticket counts — anchor dates are the reliable part. And
   if the tracker doesn't reach my true start date, ASK me: I'll supply it, and
   you should mark it as a human-confirmed anchor, not invent one.

4. CATEGORIZE. Group repositories into a handful of domains (product, test
   automation, platform/CI, backend, AI, infra, …) and total commits per domain
   so breadth is visible. Assign each repo one primary domain; note the method.

5. DESIGN. Pick a deliberate visual direction with a point of view — NOT the
   default dark-mode-neon-gradient look. Use real typography and a restrained
   palette. Charts should match the design (flat, no stock widgets).

6. BUILD. Produce ONE self-contained index.html (inline CSS/JS; fonts and a chart
   library may load from a CDN). Sections: intro/about (first person), the record
   (headline stats), career arc, by-the-numbers charts, an activity heatmap built
   from the mined data, flagship project write-ups, and a toolbox.

7. PUBLISH. Create a public repo, commit index.html, enable GitHub Pages
   (deploy from main / root). If you can drive my browser, do it through my
   signed-in session; otherwise give me the exact git commands.
```

---

## Guardrails (the part that makes it honest)

- **Confirm identities before counting.** Commit-author data is messy; one wrong email skews everything.
- **Scope the numbers.** Local clones are often shallow and your work email may not map to a personal contribution graph. Label commit metrics as the "git era" and lean on the issue tracker for earlier years.
- **Don't let the earliest data point become your start date.** If neither git nor the tracker reaches your true beginning, ask and use a human-confirmed date — say which anchors are git, which are tracker, and which are you.
- **Don't fabricate impact.** Volume and range come from data; outcomes (flake reduction, coverage, deploy frequency, adoption) must come from you. Leave them out rather than inventing them — and never ship `[bracketed placeholders]` to a public page.
- **Verify before publishing.** Re-read the committed file and load the live page before calling it done.
- **Privacy.** Decide explicitly what's public (contact links, employer names, internal ticket IDs). Strip anything company-private.

---

## Customize

- **Design:** swap the typeface pairing and the single accent color (this example uses a deepened teal). Keep one accent and one strong display face.
- **Data source:** any issue tracker works for the timeline anchors; any static host works for deploy (GitHub Pages, Netlify, Cloudflare Pages).
- **Scope:** point it at one repo, one org, or everything — the mining steps are the same.

---

## Stack

Plain HTML/CSS/JS in a single file. Fonts and Chart.js load from a CDN; everything else is inline. No framework, no build step, no tracking.
