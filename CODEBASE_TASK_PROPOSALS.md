# Codebase Task Proposals

## 1) Typo Fix Task
**Task:** Correct the seeded incident context text in `App.tsx` from **"Wait for matatu at Tom Mboya St."** to **"Waiting for a matatu at Tom Mboya St."** to fix wording quality in default sample data.

**Why this matters:** The seeded report is user-visible and currently reads like a typo/fragment instead of a complete sentence.

**Suggested acceptance criteria:**
- The default incident `context` string is updated to a complete, natural sentence.
- No behavior changes to seeded data beyond text correction.

---

## 2) Bug Fix Task
**Task:** Replace `process.env.API_KEY` usage with Vite-compatible environment access (`import.meta.env.VITE_GEMINI_API_KEY`) in Gemini client initialization.

**Why this matters:** Multiple runtime paths initialize `GoogleGenAI` using `process.env.API_KEY`, which is not the standard runtime env pattern in Vite browser apps. This can cause API calls to fail in production/browser builds.

**Suggested acceptance criteria:**
- `LiveCompanion`, `FakeCall`, and `geminiService` all read from `import.meta.env.VITE_GEMINI_API_KEY` (or a shared helper that does).
- App shows a clear fallback/error state if the key is missing.
- A production build still succeeds.

---

## 3) Code Comment / Documentation Discrepancy Task
**Task:** Reconcile environment variable documentation and in-code guidance so docs and code agree on one key name and access pattern.

**Why this matters:** README instructs users to set `GEMINI_API_KEY`, while code comments explicitly reference `process.env.API_KEY`. This mismatch creates onboarding confusion and setup failures.

**Suggested acceptance criteria:**
- README setup section uses the exact key name expected by runtime code.
- Comments that describe env usage in `services/geminiService.ts` are updated/removed to match actual implementation.
- A short `.env.example` is added to prevent drift.

---

## 4) Test Improvement Task
**Task:** Add a basic test setup (Vitest + React Testing Library) and a timer behavior test for `CheckInReminder` to verify `onPanic` fires when countdown reaches zero.

**Why this matters:** There is currently no test script or test suite, so critical safety flows (timers/distress escalation) are unverified and regressions are likely.

**Suggested acceptance criteria:**
- `package.json` includes a `test` script.
- One test file covers `CheckInReminder` countdown behavior using fake timers.
- Test runs successfully in CI/local via a single command.
