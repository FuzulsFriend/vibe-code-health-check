# Error Handling Rules

What good error handling looks like across different contexts.

---

## The Golden Rule

**Every external call can fail.** Network requests, database queries, file reads, API calls, third-party services — any of these can fail at any time. Good code handles failure. Bad code assumes everything always works.

---

## API / Network Calls

### JavaScript/TypeScript
\`\`\`
// BAD - no error handling
const data = await fetch('/api/users').then(r => r.json());

// GOOD - handles errors
try {
  const response = await fetch('/api/users');
  if (!response.ok) throw new Error('Failed to fetch users');
  const data = await response.json();
} catch (error) {
  // Show user-friendly message
  showError('Could not load users. Please try again.');
}
\`\`\`

### Rules
- Every \`fetch\` or \`axios\` call MUST have error handling
- Check HTTP status codes (response.ok or status >= 400)
- Show user-friendly error messages (not raw error objects)
- Log errors for debugging (but not in production console.log)
- Provide a retry mechanism for transient failures

---

## Database Queries

### Rules
- Every database call MUST have try/catch (or .catch())
- Handle: connection failures, query errors, timeout errors
- Never expose database error details to users
- Use transactions for multi-step operations

---

## React Components

### Error Boundaries
\`\`\`
// Create an error boundary for major sections
class ErrorBoundary extends React.Component {
  state = { hasError: false };
  static getDerivedStateFromError() { return { hasError: true }; }
  render() {
    if (this.state.hasError) return <div>Something went wrong. Please refresh.</div>;
    return this.props.children;
  }
}
\`\`\`

### Rules
- Wrap major page sections in error boundaries
- One component crashing should NOT crash the whole app
- Error boundaries should offer recovery actions (refresh, retry)
- Use Suspense boundaries with fallback loading UI

---

## Forms

### Rules
- Validate on the client AND the server (client for UX, server for security)
- Show errors inline next to the field (not just at the top)
- Don't clear the form on error (let users fix and retry)
- Show specific messages: "Email is required" not "Form error"
- Disable submit button while processing (prevent double submission)

---

## Loading States

### Rules
- Show a spinner, skeleton, or progress bar while data loads
- Never show a blank screen (users think it's broken)
- Skeleton loaders are better than spinners (less jarring)
- Long operations (>3 seconds) should show progress indication
- Buttons should show loading state after click

---

## What NOT To Do

| Bad Pattern | Why | Better |
|-------------|-----|--------|
| Bare catch that does nothing | Errors silently swallowed | Log the error, show user message |
| \`catch(e) { console.log(e) }\` | User sees nothing, dev sees console | Show user error, log for monitoring |
| Showing raw error.message to user | May contain sensitive info | Map to user-friendly messages |
| Catching too broadly | Hides the real problem | Catch specific error types |
| No loading state | User thinks page is broken | Show skeleton or spinner |
| Alert/prompt for errors | Jarring, blocks the page | Inline error messages |
