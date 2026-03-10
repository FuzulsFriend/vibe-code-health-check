# Performance Rules

Common performance pitfalls and how to detect them.

---

## 1. Waterfall Requests

**What it is:** Making API calls one after another when they don't depend on each other.

**How to detect:**
- Look for sequential await calls that don't use each other's results
- Search for: multiple \`await fetch\` or \`await supabase\` in the same function

**Example:**
\`\`\`
// BAD - waterfall (takes 3 seconds total)
const users = await fetchUsers();     // 1 second
const products = await fetchProducts(); // 1 second
const orders = await fetchOrders();    // 1 second

// GOOD - parallel (takes 1 second total)
const [users, products, orders] = await Promise.all([
  fetchUsers(),
  fetchProducts(),
  fetchOrders()
]);
\`\`\`

---

## 2. Bundle Size

**What to check:**
- Total JS bundle size (should be under 500KB gzipped)
- Individual heavy dependencies

**Common offenders:**
| Package | Size | Lighter Alternative |
|---------|------|-------------------|
| moment.js | 330KB | date-fns (tree-shakeable) or dayjs (2KB) |
| lodash (full) | 531KB | lodash/specific-function (individual imports) |
| chart.js (full) | 200KB+ | Use tree-shaking or lighter chart lib |
| highlight.js (all languages) | 1MB+ | Import only needed languages |

**How to check:** Run \`npm run build\` and check output sizes. Or use \`npx bundle-analyzer\`.

---

## 3. Image Optimization

**Rules:**
- Use WebP or AVIF format (30-50% smaller than JPEG/PNG)
- Size images to display size (don't load 4000px image for a 400px display)
- Lazy load images below the fold
- Use Next.js \`<Image>\` component (automatic optimization)
- Add width and height to prevent layout shift

**How to detect:**
- Search for \`<img\` tags without \`loading="lazy"\`
- Check image file sizes (anything over 500KB for a web image is suspicious)
- Look for PNG screenshots that could be JPEG/WebP

---

## 4. React Re-render Issues

**What to check:**
- Missing \`key\` props on list items
- Objects/arrays created in render (cause child re-renders)
- Heavy computations without useMemo
- Missing useCallback on event handlers passed to memoized children

**Common patterns:**
\`\`\`
// BAD - new object every render, children always re-render
<Component style={{ color: 'red' }} />

// GOOD - stable reference
const style = useMemo(() => ({ color: 'red' }), []);
<Component style={style} />
\`\`\`

---

## 5. Database Performance

**Rules:**
- Select only needed columns (not SELECT *)
- Paginate list queries (LIMIT + OFFSET or cursor)
- Add indexes for frequently queried columns
- Avoid N+1 queries (fetching related data in a loop)
- Cache expensive queries where appropriate

---

## 6. Network Performance

**Rules:**
- Minimize the number of network requests on page load
- Use HTTP/2 or HTTP/3 (most modern hosts support this)
- Compress responses (gzip/brotli - usually handled by hosting)
- Cache static assets with appropriate headers
- Prefetch data for likely next actions

---

## Quick Performance Checklist

\`\`\`
[ ] No waterfall requests (independent fetches use Promise.all)
[ ] Bundle under 500KB gzipped
[ ] No heavy dependencies with lighter alternatives
[ ] Images optimized (WebP, sized appropriately, lazy loaded)
[ ] React lists have unique keys
[ ] Database queries select only needed fields
[ ] Database queries are paginated
[ ] No N+1 query patterns
\`\`\`
