# Mega Prompt for Claude Code

## Zero-Friction Static Blog (Next.js App Router + MDX + Vercel + GitHub)

This is an **executable instruction set** for Claude Code.

If you are Claude Code reading this:
- Follow **all instructions exactly**
- Prefer **doing work yourself via CLI / APIs** instead of asking the user
- Ask the user questions **only when strictly necessary**
- When you do ask a question, use the **AskUserQuestion** tool
- Do **not** add extra features beyond the explicit scope
- Treat this file as authoritative

---

## 0. Objective

Create a **minimal, text-only blog** such that:
- The user never edits code manually
- The user can publish by pasting a draft to you
- You handle scaffolding, preview, and production publishing
- Deployment is on Vercel
- Source of truth is a GitHub repo (default private)

---

## 1. Hard Constraints (Do Not Violate)

- Framework: **Next.js (App Router)**
- Deployment: **Vercel**
- Source of truth: **GitHub repository**, default **private**
- Content format: **MDX**, but user writes **normal Markdown**
- Mandatory pages:
  - Home page listing posts
  - `/about` page (must exist and be linked in navigation)
- Publishing workflow must be:
  - Draft → Preview deployment → Explicit approval → Production
- Exclude all of the following:
  - tags/categories
  - images/media handling
  - SEO metadata fields (beyond minimal HTML title)
  - analytics
  - comments
  - authentication
  - CMS dashboards
  - search
  - newsletters
  - monetization

---

## 2. Claude Code Operating Rules

### 2.1 AskUserQuestion tool usage

Use **AskUserQuestion** when you need:
- a choice that changes outcomes (site title, repo name)
- confirmation an external auth step is complete (GitHub/Vercel login)
- the explicit publish gate ("Publish to production? yes/no")
- whether to configure a custom domain (yes/no, and domain name if yes)
- **content for the About page** (ask the user what they want written about themselves)

Rules:
- Minimize questions
- Ask one question at a time
- Do not ask the user to debug code

### 2.2 Start-of-project initialization

At the beginning, do all of this:
1. Run `/init`
2. Create `.claude/rules/` and write a few short rules docs that extract the enforceable constraints from this file

Required rules docs (create these exact filenames):

- `.claude/rules/00-scope.md`
- `.claude/rules/01-content-contract.md`
- `.claude/rules/02-publishing-flow.md`
- `.claude/rules/03-cli-first.md`
- `.claude/rules/04-project-structure.md`

Each rules doc must be short, clear, and enforceable. Suggested contents are provided in Section 12.

### 2.3 Completion hardening steps (Mandatory)

After you implement everything in this file and before declaring completion:
1. Re-read this entire file and verify every requirement is satisfied
2. Run: `npx skills add https://github.com/vercel-labs/agent-skills --skill vercel-react-best-practices`
3. Run: `/vercel-react-best-practices`
4. Run: `/code-review`
5. Apply any fixes required by best-practices and code review, while preserving the MVP scope (no feature creep)

---

## 3. Unavoidable Human Steps (Auth and Accounts)

Assume the user may not have:
- a GitHub account
- a Vercel account
- GitHub ↔ Vercel OAuth already approved

Rules:
- Guide the user through sign-in only when needed
- After the user completes auth, continue automatically and do everything else via CLI as much as possible
- Do not tell the user to click around in dashboards unless there is no CLI alternative

---

## 4. Required Project Structure (Must Match Exactly)

You must enforce this structure exactly:

```
/
├─ app/
│  ├─ page.tsx
│  ├─ about/page.tsx
│  └─ posts/[slug]/page.tsx
├─ content/
│  ├─ posts/
│  │  └─ YYYY-MM-DD-title.mdx
│  └─ pages/
│     └─ about.mdx
├─ lib/
│  └─ posts.ts
├─ components/
│  └─ Nav.tsx
├─ styles/
│  └─ globals.css
├─ next.config.js
├─ package.json
└─ README.md
```

Rules:
- All content under `content/`
- Posts sorted strictly by date descending
- Slugs derived from filenames (do not add a `slug:` field to front matter)
- `/about` must exist and be linked in the nav
- Keep code minimal and boring

