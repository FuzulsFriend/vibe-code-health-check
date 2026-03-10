# Grade Thresholds

Detailed descriptions of what each grade means, what the codebase typically looks like at that grade, and what to focus on next.

---

## Grade A (90-100): Production-Ready

**What it means:** This codebase is ready to ship. Security is solid, errors are handled, code is well-organized, and the app performs well.

**What it looks like:**
- No hardcoded secrets
- All API routes protected
- Error handling on all external calls
- Clean, organized code structure
- Build passes cleanly
- Good loading and error states in the UI
- Mobile responsive

**What to focus on:** Minor polish. Consider adding monitoring, analytics, and automated testing.

**Who gets this grade:** Experienced developers, well-established codebases, projects that have been through multiple reviews.

---

## Grade B (80-89): Almost There

**What it means:** The codebase is solid but has some gaps. Most things work well, but there are areas that need attention before a serious launch.

**What it looks like:**
- Security basics covered but maybe missing rate limiting or CORS config
- Most error handling in place but a few gaps
- Code is organized but some files are getting long
- Build passes but with some warnings
- Loading states present but not everywhere
- Mobile works but not perfectly

**What to focus on:** Fix the warnings. Address the 2-3 specific issues in the report and you'll be at A level.

**Who gets this grade:** Good developers who built quickly and missed a few things. Most competent side projects.

---

## Grade C (70-79): Functional But Risky

**What it means:** The app works, but there are real risks. Security gaps, missing error handling, or UX issues that would frustrate users.

**What it looks like:**
- Some security issues (maybe exposed env vars or unprotected routes)
- Inconsistent error handling (some places handle errors, others don't)
- Code organization is okay but getting messy
- Build might have some issues
- Loading states missing in several places
- Mobile experience is rough

**What to focus on:** Fix critical security issues first. Then address error handling gaps. These two improvements alone could push you to B.

**Who gets this grade:** Vibe coders who built something that works but didn't focus on robustness. First-time builders.

---

## Grade D (60-69): Significant Problems

**What it means:** The codebase has serious issues that need to be fixed before real users touch it. Security vulnerabilities, frequent crashes, or deployment blockers.

**What it looks like:**
- Security vulnerabilities (hardcoded secrets, SQL injection risks)
- Many unhandled errors (app crashes on common failure scenarios)
- Code is disorganized (large files, duplicated code)
- Build may not pass cleanly
- Poor or no UX for errors and loading
- Mobile broken or untested

**What to focus on:** Security first. Move secrets to env vars, protect API routes. Then fix the top 3 crash scenarios. Don't worry about code style until security and stability are handled.

**Who gets this grade:** Very early prototypes, first-time coders building something ambitious, codebases with significant technical debt.

---

## Grade F (Below 60): Not Ready

**What it means:** Major issues across multiple dimensions. The codebase needs significant work before it's safe to deploy.

**What it looks like:**
- Serious security vulnerabilities (API keys in code, no auth on sensitive routes)
- No error handling (app crashes constantly)
- Code is hard to maintain (huge files, no structure)
- Build doesn't pass
- No loading or error states
- Mobile completely broken

**What to focus on:** Don't try to fix everything at once. Follow the "Path to improvement" in the report. Start with the 3 critical issues. Each one fixed will meaningfully improve the score.

**Who gets this grade:** Very early prototypes that haven't had any review. Codebases where the developer focused entirely on features and skipped infrastructure.

---

## Side Project vs Production

The same code might get different effective grades depending on context:

| Finding | Production Impact | Side Project Impact |
|---------|------------------|-------------------|
| Hardcoded API key | CRITICAL (data breach risk) | WARNING (still bad, but lower stakes) |
| No rate limiting | WARNING (cost/abuse risk) | SUGGESTION (low traffic = low risk) |
| No error boundaries | WARNING (crashes affect users) | SUGGESTION (only you see the crashes) |
| console.log everywhere | WARNING (performance + professionalism) | SUGGESTION (it's your console) |
| No 404 page | WARNING (looks unprofessional) | SUGGESTION (you know the routes) |

The scoring rubric has a "Side Project Adjustment" that shifts weights accordingly.
