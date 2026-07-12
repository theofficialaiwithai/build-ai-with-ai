# Writing Bolt Prompts

Use this when the PRD doesn't include Bolt prompts and you need to write them yourself. These principles are grounded in Bolt's own prompting guidance and how the product actually behaves as of 2026.

---

## How Bolt Works (What You're Prompting Into)

Bolt runs a WebContainer — a full-stack dev environment inside the browser. When you send a prompt in Build mode (the default, non-Plan-mode state), Bolt:
1. Reads your prompt plus recent chat history (not the full history — see Context below)
2. Writes and edits code, installs packages, and updates the running app
3. Shows the result live in the Preview pane

The single biggest lever on result quality in Bolt is **scope discipline**: one clear task per prompt, explicit about what shouldn't change.

---

## The Five Things Every Prompt Should Include

**1. The application architecture, stated up front (first prompt only)**
Before Bolt writes anything, tell it your framework/tooling choices if you have them. This anchors every prompt that follows.

> "Build this as a React + Vite app with Tailwind CSS. Use Bolt Database for auth and data."

**2. One task per prompt, added incrementally**
Don't ask for multiple features in a single prompt. Add components one at a time, and give each one a small, specific instruction rather than a long list of requirements.

> "Add a reservation form to the booking page: date, time, party size, and a submit button."

**3. What should and shouldn't change — be explicit**
Bolt responds well to precise scoping. Reference specific files, elements, classes, or functions where possible, and say directly what shouldn't be touched.

> "Only modify the BookingForm component. Do not change the navbar, footer, or any other page."

**4. The acceptance criteria — what "done" looks like**
State the observable outcome so Bolt (and you) know when the step is finished.

> "When this is done, a user can fill out the form, submit it, and see a confirmation message. The reservation should appear in the database."

**5. Where secrets/config come from, for any step using external services**
Reference the actual mechanism — Bolt Database secrets, or environment variables — rather than assuming Bolt will invent a safe default.

> "Read the Stripe key from the project's Secrets as STRIPE_SECRET_KEY."

---

## Prompt Length and Scope

Bolt does best with focused, specific prompts — not exhaustive ones. If a feature needs auth + database + UI all at once, split it into separate steps: Bolt handles a single clear goal per prompt more reliably than a sprawling one.

Rule of thumb: if you're describing more than one distinct capability in a prompt, split it.

---

## When to Use the Enhance Prompt Feature

For a first prompt on a new project (or a big new feature), suggest using Bolt's **Enhance prompt** button (plus icon in the chatbox) instead of writing the full detailed prompt from scratch. It asks the user guided questions and expands a short prompt into a much more complete one — genuinely useful for a first-time builder who isn't sure what detail to include.

Don't use it for small, well-scoped edits later in a build — at that point a short, precise prompt is faster and uses fewer tokens.

---

## When to Use Plan Mode Instead of Writing a Prompt Directly

Recommend switching to Plan Mode before pasting a prompt when:
- The step involves a database schema decision or migration
- The step is a debugging session where the cause isn't obvious yet
- You want Bolt to compare two possible approaches before committing to one

Plan Mode messages cost fewer tokens than a Build-mode prompt that generates and possibly regenerates code, so it's also the token-efficient choice whenever the user isn't sure exactly what they want yet.

---

## Example: Weak vs. Strong

**Weak:**
```
Add a dashboard.
```

**Strong:**
```
Build the main dashboard page at the route /dashboard.

This is a React + Vite app using Bolt Database for auth and data.

The page should be protected — if a user isn't signed in, redirect them to /login.

The dashboard should show:
- A welcome message: "Welcome back, [user's first name]"
- A grid of project cards. Each card shows: project name (bold), a short description (max 2 lines, truncated), and a "View" button linking to /projects/[id].
- An "Add Project" button in the top-right linking to /projects/new.

Fetch only the projects belonging to the currently signed-in user. Show skeleton placeholder cards while loading.

Styling: Tailwind CSS. Cards should be white with rounded corners and a subtle shadow.

Keep the existing navbar and footer exactly as they are. Do not modify any other files.

Done looks like: a logged-in user sees only their own projects, and a second test account sees only its own.
```

---

## Bolt-Specific Prompt Patterns

**Bolt Database setup:**
> "Add authentication and a database using Bolt Database. Set it up automatically — no external account needed."

**Supabase setup (alternative):**
> "Connect this project to Supabase for the database and auth." (Requires the user to have connected Supabase via Bolt's integration first.)

**Targeting a specific element (pair with the Select tool in the UI):**
> "[Selection attached] Change this button's text to 'Get Started' and make it the primary accent color."

**Referencing a file directly:**
> "In @src/components/BookingForm.tsx, add validation so the date field can't be in the past."

**Debugging with console output:**
> "Here's the error from the browser console: [paste]. Please find and fix the cause without modifying unrelated files."

**Publishing-ready check:**
> "Before we publish, confirm there are no hardcoded secrets and all sensitive values are read from Secrets, not written directly in the code."

**Guardrail pattern (use in almost every prompt after the first):**
> "Only change [the specific thing]. Do not modify any other pages, components, or styles."

---

## Inferring the Stack and Format

If the PRD doesn't specify these and the user hasn't said, ask before writing any step prompts:

> "Before I write the prompts, a couple of quick questions:"
> - **Format:** a website, a web application, or a mobile app (via Expo)?
> - **Database, if you need one:** Bolt Database (built in, zero setup) or your own Supabase project?

Once confirmed, restate the key choices (framework, database) in prompts that touch the backend or app structure — Bolt's active context window is limited to recent messages, so repeating anchors prevents drift.

## Common Recommendations

Use these when the user asks you to choose:

- **Agent:** Standard for most day-to-day building — fast and token-efficient. Max only for large, interconnected codebases or genuinely open-ended problems.
- **Database:** Bolt Database for almost everyone — zero setup, managed entirely inside Bolt. Supabase only if the user wants to own/manage an external Supabase project directly.
- **Hosting:** Bolt's built-in `bolt.host` for the fastest path to a live URL. Netlify if the user already has a Netlify account and workflow. Expo for anything that needs to ship to the App Store or Google Play.
- **When stuck in an error loop:** Stop clicking "Attempt fix" repeatedly — each click costs tokens. Switch to Plan Mode, paste the actual error, and ask Bolt to investigate the root cause before making another change.