---

## 5. Content Contract (Strict)

### 5.1 Post front matter (Mandatory and only these keys)

Every post must contain:

```yaml
---
title: "Human readable title"
date: "YYYY-MM-DD"
---
```

Rules:
- No other front matter keys allowed
- If missing, add them
- Validate and normalize dates
- Derive filename as: `YYYY-MM-DD-kebab-case-title.mdx`

### 5.2 About page content

`content/pages/about.mdx` must exist and be used by `/about`.

It can have front matter or not. If you include front matter, keep it minimal and do not introduce extra schema.

### 5.3 MDX constraints

- Treat user drafts as Markdown
- Do not require the user to write JSX
- Do not introduce custom MDX components in MVP
- You may use MDX pipeline so .mdx files render, but keep content plain Markdown

---

## 6. UI Requirements (Minimal)

- One-column layout
- Readable typography
- Nav at top with:
  - Site title (left)
  - Links: Home, About (right or inline)
- Home page shows list of posts (newest first):
  - Title (link)
  - Date
- Post page shows:
  - Title
  - Date
  - Content
- About page shows:
  - Content from `content/pages/about.mdx`

Rules:
- No theming system
- No dark mode toggle
- No client-side state required for core pages
- No images
- No tag UI
- Tailwind is allowed. shadcn is allowed but not required and must not bloat the MVP

---

## 7. Implementation Plan (Do This, Do Not Ask the User To Do It)

### 7.1 Choose project folder

- If user is in an empty folder, proceed
- If not, create a new folder
- Use AskUserQuestion only if you must choose a folder name

### 7.2 Preflight checks (agent does the checking)

- Confirm Node is installed and compatible with modern Next.js
- Confirm a package manager exists (npm is fine)
- Confirm git exists
- If something is missing, provide one short instruction to the user to install it, then continue when available

### 7.3 Scaffold Next.js app (no templates, no starters)

Create a minimal Next.js app (App Router). Do not clone starters.

Then implement:
- MDX support
- The required routes and file structure
- A minimal MDX loader using the filesystem at build/runtime on the server (no database)

### 7.4 Add MDX support

Implement MDX in a straightforward way compatible with App Router.

Requirements:
- .mdx files in content/ render correctly
- Use server-side file reading in lib/posts.ts
- Avoid unnecessary libraries

### 7.5 Create required content files

Create:
- `content/pages/about.mdx`:
  - Use **AskUserQuestion** to ask: "What would you like written on your About page? (Tell me about yourself, what you write about, etc.)"
  - Take the user's response and write a well-formatted About page based on their input
  - If the user provides minimal information, expand it slightly while staying true to what they said
  - Keep it concise and personal
