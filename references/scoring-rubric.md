# Scoring Rubric

Exact scoring criteria for each of the 6 health dimensions. Every dimension is scored 0-100 using the checkpoints below. The overall score is a weighted average.

---

## 1. Security (Weight: 25%)

| # | Checkpoint | Points | Pass | Fail |
|---|-----------|--------|------|------|
| 1 | No hardcoded secrets | 25 | No API keys, passwords, tokens in source code. All secrets in .env files. | Any secret found in code |
| 2 | Protected API routes | 25 | All sensitive endpoints check for authentication. Admin routes have authorization. | Any route returning private data without auth |
| 3 | No injection vulnerabilities | 20 | Parameterized queries or ORM. User input sanitized. No eval() with user input. | Raw SQL concatenation, eval() on user strings |
| 4 | Secure dependencies | 15 | npm audit / pip audit shows no critical vulnerabilities. Dependencies reasonably current. | Critical vulnerabilities or 2+ major versions behind |
| 5 | CORS and headers | 15 | CORS restricted to known origins. Security headers present. | CORS allows all origins, missing security headers |

**Total: 100 points**

### Automatic Critical Flags
Always flagged as CRITICAL regardless of score:
- AWS/GCP/Azure key in source code
- Database password in source code
- JWT secret in source code
- API endpoint returning user data without auth
- SQL injection vulnerability
- eval() or Function() with user input

---

## 2. Error Handling (Weight: 20%)

| # | Checkpoint | Points | Pass | Fail |
|---|-----------|--------|------|------|
| 1 | Async error handling | 30 | Async functions have try/catch. Promises have .catch(). Fetch calls handle failures. | Unhandled promise rejections, async without try/catch |
| 2 | User-facing error states | 25 | API errors show friendly messages. Forms show validation errors. Network failures show retry option. | Raw error objects, stack traces, or blank screens |
| 3 | Error boundaries (React) | 15 | Error boundaries around major sections. One component error doesn't crash the app. | No error boundaries. (N/A for non-React) |
| 4 | Logging over console.log | 15 | Structured logging or no logging in production. No console.log in production paths. | console.log scattered in production code |
| 5 | Graceful degradation | 15 | Features fail individually, not globally. One API down doesn't break everything. | One failed call brings down the page |

**Total: 100 points**

### Notes
- Non-React: redistribute Error Boundaries 15 points to checkpoints 1 (+8) and 2 (+7)
- console.log behind NODE_ENV checks is acceptable
- console.log severity tiers:
  - 1-10: PASS — minimal logging, acceptable
  - 11-50: WARNING — "Clean these up before launch. Consider a lint rule."
  - 51-200: FAILURE (checkpoint 4 scores 0) — "This is a pattern problem. Add an ESLint rule: no-console"
  - 200+: CRITICAL — "This is systematic over-logging. You need a logging strategy (structured logger like pino/winston, log levels, production filter)."

---

## 3. Code Structure (Weight: 15%)

| # | Checkpoint | Points | Pass | Fail |
|---|-----------|--------|------|------|
| 1 | File size | 25 | No files over 500 lines. Most under 200. Each file has one purpose. | Files over 500 lines doing multiple things |
| 2 | Function size | 20 | No functions over 50 lines. Most under 20. Each does one thing. | Functions over 50 lines with multiple responsibilities |
| 3 | No code duplication | 20 | No 10+ line blocks repeated across files. Shared logic extracted. | Same 15-line block in 3 places |
| 4 | Consistent naming | 15 | One naming convention throughout. Descriptive names. | Mixed conventions, single-letter variables |
| 5 | Clean imports | 10 | No unused imports. Consistent import order. No circular deps. | 5+ unused imports per file |
| 6 | Dead code | 10 | No commented-out blocks. No unreachable code. No unused functions. | Large commented-out blocks, unused exports |

**Total: 100 points**

---

## 4. Performance (Weight: 15%)

| # | Checkpoint | Points | Pass | Fail |
|---|-----------|--------|------|------|
| 1 | No waterfall requests | 25 | Independent fetches run in parallel (Promise.all). | Sequential API calls that could be parallel |
| 2 | Bundle size | 20 | JS bundle under 500KB gzipped. No single dep over 200KB with lighter alternatives. | Bundle over 1MB or heavy deps (moment.js, full lodash) |
| 3 | Image optimization | 20 | Modern formats (WebP). Appropriate sizing. Lazy loading below fold. | Unoptimized PNGs 10x display size, no lazy loading |
| 4 | Frontend efficiency | 20 | No unnecessary re-renders. Keys on lists. Dynamic imports for heavy components. | Missing keys, expensive renders without memo |
| 5 | Database efficiency | 15 | Select only needed fields. Pagination. No N+1 queries. | SELECT * on large tables, queries in loops |

**Total: 100 points**

### Notes
- React checks only apply to React/Next.js. Redistribute for other stacks.
- Database checks only apply if project has DB access.

---

## 5. Deployment Readiness (Weight: 15%)

| # | Checkpoint | Points | Pass | Fail |
|---|-----------|--------|------|------|
| 1 | Build passes | 30 | npm run build completes without errors. | Build fails |
| 2 | Type safety | 20 | TypeScript: tsc --noEmit passes. No \`any\` on critical paths. | TypeScript errors, \`any\` on API/DB calls |
| 3 | Environment config | 20 | .env.example exists. .env in .gitignore. No secrets committed. | No .env.example, or .env committed |
| 4 | Error pages | 15 | Custom 404 and 500 pages. No stack traces in production. | Default framework error pages |
| 5 | .gitignore coverage | 15 | Covers node_modules, .env, build output, OS files. | Missing .gitignore or critical entries |

**Total: 100 points**

---

## 6. UX Basics (Weight: 10%)

| # | Checkpoint | Points | Pass | Fail |
|---|-----------|--------|------|------|
| 1 | Loading indicators | 25 | Spinners/skeletons on data-fetching pages. Button loading states. | Blank screen while loading |
| 2 | Error messages | 25 | User-friendly error messages. Form validation. Network retry option. | Silent failures, technical error codes |
| 3 | Mobile responsive | 20 | Works at 375px. No horizontal scroll. Readable text. | Broken mobile layout |
| 4 | Basic accessibility | 15 | Alt text, form labels, readable contrast, focus indicators. | No alt text, no labels, gray-on-white |
| 5 | Navigation clarity | 15 | User knows where they are. Back button works. Consistent nav. | No active state, broken back button |

**Total: 100 points**

---

## Overall Score Calculation

\`\`\`
Overall = (Security x 0.25) + (Error Handling x 0.20) + (Code Structure x 0.15)
        + (Performance x 0.15) + (Deployment x 0.15) + (UX Basics x 0.10)
\`\`\`

### Side Project Adjustment
If the user says it's a side project:
- Security weight: 25% -> 15%
- Code Structure weight: 15% -> 20%
- Deployment Readiness: 15% -> 10%
- Note this adjustment in the report
