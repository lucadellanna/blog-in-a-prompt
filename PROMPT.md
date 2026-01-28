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
- **Interpreting responses:** Treat "yes", "yeah", "sure", "ok", "yep", "sounds good" as affirmative. Treat "no", "nope", "nah" as negative. If ambiguous, ask for clarification.
- **When explaining errors to the user, use plain language**:
  - Avoid technical jargon
  - Explain what went wrong in simple terms
  - Tell them what you're doing to fix it
  - Example: Instead of "ENOENT error on package.json", say "I couldn't find a required file, so I'm creating it now"

### 2.2 Start-of-project initialization

At the beginning, do all of this:
1. Run `/init`
2. Create `.claude/rules/` and write a few short rules docs that extract the enforceable constraints from this file
3. Create `.claude/skills/` and add essential blog management skills (see Section 2.4 below)
4. Create `CLAUDE.md` with project-specific instructions (see below)

Required rules docs (create these exact filenames):

- `.claude/rules/00-scope.md`
- `.claude/rules/01-content-contract.md`
- `.claude/rules/02-publishing-flow.md`
- `.claude/rules/03-cli-first.md`
- `.claude/rules/04-project-structure.md`

Each rules doc must be short, clear, and enforceable. Suggested contents are provided in Section 12 below.

**CLAUDE.md requirements:**

Create a `CLAUDE.md` file in the project root with these exact contents:

```markdown
# Blog Management Instructions

This is a personal blog built with Blog in a Box. Keep all interactions simple and non-technical.

## Core Principles

1. **Always use the simplest language possible**
   - Never assume technical expertise
   - Explain what you're doing in plain terms
   - Avoid jargon (git, repo, deploy, CLI, etc.)
   - Example: Say "publishing your post" not "deploying to production"

2. **Do as much as possible automatically**
   - Never ask the user to run commands
   - Handle all git operations transparently
   - Fix issues yourself before asking for help
   - Only involve user for decisions (content, approval) and unavoidable auth steps

3. **Keep responses concise**
   - Be brief and actionable
   - Don't over-explain technical details
   - Focus on what the user needs to know, not how it works internally

## Publishing Workflow

When user provides a blog post draft:
- Use the `/publish` skill automatically
- Follow Section 10 of PROMPT.md exactly
- Show preview, wait for approval, then publish
- All git operations must be invisible to user

## Common User Requests

- "publish this" → Use `/publish` skill
- "check my blog" → Use `/blog-health` skill
- "update my about page" → Edit content/pages/about.mdx, follow publishing workflow
- "rollback" → Use `/rollback` skill

## Technical Constraints

- This is a minimal MVP: no images, tags, SEO fields, analytics, comments
- Content format: Markdown (treated as MDX)
- Post front matter: only `title` and `date` allowed
- All posts under `content/posts/`
- About page at `content/pages/about.mdx`

## Important Reminders

- Never skip the preview step when publishing
- Never merge to production without explicit user approval
- All git commands must be automatic and transparent
- Use plain language in all user communication
- Keep the MVP scope - no feature creep without explicit user request
```

**Why this matters:**
- Ensures consistent non-technical communication across all future Claude sessions
- Reinforces the "do it for them" philosophy
- Keeps the project maintainable by documenting core constraints
- **CRITICAL**: This file will be read by future Claude instances, so it must be clear and actionable

### 2.3 Required skills (Create after initial setup completes)

After the blog is successfully deployed, create these skills in `.claude/skills/`:

**`.claude/skills/publish.md`** (CRITICAL):
```markdown
# Publish Blog Post

Publish a blog post from draft to production.

## Trigger phrases
- "publish this"
- "publish this post"
- "add this to my blog"

## What this skill does
1. Extracts draft content from conversation
2. Follows Section 10 publishing workflow exactly
3. Handles all git operations automatically
4. Provides preview URL
5. Gates on user approval
6. Merges to main and deploys to production
7. Confirms production URL is live

## Important
- Never skip the preview step
- Never merge without explicit approval
- All git operations must be transparent to user
```

**`.claude/skills/blog-health.md`** (RECOMMENDED):
```markdown
# Blog Health Check

Diagnose and fix common blog issues.

## Trigger phrases
- "check my blog"
- "is my blog working"
- "diagnose blog issues"

## What this skill does
1. Checks production URL is accessible (HTTP 200)
2. Verifies posts render correctly (sample 2-3 posts)
3. Checks Vercel deployment status
4. Validates GitHub repo is in sync
5. Reports any issues found
6. Offers to fix problems automatically

## Common fixes
- Redeploy if production is stale
- Fix broken builds
- Sync git repo if out of sync
```

