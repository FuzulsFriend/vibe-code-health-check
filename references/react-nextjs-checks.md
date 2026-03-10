# React / Next.js Specific Checks

Additional checks that apply specifically to React and Next.js projects. These supplement the main scoring rubric.

---

## Security (React/Next.js)

- [ ] \`NEXT_PUBLIC_\` env vars don't contain secrets (these are exposed to the browser)
- [ ] API routes in \`app/api/\` check authentication
- [ ] Server Actions validate input
- [ ] No \`dangerouslySetInnerHTML\` with user-provided content
- [ ] next.config.js doesn't expose sensitive headers

## Error Handling (React/Next.js)

- [ ] Error boundaries around major page sections
- [ ] \`error.tsx\` files in route segments for error recovery
- [ ] \`loading.tsx\` files for Suspense boundaries
- [ ] API route handlers have try/catch
- [ ] Server Actions have error handling
- [ ] \`not-found.tsx\` exists for 404 pages

## Code Structure (React/Next.js)

- [ ] Components under 300 lines (React components get long fast)
- [ ] Custom hooks extracted for reusable logic
- [ ] Client vs server components properly separated (\`"use client"\` only where needed)
- [ ] No business logic in components (extracted to hooks or utilities)
- [ ] Consistent file naming (PascalCase for components, camelCase for utils)

## Performance (React/Next.js)

- [ ] No waterfall data fetching (parallel fetches with Promise.all)
- [ ] Heavy components use dynamic imports (\`next/dynamic\`)
- [ ] Images use \`next/image\` (automatic optimization)
- [ ] Lists have unique \`key\` props (not array index for dynamic lists)
- [ ] Expensive computations use \`useMemo\` or \`useCallback\` where appropriate
- [ ] No unnecessary re-renders (check React DevTools profiler)
- [ ] Bundle analysis: no oversized dependencies
- [ ] Static pages use static generation where possible

## Deployment Readiness (React/Next.js)

- [ ] \`next build\` passes without errors
- [ ] \`tsc --noEmit\` passes (if TypeScript)
- [ ] ESLint passes (if configured)
- [ ] All required env vars documented in .env.example
- [ ] \`next.config.js\` has appropriate settings for production
- [ ] No \`console.log\` in production paths

## UX Basics (React/Next.js)

- [ ] Loading states on data-fetching pages (Suspense + loading.tsx)
- [ ] Error states on pages that fetch data
- [ ] Form validation with user-friendly messages
- [ ] Responsive layout (test at 375px, 768px, 1440px)
- [ ] Focus management on route changes
- [ ] \`alt\` text on all images
- [ ] Form inputs have labels

---

## Fragile Import Workarounds

Check for imports using internal subpaths instead of the package's main entry:

- `require('package-name/lib/internal-module')` instead of `require('package-name')`
- `import from 'package/dist/esm/submodule'` instead of `import from 'package'`

These often exist because the main entry breaks in specific deployment environments (e.g., `pdf-parse` main entry crashes on Vercel because it tries to read a test file from disk).

**Flag as:** WARNING (Deployment Readiness)
**Why it matters:** These workarounds are invisible traps. A developer "cleaning up" imports by switching to the main entry will break production. They need a code comment explaining WHY the subpath is used.
**Fix:** Add a comment above each fragile import explaining the workaround: `// IMPORTANT: Using subpath import because main entry crashes on Vercel (reads test file from disk)`

---

## AI Provider Configuration

If the project uses AI APIs (OpenAI, Anthropic, etc.), check for:

- **Model-specific parameter traps:** Some models don't support `temperature`, `top_p`, or `max_tokens`. Check if the code uses parameters that the configured model doesn't support. (e.g., GPT-5 models don't support `temperature` — using it throws an error)
- **Token parameter naming:** `max_tokens` vs `max_completion_tokens` — using the wrong one silently fails or errors
- **Hardcoded model names:** Model IDs should be in config/env, not scattered across files
- **Missing error handling on AI calls:** AI API calls are the most likely to fail (rate limits, outages, token limits). Every AI call needs try/catch with a meaningful fallback.

**Flag as:** WARNING (Deployment Readiness) for parameter traps, SUGGESTION for hardcoded models
