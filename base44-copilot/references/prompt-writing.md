# Writing Base44 Prompts

Use this when the PRD doesn't include Base44 prompts and you need to write them yourself. These principles are grounded in Base44's own prompting guidance and how the product actually behaves as of 2026.

---

## How Base44 Works (What You're Prompting Into)

Base44's AI chat is agentic, not just a code generator: it can read your files, search the web, fetch content from a URL you give it, and directly read/write your app's database — all from a single instruction. It also picks the right underlying AI model for each request automatically (Automatic mode), so there's usually no need to think about which model to use.

The single biggest lever on result quality in Base44 is **being specific about scope and outcome** — because Base44 handles design, data, and hosting for you automatically, the thing worth being precise about is exactly what the feature should do and for whom, not the technical plumbing underneath it.

---

## Two Ways to Start a Feature

**Default mode, direct** — for a well-defined, single feature. Just describe it.

**Plan mode or Discuss mode first** — for anything with real ambiguity or multiple moving parts:
- **Plan mode** (only available on the very first prompt of a new app) asks structured clarifying questions and produces a plan before any credits are spent.
- **Discuss mode** (available anytime, mid-build) is the equivalent for later features — a free-form conversation to think through an approach before committing to a prompt. Costs a flat, small amount (0.3 credits) regardless of how long the conversation runs.

If a PRD already exists for this build, we're generally past the planning stage — but if one particular step is unusually ambiguous (a complex permissions model, an unusual workflow), it's worth suggesting Discuss mode for just that step before writing the real prompt.

---

## The Four Things Every Prompt Should Include

**1. What the feature does, plainly**
Base44 doesn't need architecture specified — it decides layout, styling, and data model. Focus the prompt on functionality and rules, not implementation.

> "Add a reservation form to the booking page with date, time, and party size fields, and a submit button."

**2. One clear task per prompt, added incrementally**
Base44's own credit guidance is explicit about this: focused prompts touching specific pages or components use fewer credits and get better results than one broad request covering the whole app.

> Not: "Build the whole booking system with forms, confirmations, and admin views."
> Instead: three separate prompts, one per piece, verified in between.

**3. What should and shouldn't change — be explicit**
Name the specific page, component, or entity, and say directly what shouldn't be touched.

> "Only change the BookingForm section on the Book page. Do not modify the navbar, footer, or any other page."

**4. The acceptance criteria and any access rules**
State the observable outcome, and — critically for Base44 — who should be able to see or do what. Base44 builds data access rules as part of the feature, so leaving this vague means the AI has to guess.

> "When this is done, any signed-in user can submit a reservation and see a confirmation message. Each user should only be able to see their own past reservations, not anyone else's."

---

## Prompt Length and Scope

Base44 does best with prompts that are short but complete — specific about functionality and access rules, without describing implementation details Base44 already handles automatically (styling frameworks, hosting, database engines). A good prompt reads like a clear feature request to a competent assistant, not a technical spec.

Rule of thumb: if a feature needs a new data model, a new page, and new access rules, consider splitting it into two prompts — one for the data/entity, one for the page/UI — verified separately.

---

## Example: Weak vs. Strong

**Weak:**
```
Add a dashboard.
```

**Strong:**
```
Add a dashboard page at /dashboard for signed-in users only. If someone isn't signed in, send them to the login page.

The dashboard should show:
- A welcome message: "Welcome back, [user's first name]"
- A grid of the user's own projects. Each card shows: project name (bold), a short description (max 2 lines), and a "View" button.
- An "Add Project" button in the top-right that opens a new-project form.

Each user should only ever see their own projects — never another user's.

Keep the existing homepage and navbar exactly as they are. Do not modify any other page.

Done looks like: a signed-in user sees only their own projects, and a second test account sees only its own.
```

---

## Base44-Specific Prompt Patterns

**Referencing a specific page or element (pair with Edit mode's "Edit Element"):**
> "[Element selected] Change this button's text to 'Get Started' and make it the primary accent color."

**Copying layout/content from an existing site:**
> "Copy the layout, sections, and visible content from https://example.com onto the homepage. Keep the same structure but use our own copy and colors."

**Direct database instructions (Base44's AI can act on data directly):**
> "Add three sample customers to the Customers table with realistic names and emails, so I can test the dashboard."

**Setting up access rules explicitly:**
> "Create a custom role called Event Organizer that can create and edit events, but cannot change app settings or see other organizers' events."

**Debugging with specifics:**
> "This form is showing a validation error even with valid input: [describe/paste]. Investigate and fix without modifying unrelated files."

**Publishing-ready check:**
> "Before we publish, check that no API keys or secrets are hardcoded anywhere in the app — everything sensitive should be in Secrets."

**Guardrail pattern (use in almost every prompt after the first):**
> "Only change [the specific thing]. Do not modify anything else."

---

## Inferring Access and Complexity

If the PRD doesn't specify these and the user hasn't said, ask before writing any step prompts:

> "Before I write the prompts, a couple of quick questions:"
> - **Who can access this app** — just you, your workspace/team, or the public?
> - **Are there different user roles** with different permissions (e.g., admin vs. regular user), or does everyone see the same thing?

Once confirmed, restate the access model in any prompt that touches a new page or feature involving user data — Base44 builds default access rules per step, so repeating this anchors it correctly each time.

## Common Recommendations

Use these when the user asks you to choose:

- **Mode for an uncertain step:** Discuss mode first (0.3 credits, no changes made) — cheaper and safer than a live prompt when the outcome isn't fully settled.
- **Mode for a purely visual tweak:** Edit mode — manual style changes cost nothing, and "Edit Element" costs less than a full prompt since the AI only looks at the one element.
- **AI model choice:** Leave it on Automatic unless the user has a specific, informed reason to pick manually — Base44 already routes each prompt to the right model, and manual selection can use more credits unpredictably.
- **When something breaks:** Try the automatic fix once or twice; if it doesn't resolve, stop and switch to Discuss mode to talk through the real cause rather than repeating the auto-fix.
- **Before any real publish:** Run a security scan, especially if the app has logins, payments, or personal data.