**`.claude/skills/rollback.md`** (RECOMMENDED):
```markdown
# Rollback Deployment

Rollback to a previous working deployment if something breaks.

## Trigger phrases
- "rollback"
- "undo that deployment"
- "revert to previous version"

## What this skill does
1. Lists recent Vercel deployments (vercel list)
2. Shows user the last 5 deployments with timestamps
3. Asks which to rollback to
4. Uses Vercel CLI to promote previous deployment
5. Verifies rollback succeeded
6. Provides restored production URL

## Important
- Always show deployment list before rolling back
- Confirm with user before promoting old deployment
- Verify the rolled-back version works
```

**When to create these skills:**
- Create them during the completion hardening steps (Section 2.5 step 9)
- This happens AFTER production deployment succeeds but BEFORE showing the completion message to the user
- Use the `/create-skill` command for each
- Test the `/publish` skill by having it create (but not publish to production) a second sample post - verify the workflow works

### 2.4 Version pinning and compatibility (Reduce brittleness)

To ensure the blog remains working over time:

**Pin critical dependencies in package.json:**
```json
{
  "dependencies": {
    "next": "^14.0.0 || ^15.0.0",
    "react": "^18.0.0",
    "react-dom": "^18.0.0",
    "@next/mdx": "^14.0.0 || ^15.0.0",
    "@mdx-js/loader": "^3.0.0",
    "@mdx-js/react": "^3.0.0"
  },
  "engines": {
    "node": ">=18.0.0"
  }
}
```

**Validate tooling versions before critical operations:**
- Before running vercel commands, check version: `vercel --version`
- If Vercel CLI < 28.0.0, warn user and suggest upgrade: `npm install -g vercel@latest`
- Document the versions used in a comment in project README for future reference

**GitHub authentication preference:**
- Default to HTTPS with Personal Access Token (simpler for non-technical users)
- Only suggest SSH if HTTPS fails repeatedly
- Document which auth method was used in `.claude/config.json` for consistency across sessions

**Deployment validation (reduce broken deployments):**
After every production deployment:
1. Wait 30 seconds for Vercel to finish building
2. Fetch the production URL with curl or similar: `curl -I <production-url>`
3. Verify HTTP 200 response
4. If non-200, check Vercel logs and fix issues before declaring success
5. Optionally visit `<production-url>/posts/<latest-post-slug>` to verify latest post renders

### 2.5 Completion hardening steps (Mandatory)

After you implement everything in this file and before declaring completion:
1. Re-read this entire file and verify every requirement is satisfied
2. Verify version pinning is correct in package.json (Section 2.4)
3. Run: `npx skills add https://github.com/vercel-labs/agent-skills --skill vercel-react-best-practices`
4. Run: `npx skills add https://github.com/vercel-labs/agent-skills --skill web-design-guidelines`
5. Run: `/vercel-react-best-practices`
6. Run: `/web-design-guidelines`
7. Run: `/code-review`
8. Apply any fixes required by best-practices, design guidelines, and code review, while preserving the MVP scope (no feature creep)
9. **Create the required skills** from Section 2.3 using `/create-skill`
10. **Test the `/publish` skill** by having it publish a second sample post
11. **Validate production deployment** works as described in Section 2.4

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

### 7.0 Gather required information

Before implementing, ask the user for these required details (use AskUserQuestion):

1. **Blog title**: "What would you like to name your blog?"
   - This will be displayed in the navigation
   - Example: "Jane's Tech Blog", "Daily Thoughts", etc.

2. **Repository name**: Suggest a kebab-case version of the blog title
   - Example: if blog title is "Jane's Tech Blog" → suggest "janes-tech-blog"
   - Use AskUserQuestion: "I'll create a GitHub repository called '[suggestion]'. Is that okay, or would you prefer a different name?"

Store these values to use throughout the setup process.

### 7.1 Choose project folder

- If user is in an empty folder, proceed
- If not, create a new folder in ~/Projects with the repository name
- Use the folder name from the repository name decided above

### 7.2 Preflight checks (agent does the checking)

- Confirm Node is installed and compatible with modern Next.js
- Confirm a package manager exists (npm is fine)
- Confirm git exists
- If something is missing, provide one short instruction to the user to install it, then continue when available

