# Supabase / Firebase Specific Checks

Additional checks for projects using Backend-as-a-Service platforms. These platforms handle a lot of infrastructure, but they have their own security gotchas.

---

## Supabase

### Security
- [ ] Row Level Security (RLS) enabled on all tables with user data
- [ ] RLS policies are specific (not \`true\` for all operations)
- [ ] \`service_role\` key NEVER used in client-side code (only \`anon\` key in browser)
- [ ] \`service_role\` key only in server-side code / Edge Functions
- [ ] Storage buckets have appropriate policies (not public for private files)
- [ ] No \`SUPABASE_SERVICE_ROLE_KEY\` in \`NEXT_PUBLIC_\` env vars

### Common Mistakes
- [ ] Not checking \`.single()\` returns for null before using
- [ ] Missing \`.eq('user_id', userId)\` on queries (relying solely on RLS)
- [ ] Using \`supabase.auth.admin\` in client-side code
- [ ] Storing sensitive data in user metadata instead of a secured table

### Error Handling
- [ ] Checking \`error\` return from all Supabase calls
- [ ] Handling auth session expiry gracefully
- [ ] Retrying on network failures

---

## Firebase

### Security
- [ ] Firestore Security Rules are NOT set to \`allow read, write: if true\`
- [ ] Security rules check authentication (\`request.auth != null\`)
- [ ] Security rules check authorization (\`request.auth.uid == resource.data.userId\`)
- [ ] Admin SDK only used server-side
- [ ] API key restrictions configured in Google Cloud Console
- [ ] Storage rules restrict file uploads by type and size

### Common Mistakes
- [ ] Firestore rules allowing any authenticated user to read all data
- [ ] Not validating data structure in security rules
- [ ] Using \`collection.get()\` without limits (scanning entire collections)
- [ ] Storing all user data in a single document (Firestore 1MB doc limit)

### Error Handling
- [ ] Checking for auth state changes (\`onAuthStateChanged\`)
- [ ] Handling permission denied errors from Firestore
- [ ] Retrying on network failures
- [ ] Handling quota exceeded errors gracefully

---

## Positive Signals (give credit)

When scoring Security, don't just penalize missing items — give explicit credit for good practices:

### Supabase
- ✅ RLS enabled on ALL user-data tables → +5 on Security (mention in report as a win)
- ✅ SECURITY DEFINER functions for sensitive mutations → +5 on Security
- ✅ service_role key only in server-side code → note as "correctly handled"
- ✅ Storage bucket policies properly configured → note as positive

### Firebase
- ✅ Granular security rules (not just `auth != null`) → +5 on Security
- ✅ Data validation in security rules → +5 on Security
- ✅ Admin SDK only server-side → note as positive

These bonuses cap at the dimension maximum (100). Report them explicitly in the "Wins" section of the report — good security deserves recognition.
