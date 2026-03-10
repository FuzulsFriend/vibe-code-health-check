# Security Patterns

Common security vulnerabilities explained in plain English, with detection patterns and fixes. Based on OWASP Top 10 adapted for the types of projects vibe coders build.

---

## 1. Hardcoded Secrets

**What it is:** API keys, database passwords, or tokens written directly in the code instead of stored in environment variables.

**How to detect:**
- Search for: \`password\`, \`secret\`, \`api_key\`, \`apiKey\`, \`token\`, \`AWS_\`, \`SUPABASE_\`, \`FIREBASE_\`, \`sk-\`, \`pk_\`
- Check: .env files committed to git
- Check: config files with real credentials

**Common patterns:**
\`\`\`
// BAD - key in code
const supabase = createClient('https://xxx.supabase.co', 'eyJhbGciOiJIUzI1NiIsInR...');

// GOOD - key in env
const supabase = createClient(process.env.SUPABASE_URL, process.env.SUPABASE_ANON_KEY);
\`\`\`

**Fix:** Move all secrets to .env file. Add .env to .gitignore. Create .env.example with empty values.

---

## 2. Unprotected API Routes

**What it is:** API endpoints that return private data or perform sensitive actions without checking if the user is logged in.

**How to detect:**
- Find all API route files (app/api/**, routes/**, etc.)
- Check if they verify authentication before returning data
- Look for: missing auth middleware, no session check, no token validation

**Common patterns:**
\`\`\`
// BAD - anyone can access
export async function GET(req) {
  const users = await db.query('SELECT * FROM users');
  return Response.json(users);
}

// GOOD - checks auth first
export async function GET(req) {
  const session = await getSession(req);
  if (!session) return new Response('Unauthorized', { status: 401 });
  const users = await db.query('SELECT * FROM users WHERE org_id = $1', [session.orgId]);
  return Response.json(users);
}
\`\`\`

**Fix:** Add authentication middleware to every sensitive route. Check authorization (is this user allowed to access THIS data?).

---

## 3. SQL Injection

**What it is:** Building database queries by pasting user input directly into the query string. An attacker can type special characters to read, modify, or delete your entire database.

**How to detect:**
- Search for string concatenation in SQL queries
- Look for: template literals or string concat with user input in queries
- Check for raw SQL without parameterized queries

**Common patterns:**
\`\`\`
// BAD - user input directly in query
const result = await db.query(\`SELECT * FROM users WHERE name = '${req.query.name}'\`);

// GOOD - parameterized query
const result = await db.query('SELECT * FROM users WHERE name = $1', [req.query.name]);
\`\`\`

**Fix:** Use parameterized queries or an ORM. Never concatenate user input into SQL.

---

## 4. Cross-Site Scripting (XSS)

**What it is:** Allowing user-provided content to be displayed on the page without sanitization. An attacker can inject JavaScript that runs in other users' browsers.

**How to detect:**
- Search for: \`dangerouslySetInnerHTML\`, \`innerHTML\`, \`document.write\`
- Check if user-provided content is rendered without escaping
- Look for: URL parameters rendered directly in HTML

**Fix:** Use framework's built-in escaping (React does this by default). Never use dangerouslySetInnerHTML with user content. Sanitize HTML if you must render user content.

---

## 5. CORS Misconfiguration

**What it is:** Allowing any website to make API requests to your server.

**How to detect:**
- Search for: \`Access-Control-Allow-Origin: *\`, \`cors({ origin: '*' })\`, \`cors()\` with no config
- Check Next.js API routes for missing CORS headers
- Check middleware configuration

**Fix:** Restrict CORS to your own domain(s). In development, localhost is fine. In production, specify exact origins.

---

## 6. Missing Rate Limiting

**What it is:** No limit on how many requests a single user can make. An attacker can spam your API, run up your costs, or brute-force passwords.

**How to detect:**
- Check for rate limiting middleware (express-rate-limit, etc.)
- Check auth endpoints specifically (login, signup, password reset)
- Check API endpoints that cost money (AI calls, external APIs)

**Fix:** Add rate limiting to all public endpoints. Be stricter on auth endpoints (5 attempts per minute). Consider per-IP and per-user limits.

---

## 7. Insecure File Uploads

**What it is:** Accepting file uploads without validating the file type, size, or content.

**How to detect:**
- Find file upload handlers
- Check for: file type validation, size limits, filename sanitization
- Check if uploaded files are served from the same domain (could enable XSS)

**Fix:** Validate file types server-side (not just client-side). Limit file sizes. Store uploads in a separate storage service (S3, Supabase Storage). Never execute uploaded files.

---

## Quick Security Scan Checklist

\`\`\`
[ ] No secrets in source code (search for key patterns)
[ ] All API routes check authentication
[ ] Database queries use parameterized queries
[ ] No dangerouslySetInnerHTML with user content
[ ] CORS restricted to known origins
[ ] npm audit / pip audit has no critical vulnerabilities
[ ] .env in .gitignore
[ ] .env.example exists for team documentation
[ ] Rate limiting on auth and expensive endpoints
[ ] File uploads validated (if applicable)
[ ] No test/debug endpoints exposed in production
\`\`\`

---

## 8. Test and Debug Endpoints

**What to look for:**
- Routes with "test", "debug", "seed", "admin-seed" in the path
- Login endpoints that bypass normal auth (test-login, mock-auth)
- Endpoints that accept a shared secret instead of user auth
- Routes that expose internal state (health checks that show too much)

**Why it's dangerous:**
Test endpoints that exist in production code are attack vectors. A `/api/auth/test-login` with a shared secret is one leaked secret away from unauthorized access.

**What to flag:**
- Any test/debug endpoint without an environment guard (`if (process.env.NODE_ENV !== 'production')`)
- Shared secrets used for auth (AUDIT_SECRET, TEST_TOKEN) — these should be disabled in production
- Admin seed routes that can create privileged users

**Fix:**
- Wrap test endpoints in environment checks: `if (process.env.NODE_ENV === 'test') { ... }`
- Or better: move test routes to a separate file that's only included in test builds
- Never deploy shared-secret auth to production without IP restrictions
