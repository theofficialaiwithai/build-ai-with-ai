# Writing Lovable Prompts

Use this when the PRD doesn't include Lovable prompts and you need to write them yourself. These principles are grounded in Lovable's own prompting guidance and how the product actually behaves as of 2026.

---

## How Lovable Works (What You're Prompting Into)

Lovable is a full-stack AI development platform, not a simple code generator. When you send it a prompt in Build mode, it:
1. Understands your intent and explores the existing codebase for context
2. Applies changes directly across files — frontend, backend, and configuration together if needed
3. Resolves issues that appear while it works
4. Surfaces the result as file diffs and a live preview

Lovable works best when it's told **what to build, in real terms, one piece at a time** — not a full spec for the whole app in one shot.

---

## The Five Things Every Prompt Should Include

**1. The specific task — scoped to one component or feature**
Not "build the app" — "add a hero section with a headline, subtext, and CTA button." Lovable prompts by component, not by page. A full-page prompt gets noise; a section-based prompt gets a usable result you can review before moving on.

> "Create a floating menu bar with glassmorphism effect. Include Home, Search, Favorites, and Settings icons. Add gentle floating animation and smooth hover interactions."

**2. Real content, never placeholder text**
Lovable is trained to respond to structure and intent — placeholder copy like "lorem ipsum" or "Feature 1" hides real design decisions (a real headline might need two lines instead of one). Always write the actual copy, even in early steps.

> "Hero section with headline: 'Track every workout, automatically.' Subtext: 'Connect your wearable and let the data build your plan.' CTA: 'Start free.'"

**3. The visual direction — in buzzwords, not just layout**
Layout alone isn't enough; Lovable needs to know the vibe. Terms like "minimal," "expressive," "premium," "playful," and "developer-focused" are promptable parameters that shape typography, spacing, and color, not just decoration. State this once early (ideally in the Knowledge file) and repeat it across prompts for consistency.

> "Use a calm, wellness-inspired design. Soft gradients, muted earth tones, round corners, generous padding."

**4. Guardrails — what should NOT change**
Tell Lovable explicitly what to leave alone. This prevents collateral edits to working parts of the app.

> "Add this feature to `/dashboard`. Do not modify `/shared/Layout.tsx` or the existing authentication logic."

**5. Role scoping — for any app with more than one user type**
If the app has multiple roles (Admin, Investor, Customer, etc.), state which role a prompt applies to every time. This is the single most common source of Lovable bugs in multi-role apps — shared components silently leaking behavior across roles.

> "As an Investor, I want to view the company dashboard, but I shouldn't be able to edit it. Isolate this feature to the Investor role only."

---

## Prompt Length and Scope

Break big features into smaller, testable chunks rather than writing one long prompt covering many things at once. A reasonable pattern for a multi-part feature:

1. Create the page/route
2. Add the UI layout
3. Connect the data
4. Add logic and edge cases
5. Test per role (if multi-role)

Rule of thumb: if a prompt would require more than 2-3 "and also..." clauses, it's actually multiple steps.

---

## When to Use Plan Mode

Suggest switching to Plan mode before pasting a prompt when the step is one of the following:

- Sets up or changes authentication
- Touches the database schema
- Is a debugging session for a persistent, unclear bug
- Involves comparing two different implementation approaches

In Plan mode, Lovable proposes a plan (visible as a readable document) without touching code. The user can edit it directly or ask follow-up questions before approving. Approving switches Lovable to Build mode and it implements exactly what was approved.

---

## Example: Weak vs. Strong

**Weak:**
```
Add a dashboard.
```

**Strong:**
```
Build the main dashboard page at /dashboard.

This project uses Lovable Cloud for auth and data.

The page should be protected — if a user isn't logged in, redirect to the login page.

The dashboard should show:
- A welcome message: "Welcome back, [user's first name]"
- A grid of project cards. Each card shows: project name (bold), a short description (max 2 lines, truncated with ellipsis), and a "View" button linking to /projects/[id].
- An "Add Project" button in the top-right, linking to /projects/new.

Fetch only the projects belonging to the currently signed-in user. Show skeleton placeholder cards while loading.

Styling: minimal and clean. White cards, rounded corners, subtle shadow. The Add Project button should be the app's primary accent color.

Keep the existing navbar and footer exactly as they are. Do not modify /shared/Layout.tsx.

Done looks like: a logged-in user sees only their own projects on the dashboard, and a second test account sees only its own.
```

---

## Lovable-Specific Prompt Patterns

**Backend setup (Lovable Cloud — default recommendation):**
> "Add user authentication and a database using Lovable Cloud. No external account needed — provision it automatically."

**Backend setup (Supabase, if the user wants their own instance):**
> "Connect this project to Supabase for auth and database." (Requires the user to have already connected their Supabase account via the Lovable integration panel first.)

**Asking Lovable to plan before building:**
> "Ask me any questions you need in order to fully understand what I want from this feature and how I envision it." (Best paired with Plan mode.)

**Editing an existing element precisely:**
> "Change the CTA button text to 'Get Started' and increase the padding to 24px horizontal. Keep the existing background color and font."

**Auth-aware UI:**
> "If the user is logged in, show their profile image and name in the top right. If not, show a 'Log In' button that routes to the auth screen."

**Debugging (pairs with Plan mode):**
> "Take a step back. Analyze this error and suggest a different approach before changing anything: [paste error]"

**Generating a Knowledge file from what's already built:**
> "Generate knowledge for my project at T=0 based on the features I've already implemented." (Run this in Plan mode early in a build, or after a significant milestone.)

---

## Inferring the Build Style and Backend

If the PRD doesn't specify these and the user hasn't said, ask before writing any step prompts:

> "Before I write the prompts, two quick questions:"
> - **Build order:** design the UI first with mock data, then connect a backend once it's stable (recommended if you're newer to this) — or connect the backend from day one?
> - **Backend:** Lovable Cloud (built in, no external account) or your own Supabase project?

Once confirmed, keep the choice consistent across all step prompts — restate it in prompts that touch the backend, since Lovable can lose context across a long chat history.

## Common Recommendations

Use these when the user asks you to choose:

- **Backend:** Lovable Cloud for almost everyone — zero external setup. Supabase only if the user specifically wants to own/manage their own Supabase project outside Lovable.
- **Build order:** Frontend-first for beginners — avoids SQL errors early and keeps iteration fast. Back-to-front only for users already comfortable debugging database issues as they go.
- **When stuck in an error loop:** Don't keep re-prompting the same fix. Switch to Plan mode, paste the actual error text (from the browser console if needed), and ask Lovable to investigate before changing anything.