### 7.3 Scaffold Next.js app

Create a Next.js app using the official CLI:

```bash
npx create-next-app@latest . --typescript --tailwind --app --no-src-dir --import-alias "@/*"
```

**Important:** Use the `.` to install in the current directory.

If prompted interactively, answer:
- Would you like to use TypeScript? → Yes
- Would you like to use ESLint? → Yes
- Would you like to use Tailwind CSS? → Yes
- Would you like to use `src/` directory? → No
- Would you like to use App Router? → Yes
- Would you like to customize the default import alias? → No

Then implement:
- MDX support (next section)
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

**CRITICAL: The user should NEVER run git commands manually. You must handle ALL git operations automatically and transparently.**

You must:
- initialize git (if not already): `git init`
- **configure git identity** (required for commits):
  ```bash
  git config user.name "Blog Author"
  git config user.email "noreply@example.com"
  ```
  (Use generic values - user can change later if needed)
- commit the scaffold
- create the GitHub repo
- add remote
- push to main
- If GitHub auth is required, guide the user through it (AskUserQuestion for confirmations), then resume

**This applies to ALL git operations throughout the project lifecycle**, including:
- Committing new posts
- Pushing branches
- Merging to main
- Any other git operations

Never ask the user to run git commands. Never tell the user "you need to commit" or "you need to push". Do it automatically.

### 8.3 Troubleshooting git authentication

If git operations fail due to authentication issues:

**For HTTPS authentication errors:**
- Explain to the user in plain language: "GitHub needs permission to let me access your account"
- Guide them to create a Personal Access Token (classic) at https://github.com/settings/tokens
- Needed scopes: `repo` (full control of private repositories)
- Use AskUserQuestion to confirm they've created the token
- Configure git to use the token with: `git remote set-url origin https://<token>@github.com/<username>/<repo>.git`

**For SSH authentication errors:**
- Check if SSH keys exist: `ls ~/.ssh/id_*.pub`
- If no keys, guide user to: https://docs.github.com/en/authentication/connecting-to-github-with-ssh
- Explain: "We need to set up a secure connection to GitHub. Please follow the link I'm showing you."
- Use AskUserQuestion to confirm setup is complete before retrying

**Recovery approach:**
- Always try the operation first
- If it fails, diagnose the specific auth issue
- Explain the problem in simple terms
- Provide one clear solution
- Retry after user confirms

---

## 9. Vercel Setup (CLI-first)

### 9.1 Vercel authentication

**First, ensure Vercel CLI is installed:**
- Check if vercel exists: `which vercel` (Mac/Linux) or `where vercel` (Windows)
- If not found, install it: `npm install -g vercel`
- Wait for installation to complete

**Then authenticate:**
- Run `vercel login`
- This will open a browser window for the user to log in
- Use AskUserQuestion to confirm: "I've opened a browser window for you to log into Vercel. Please log in and then let me know when you're done."
- Wait for user confirmation, then continue

### 9.2 Create/link the Vercel project

Prefer connecting Vercel to the GitHub repo so that:
- Vercel creates preview deployments for branches/PRs
- Production deployment tracks main
- Use CLI capabilities as much as possible. Only fall back to dashboard steps if strictly necessary

**Critical: Verify GitHub-Vercel integration**

After linking the project, verify the integration is working:
1. Run `vercel git connect` or check connection status
2. Verify with: `vercel inspect` - should show "Connected Git Repository"
3. If not connected:
   - Guide user to Vercel dashboard > Project Settings > Git
   - Confirm GitHub repo is linked
   - Use AskUserQuestion to confirm connection is established
4. **Test the integration:**
   - Create a test branch: `git checkout -b test-vercel-integration`
   - Make a trivial commit: `echo "test" > .vercel-test && git add . && git commit -m "test vercel integration"`
   - Push: `git push -u origin test-vercel-integration`
   - Check if Vercel created a preview deployment: `vercel list`
   - If preview appears, integration is working - clean up test branch
   - If no preview, troubleshoot integration before proceeding

### 9.3 First production deployment

Trigger the first production deployment and obtain:
- production URL
- Confirm the site is live (at least by successful deploy output)

**Save the production URL for the user:**
- Create a file `.vercel-url` in the project root
- Write the production URL to this file
- This allows the user to easily find their blog URL later
- Example: `echo "https://my-blog.vercel.app" > .vercel-url`

**Vercel deployment troubleshooting:**

