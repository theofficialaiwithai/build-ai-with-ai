# Writing v0 Prompts

Use this when the PRD doesn't include v0 prompts and you need to write them yourself. These principles are grounded in v0's own prompting guidance and how the product actually behaves as of 2026.

---

## How v0 Works (What You're Prompting Into)

v0 is an AI agent, not just a code generator. When you send a prompt, it can write and edit code, install dependencies, run terminal commands, search the web, and even open and click through the app it just built — all inside an isolated Vercel Sandbox — then show the result in the Preview tab.

The single biggest lever on result quality in v0 is **specificity**: vague prompts get generic results; prompts with concrete functionality, design preferences, and technical details get results that match what you actually pictured.

---

## Two Development Postures

**Direct implementation** — for simple, well-defined builds (a landing page, a single-purpose tool, a basic CRUD app). Just describe it and go.

**Planning first** — for multi-feature, multi-role, or genuinely complex builds. Ask v0 to draft a PRD or break the project into steps before writing code:
> "Create a PRD for a restaurant reservation system with customer-facing booking and a staff dashboard."

Then implement from that plan:
> "Build the MVP based on the PRD, starting with core features."

If a PRD is already provided for this build, we're already past this step — but if a single feature within it is unusually complex, it's fine to ask v0 to plan just that feature before prompting for the code.

---

## The Five Things Every Prompt Should Include

**1. The application architecture, stated up front (first prompt only)**
Before v0 writes anything, state your framework/tooling preference if you have one — v0 defaults to Next.js + Tailwind + shadcn/ui, which is usually right unless the user has a specific reason to deviate.

> "Build this as a Next.js App Router project with TypeScript and Tailwind CSS."

**2. One clear task per prompt, added incrementally**
Break large builds into pieces rather than asking for everything in one prompt. v0's own guidance is explicit about this: complex, multi-part features are better built incrementally than in one sprawling prompt.

> "Add a reservation form to the booking page: date, time, party size, and a submit button."

**3. What should and shouldn't change — be explicit**
Reference specific files, components, or pages, and say directly what shouldn't be touched.

> "Only modify the BookingForm component. Do not change the navbar, footer, or any other page."

**4. The acceptance criteria — what "done" looks like**
State the observable outcome so v0 (and you) know when the step is finished.

> "When this is done, a user can fill out the form, submit it, and see a confirmation message. The reservation should appear in the database."

**5. Where the backend comes from, for any step needing data or auth**
v0 has no built-in Cloud backend — name the actual integration.

> "Use the connected Supabase integration for the database and auth. Do not assume any other backend exists."

---

## Prompt Length and Scope

v0 does best with focused, specific prompts that still include enough context to avoid ambiguity — this is different from "as short as possible." A good prompt names the functionality, the design preferences, and the technical constraints in a few clear sentences or a short list, not a single vague line.

Rule of thumb: if a feature needs a database, an auth flow, and new UI all in one go, split it into separate steps — one for the schema/integration, one for the UI, one for wiring them together.

---

## When to Use Design Mode Instead of a Text Prompt

Recommend Design Mode (`Option+D` / `Alt+D`, or the design icon in the composer) instead of writing a prompt when:
- The change is purely visual — spacing, color, font, a single element's styling
- The user wants to point at something in the live Preview rather than describe it in words
- Multiple small visual tweaks need to happen to the same element before committing

For anything structural (new components, new logic, new routes), a text prompt is still the right tool — Design Mode's natural-language box is best for changes scoped to the selected element, not new features.

---

## Example: Weak vs. Strong

**Weak:**
```
Add a dashboard.
```

**Strong:**
```
Build the main dashboard page at the route /dashboard.

This is a Next.js App Router app using the connected Supabase integration for auth and data.

The page should be protected — if a user isn't signed in, redirect them to /login.

The dashboard should show:
- A welcome message: "Welcome back, [user's first name]"
- A grid of project cards. Each card shows: project name (bold), a short description (max 2 lines, truncated), and a "View" button linking to /projects/[id].
- An "Add Project" button in the top-right linking to /projects/new.

Fetch only the projects belonging to the currently signed-in user. Show skeleton placeholder cards while loading.

Styling: Tailwind CSS with shadcn/ui components. Cards should be white with rounded corners and a subtle shadow.

Keep the existing navbar and footer exactly as they are. Do not modify any other files.

Done looks like: a logged-in user sees only their own projects, and a second test account sees only its own.
```

---

## v0-Specific Prompt Patterns

**Database/auth setup (name the integration explicitly):**
> "Connect a Supabase database to this app and add authentication using it. Set up the schema for [describe the data]."

**Referencing a specific file or component:**
> "In the BookingForm component, add validation so the date field can't be in the past."

**Debugging with console/terminal output:**
> "Here's the error from the deployment logs: [paste]. Please find and fix the cause without modifying unrelated files."

**Asking v0 to use its agentic capabilities directly:**
> "Open the app and test the signup flow end to end. Report any failures you find."
> "Run the unit tests and fix any failures."

**Publishing-ready check:**
> "Before we publish, confirm there are no hardcoded secrets and all sensitive values are read from environment variables, not written directly in the code."

**Guardrail pattern (use in almost every prompt after the first):**
> "Only change [the specific thing]. Do not modify any other pages, components, or styles."

---

## Inferring the Stack and Format

If the PRD doesn't specify these and the user hasn't said, ask before writing any step prompts:

> "Before I write the prompts, a couple of quick questions:"
> - **Complexity:** is this a fairly simple build, or does it have multiple features/user roles worth planning out first?
> - **Data or auth needs:** none, or should we connect Supabase (database + auth together), Neon (Postgres only), or Upstash (Redis)?

Once confirmed, restate the key choices (framework, backend integration) in prompts that touch data or auth — this keeps every step self-contained even though v0's chat history is generally available in context.

## Common Recommendations

Use these when the user asks you to choose:

- **Backend:** Supabase for almost any app that needs both a database and auth — it's the only Marketplace option that bundles both. Neon if the user only needs a Postgres database with no built-in auth. Upstash if the need is specifically Redis (caching, rate limiting, queues).
- **Development posture:** Direct implementation for simple builds. Planning first (ask v0 for a PRD or step breakdown) for anything multi-role or multi-feature.
- **Visual tweaks:** Design Mode over a text prompt — faster and the result is exactly what's shown, not a guess from a description.
- **When a deployment breaks:** Try **Fix with v0** first (free up to 20/day on unedited code) before manually diagnosing or re-prompting.
