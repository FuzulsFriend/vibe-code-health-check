# Vibe Code Health Check

> A Claude Code skill that scans any codebase and grades it A through F across 6 health dimensions — with plain-English fixes, not developer jargon.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://code.claude.com)
[![SKILL.md](https://img.shields.io/badge/SKILL.md-open%20standard-purple)](SKILL.md)

---

## What it does

Built for vibe coders, indie hackers, and solo founders who ship fast and want to know: **is this safe to launch?**

Scans your codebase across 6 health dimensions, runs the actual build, checks for real security issues, and explains everything in plain English — no senior dev required.

**6 health dimensions:**

| Dimension | Weight | What it checks |
|-----------|--------|---------------|
| Security | 25% | Exposed secrets, unprotected routes, injection vulnerabilities |
| Error Handling | 20% | Missing try/catch, unhandled promises, crash-prone code |
| Code Structure | 15% | File size, duplication, naming, organization |
| Performance | 15% | Bundle size, waterfall fetches, unoptimized assets |
| Deploy Readiness | 15% | Build pass/fail, env config, .gitignore coverage |
| UX Basics | 10% | Loading states, error messages, mobile responsiveness |

Stack-aware checks for: **React/Next.js, Python/Flask/FastAPI, Supabase, Firebase.**

---

## Install

```bash
npx skills add FuzulsFriend/vibe-code-health-check
```

Or manually: copy `SKILL.md` into your `.claude/skills/` directory.

**Recommended for live testing:**
```bash
npx skills add lackeyjb/playwright-skill --skill playwright-skill
```

---

## Usage

Just ask Claude:

```
Check my code
```

```
Is my app ready to ship?
```

```
Vibe check my codebase
```

```
Audit my project for security issues
```

---

## What you get

```
VIBE CODE HEALTH CHECK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Project: my-saas-app
Stack:   Next.js 14, TypeScript, Supabase, Vercel
Files:   312 | Lines: 28,400 | Dependencies: 47

Overall Grade: B (82/100)

BREAKDOWN:
  Security:         [████████░░]  78/100
  Error Handling:   [████████░░]  80/100
  Code Structure:   [███████░░░]  72/100
  Performance:      [████████░░]  85/100
  Deploy Readiness: [█████████░]  90/100
  UX Basics:        [████████░░]  84/100

CRITICAL — Fix these before anyone uses your app:
1. "Your CSP header is in report-only mode..."
   Fix: app/api/middleware.ts — change Content-Security-Policy-Report-Only to Content-Security-Policy

WARNINGS — Fix these soon:
2. "434 places in your code log debug info..."
   Fix: Add ESLint rule: no-console
```

Plus: a visual browser report with animated grade badge, severity-colored finding cards with copy-to-clipboard file paths, stats dashboard, and path-to-next-grade timeline.

---

## What's included

```
vibe-code-health-check/
├── SKILL.md                             # Main skill instructions
├── assets/
│   ├── report-template.md               # Text report template
│   └── report-ui.html                   # Visual browser report (health theme)
└── references/
    ├── scoring-rubric.md                # Exact criteria per dimension
    ├── security-patterns.md             # 8 vulnerability types with examples
    ├── react-nextjs-checks.md           # React/Next.js specific checks
    ├── python-flask-checks.md           # Python/Flask/FastAPI checks
    ├── supabase-firebase-checks.md      # BaaS security gotchas + positive credit
    ├── error-handling-rules.md          # Error handling patterns with code examples
    ├── performance-rules.md             # 6 performance rules with examples
    └── grade-thresholds.md             # What each grade (A–F) looks like
```

---

## Works with

- **Agent Teams mode** — all 6 checks run in parallel
- **Playwright CLI** — live app testing as a real user
- **Chrome DevTools MCP** — console errors + page speed

Enable Agent Teams:
```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

---

## Grading scale

| Grade | Score | What it means |
|-------|-------|--------------|
| A | 90–100 | Production-ready. Ship it. |
| B | 80–89 | Almost there. Fix the warnings and you're good. |
| C | 70–79 | Functional but risky. Fix critical issues first. |
| D | 60–69 | Significant problems. Not safe to deploy yet. |
| F | Below 60 | Major security or stability issues. |

Side projects get adjusted weights — security stays high, structure and deployment are weighted lighter.

---

## What it catches (based on real audits)

- API keys or secrets committed in source code
- Admin routes without authentication
- SQL injection vulnerabilities
- Unhandled promise rejections
- Missing error boundaries in React
- CSP headers in report-only mode instead of enforced
- Unverified webhook signatures
- Test login endpoints deployed to production
- Fragile import workarounds that will break if "cleaned up"
- Files over 500 lines doing too many things
- `SELECT *` queries on large tables
- 434 console.logs scattered in production code

---

## License

MIT — use it, fork it, improve it, share it.

---

*Built with the [SKILL.md open standard](https://github.com/FuzulsFriend/vibe-code-health-check/blob/main/SKILL.md) — works across Claude Code, Codex CLI, Gemini CLI, and 15+ other AI coding assistants.*