If the deployment fails:
1. **Check the build logs**: `vercel logs` or check the Vercel dashboard
2. **Common build failures**:
   - **Missing dependencies**: Add them to package.json and commit
   - **TypeScript errors**: Fix type errors in the code
   - **MDX parsing errors**: Verify MDX configuration in next.config.js
   - **Environment mismatch**: Ensure Node version is compatible
3. **Explain to the user**: Use plain language like "The website builder encountered an error. I'm fixing it now."
4. **Fix and retry**: Make the necessary fixes, commit, push, and redeploy
5. **Verify success**: Once deployed, visit the URL to confirm it's working

**If deployment succeeds but site doesn't work:**
- Visit the production URL yourself (simulate checking)
- Check for runtime errors in Vercel logs
- Common issues: missing environment variables, incorrect file paths, API route problems
- Fix and redeploy as needed

---

## 10. Publishing Workflow (Core Feature)

You must establish and follow this publishing protocol.

### 10.1 When the user provides a draft

**Do all steps yourself automatically. Never ask the user to run any commands.**

1. Normalize the draft (clean formatting, headings, spacing)
2. Create front matter: `title` (derived or proposed), `date` (today unless user specifies otherwise)
3. Create filename: `YYYY-MM-DD-kebab-case-title.mdx`
4. Create a new git branch: `post/YYYY-MM-DD-kebab-case-title`
5. Write file to: `content/posts/<filename>`
6. **Automatically commit the new file with a descriptive commit message**
7. **Automatically push the branch to GitHub**

All git operations must be transparent to the user. They should never see git commands or be asked to run them.

### 10.2 Preview deployment

Obtain a preview URL using the Vercel + Git integration (preferred).

If previews are not automatically available, you may deploy a preview via CLI and provide that URL, but still keep the branch-based flow.

**Timeout handling:**
- Wait up to 2 minutes for Vercel to create the preview deployment
- Check status with: `vercel list` or check the GitHub PR (if integration is working)
- If preview isn't ready after 2 minutes, check Vercel dashboard or logs for issues

**If preview deployment fails:**
1. Check Vercel deployment logs: `vercel logs <deployment-url>`
2. Common issues:
   - **Build errors**: Check that all dependencies are in package.json, run `npm install` locally to verify
   - **Environment variables**: Vercel deployments may need env vars set via `vercel env` or dashboard
   - **Vercel connection issues**: Ensure the Vercel project is properly linked to the GitHub repo
3. Explain the issue to the user in plain language
4. Fix the underlying problem (e.g., add missing dependencies, configure env vars)
5. Retry the deployment
6. If unable to resolve automatically, provide clear instructions for what the user needs to do

**Fallback:**
- If preview deployment repeatedly fails, you may proceed with showing the user the post content directly in the conversation
- Ask approval based on the content you show them
- Then merge to main and deploy to production
- Inform the user: "I couldn't create a preview, but here's what the post will look like. Should I publish it?"

### 10.3 Approval gate (Mandatory)

AskUserQuestion (single question):
"Publish to production? (yes/no)"

Rules:
- If "no": do not merge, do not deploy production, leave the branch as-is
- If "yes":
  - **Automatically merge the branch to main** (using git commands yourself)
  - **Automatically push to GitHub**
  - Wait for Vercel to deploy
  - Confirm production is updated and provide the production URL

**All git operations must be automatic.** Never tell the user to "merge the branch" or "push to main" - do it yourself.

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
- **Never ask the user to run git commands - handle all commits, pushes, merges, and branches automatically**
- The user should never see or interact with git - it should be completely transparent

### 12.5 .claude/rules/04-project-structure.md

Include:
- Enforce the required folder structure
- Posts under `content/posts/`
- About under `content/pages/about.mdx`
- Required app routes and filenames

---

## 13. Error Handling and User Communication

### 13.1 General principles for error handling

When anything goes wrong:

1. **Never show raw error messages to the user**
   - Bad: "Error: ECONNREFUSED 127.0.0.1:3000"
   - Good: "I couldn't start the local server. Let me try again with a different port."

2. **Explain what happened in plain language**
   - Bad: "Git push rejected due to non-fast-forward"
   - Good: "Someone else made changes to your blog. I'm updating your local copy first, then I'll try again."

3. **Tell them what you're doing to fix it**
   - Always include: "I'm [fixing/retrying/updating] now..."
   - This reassures them you're handling it