- `content/posts/` with at least one sample post: `YYYY-MM-DD-hello-world.mdx` (use today's date)

### 7.6 Create required pages

Implement:
- `app/page.tsx` (home listing)
- `app/about/page.tsx` (renders about.mdx)
- `app/posts/[slug]/page.tsx` (renders post)

### 7.7 Local validation

Run the dev server and verify:
- `/` renders post list
- `/about` renders
- `/posts/<slug>` renders the sample post

If any errors:
- Fix them yourself
- Re-run until clean

---

## 8. GitHub Setup (Default Private Repo)

### 8.1 Repo naming and privacy

AskUserQuestion:
- repo name (suggest a default)
- confirm privacy: default is private, only make public if user explicitly requests it

### 8.2 Git operations

The user should not run git commands manually.

You must:
- initialize git (if not already)
- commit the scaffold
- create the GitHub repo
- add remote
- push to main
- If GitHub auth is required, guide the user through it (AskUserQuestion for confirmations), then resume

---

## 9. Vercel Setup (CLI-first)

### 9.1 Vercel authentication

Use the Vercel CLI.

If not logged in:
- Run `vercel login`
- AskUserQuestion to confirm the user completed the login flow in the browser
- Continue

### 9.2 Create/link the Vercel project

Prefer connecting Vercel to the GitHub repo so that:
- Vercel creates preview deployments for branches/PRs
- Production deployment tracks main
- Use CLI capabilities as much as possible. Only fall back to dashboard steps if strictly necessary

### 9.3 First production deployment

Trigger the first production deployment and obtain:
- production URL
- Confirm the site is live (at least by successful deploy output)

---

## 10. Publishing Workflow (Core Feature)

You must establish and follow this publishing protocol.

### 10.1 When the user provides a draft

Do all steps yourself:
- Normalize the draft (clean formatting, headings, spacing)
- Create front matter: `title` (derived or proposed), `date` (today unless user specifies otherwise)
- Create filename: `YYYY-MM-DD-kebab-case-title.mdx`
- Create a new git branch: `post/YYYY-MM-DD-kebab-case-title`
- Write file to: `content/posts/<filename>`
- Commit and push branch to GitHub

### 10.2 Preview deployment

Obtain a preview URL using the Vercel + Git integration (preferred).

If previews are not automatically available, you may deploy a preview via CLI and provide that URL, but still keep the branch-based flow.

### 10.3 Approval gate (Mandatory)

AskUserQuestion (single question):
"Publish to production? (yes/no)"

Rules:
- If "no": do not merge, do not deploy production
- If "yes": merge branch to main (via git / GitHub), then confirm production is updated

---

## 11. Custom Domain (Optional, After First Production Deploy)

After first successful production deploy:

AskUserQuestion:
"Do you want to add a custom domain now? (yes/no)"

If yes:
- AskUserQuestion for the domain name
- Guide the user through adding it in Vercel and updating DNS at registrar
- Do not assume access to registrar

This is advisory and must not block completion

If no:
- Do nothing

---

## 12. Required .claude/rules/* Files (Create These at Start)

Create `.claude/rules/` and write the following.

### 12.1 .claude/rules/00-scope.md

Include:
- This is a minimal text-only blog
- No tags, images, SEO fields, analytics, comments, auth, search, CMS
- Keep implementation boring and minimal
- Do not add features beyond scope

### 12.2 .claude/rules/01-content-contract.md

Include:
- Posts are MDX but treated as Markdown
- Required post front matter keys: title, date only
- No other front matter keys allowed
- Slug derived from filename
- About page mandatory from `content/pages/about.mdx`

### 12.3 .claude/rules/02-publishing-flow.md

Include:
- Draft → Preview → explicit approval → merge → production
- No production publish without explicit approval
- Use AskUserQuestion for approval gate
- Leave branch if user says "no"

### 12.4 .claude/rules/03-cli-first.md

Include:
- Prefer CLI for git, GitHub, Vercel
- Do as much as possible yourself
- Ask the user only for unavoidable auth/account steps
- Never ask the user to edit code manually

### 12.5 .claude/rules/04-project-structure.md

Include:
- Enforce the required folder structure
- Posts under `content/posts/`
- About under `content/pages/about.mdx`
- Required app routes and filenames

---

## 13. README Requirements (Minimal)

Write a short README that includes:
- What this is
- How to run locally (npm install, npm run dev)
- How publishing works (draft to agent → preview → approval)
- Where content files live
- What is intentionally excluded (no tags, images, SEO, analytics, comments)

Do not over-document.

---

## 14. Final Checklist Before Declaring Completion

You must verify all of the following are true:
- Project structure matches Section 4 exactly
- `/about` exists, is linked in nav, and renders from `content/pages/about.mdx`
- Home page lists posts newest-first
- Post pages render MDX from `content/posts/`
- Sample post exists and renders
- Repo exists on GitHub and is private by default
- Vercel project exists and production deploy succeeded
- Preview-first publishing flow is feasible and documented in README
- You did not add excluded features

Then do the completion hardening steps:
- Re-read this entire file and verify compliance
- `npx skills add https://github.com/vercel-labs/agent-skills --skill vercel-react-best-practices`
- `/vercel-react-best-practices`
- `/code-review`
- Apply fixes without scope creep

Only then declare completion.
