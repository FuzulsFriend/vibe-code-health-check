# Report Output Template

Use this exact format when presenting the final health check to the user.

---

## Format

\`\`\`
VIBE CODE HEALTH CHECK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Project: [PROJECT_NAME]
Stack: [DETECTED_STACK]
Files: [FILE_COUNT] | Lines: [LINE_COUNT] | Dependencies: [DEP_COUNT]
Mode: [full with live testing / code-only]

Overall Grade: [GRADE] ([SCORE]/100)
[ONE_SENTENCE_VERDICT]

BREAKDOWN:
  Security:            [BAR]  [SCORE]/100  [!! if below 60]
  Error Handling:      [BAR]  [SCORE]/100  [!! if below 60]
  Code Structure:      [BAR]  [SCORE]/100
  Performance:         [BAR]  [SCORE]/100
  Deploy Readiness:    [BAR]  [SCORE]/100
  UX Basics:           [BAR]  [SCORE]/100

CRITICAL — Fix these before anyone uses your app:

1. "[ISSUE — plain English, one sentence]"
   Where: [FILE_PATH:LINE if applicable]
   Fix: [SPECIFIC_ACTION]
   Effort: [quick fix / few hours / redesign needed]

2. ...

WARNINGS — Fix these soon:

3. "[ISSUE — plain English]"
   Where: [FILE_PATH:LINE]
   Fix: [SPECIFIC_ACTION]
   Effort: [ESTIMATE]

4. ...

SUGGESTIONS — Nice to have:

5. "[IMPROVEMENT — plain English]"
   Fix: [SPECIFIC_ACTION]
   Effort: [ESTIMATE]

6. ...

BUILD STATUS:
  [PASS/FAIL]: [build command output summary]
  Type check: [PASS/FAIL/SKIPPED]
  Dependency audit: [X critical, Y high, Z moderate vulnerabilities]

YOUR PATH FROM [CURRENT_GRADE] TO [NEXT_GRADE]:
1. [STEP] ([DIMENSION]: [CURRENT] -> [PROJECTED])
2. [STEP] ([DIMENSION]: [CURRENT] -> [PROJECTED])
3. [STEP] ([DIMENSION]: [CURRENT] -> [PROJECTED])
Estimated time: [TOTAL]
Expected new grade: [PROJECTED_GRADE] ([PROJECTED_SCORE]/100)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Checked by Vibe Code Health Check — By Tomer & Guy
github.com/FuzulsFriend
\`\`\`

---

## Progress Bar Format

Each bar is 10 characters wide:

\`\`\`
100/100  ██████████
 90/100  █████████░
 80/100  ████████░░
 70/100  ███████░░░
 60/100  ██████░░░░
 50/100  █████░░░░░
 40/100  ████░░░░░░
 30/100  ███░░░░░░░
 20/100  ██░░░░░░░░
 10/100  █░░░░░░░░░
  0/100  ░░░░░░░░░░
\`\`\`

Add \`!!\` after scores below 60.

---

## Rules

1. **Plain English only.** No jargon in findings.
2. **File paths when possible.** Point to the exact file and line.
3. **Specific fixes.** Not "improve error handling" but "add try/catch to the fetchUsers call in src/api/users.ts:23."
4. **Honest verdict.** One sentence capturing the biggest strength AND weakness.
5. **Conservative projections.** Under-promise on the "path to improvement" scores.
6. **Build status always included.** Even if it passes, mention it (it's a win).
7. **Side project adjustment noted.** If weights were adjusted, say so.

---

## Live Testing Section (if Playwright available)

Add after the breakdown:

\`\`\`
LIVE TEST RESULTS:
  Main flow: [what happened when testing the primary user flow]
  Mobile (375px): [observations]
  Console errors: [count and summary]
  Screenshots: [what was captured]
\`\`\`

---

## Language Check (before presenting)

- No "leveraging", "robust", "seamless", "cutting-edge"
- No passive voice in recommendations
- Every finding understandable by a non-developer
- Tone: helpful senior dev friend, not code police