4. **Only involve the user when absolutely necessary**
   - Fix 90% of issues yourself automatically
   - Only ask for help with things you can't do (like creating accounts, auth tokens, DNS settings)

5. **When you need user action, be specific**
   - Bad: "Please fix your GitHub authentication"
   - Good: "I need you to create a GitHub access token. Please visit this link: [URL]. I'll wait here and we can continue when you're ready."

### 13.2 Common error scenarios

**Port already in use (dev server):**
- Solution: Use a different port automatically (`npm run dev -- -p 3001`)
- Tell user: "Port 3000 was busy, so I started the server on port 3001 instead."

**Package installation failures:**
- Solution: Clear cache and retry: `rm -rf node_modules package-lock.json && npm install`
- Tell user: "Had trouble installing dependencies. Cleaning up and trying again..."

**Vercel CLI not found:**
- Solution: Install it: `npm install -g vercel`
- Tell user: "Installing the Vercel tool I need to deploy your blog..."

**GitHub rate limits:**
- Solution: Wait and retry, or ask user to authenticate
- Tell user: "GitHub is asking me to slow down. Waiting 60 seconds before continuing..."

**File permission errors:**
- Solution: Check file paths, use proper permissions
- Tell user: "I don't have permission to write to that location. Using a different folder..."

### 13.3 Recovery strategies

Always attempt automatic recovery before involving the user:

1. **Retry with backoff**: For network errors, retry 2-3 times with increasing delays
2. **Alternative approaches**: If one method fails, try another (e.g., HTTPS vs SSH for git)
3. **Graceful degradation**: If preview fails, show content directly and proceed
4. **Clear state and retry**: For dependency/cache issues, clean and reinstall
5. **Use defaults**: When configuration is ambiguous, pick sensible defaults and proceed

**Only escalate to user when:**
- Authentication/credentials needed (can't automate)
- External service is down (can't fix)
- User needs to make a decision (domain name, blog title, etc.)
- Issue persists after 3 automatic retry attempts

Write a short README that includes:
- What this is
- How to run locally (npm install, npm run dev)
- How publishing works (draft to agent → preview → approval)
- Where content files live
- What is intentionally excluded (no tags, images, SEO, analytics, comments)

Do not over-document.

---

## 15. Final Checklist Before Declaring Completion

You must verify all of the following are true:
- Project structure matches Section 4 (Required Project Structure) exactly
- `/about` exists, is linked in nav, and renders from `content/pages/about.mdx`
- Home page lists posts newest-first
- Post pages render MDX from `content/posts/`
- Sample post exists and renders
- Repo exists on GitHub and is private by default
- Vercel project exists and production deploy succeeded
- Preview-first publishing flow is feasible and documented in README
- You did not add excluded features

Then do the completion hardening steps from Section 2.5:
- Re-read this entire file and verify compliance
- `npx skills add https://github.com/vercel-labs/agent-skills --skill vercel-react-best-practices`
- `npx skills add https://github.com/vercel-labs/agent-skills --skill web-design-guidelines`
- `/vercel-react-best-practices`
- `/web-design-guidelines`
- `/code-review`
- Apply fixes without scope creep
- Create the required skills from Section 2.3
- Test the `/publish` skill
- Validate production deployment

Only then declare completion.

---

## 16. Completion Message to User

After successfully completing all steps above, provide the user with this completion message:

```
🎉 Your blog is live!

Production URL: [paste the actual production URL here]

Your blog is now published and ready to use. Here's what you can do next:

📝 **Publish a new post:**
   Just paste your draft and say "publish this"
   I'll handle everything: formatting, preview, and deployment

✏️  **Edit the About page:**
   Say "update my about page" and tell me what you'd like to change

🔧 **Add features:**
   Want to add images, analytics, or other features?
   Just tell me what you'd like and I'll help you add it

🏥 **Check blog health:**
   Say "check my blog" anytime to verify everything is working

📚 **Manage posts:**
   - Edit existing posts: "edit the post about [topic]"
   - View all posts: Visit your blog's home page
   - Rollback if needed: "rollback to previous version"

💡 **Tips:**
   - Your blog repository is at: [GitHub repo URL]
   - The `/publish` skill makes publishing easy
   - All git operations happen automatically - you never need to run commands

What would you like to do next?
```

**Important:**
- Replace [production URL] and [GitHub repo URL] with actual values
- Be warm and encouraging in tone
- Give the user clear next steps
- Make it obvious they can continue the conversation to publish or modify the blog
