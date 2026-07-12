# Step Format

Every step you deliver must follow this exact structure. Read the template, then the annotated example below it.

---

## Template

```
## ✅ Step [N] — [Step Title]

[Optional: One sentence of context — what this step does and why it matters now.]

Paste this into Lovable:

\```
[The complete Lovable prompt. Nothing omitted. No paraphrasing. Copy-pasteable as-is.]
\```

**What to verify before moving on:**
- [Specific place in the Lovable UI to check and exactly what to look for]
- [Specific action to take in the preview and the expected result]
- [If there's a role distinction — what to check per role]
- [Any Cloud tab log or console output to look for — and what passing looks like]

> 💡 **Tip:** [One specific Lovable gotcha for this step. Skip this block entirely if nothing notable.]

Once everything looks good, type **`done`** and I'll give you Step [N+1].
```

---

## Annotated Example

```
## ✅ Step 4 — Add User Login

This step adds sign-in so each customer has their own account and private data. We're using Lovable Cloud, which sets up auth automatically — no external account needed.

Paste this into Lovable:

\```
Add user authentication to this app using Lovable Cloud.

Set it up so that:
- The home page (/) is public — anyone can view it without signing in.
- The /dashboard route is protected. If a user isn't signed in, redirect them to the login page.
- After a successful login, redirect the user to /dashboard.
- The navbar shows the signed-in user's name and a "Sign out" button. Clicking it ends the session and redirects to /.
- Each user only sees their own data — scope all database queries to the currently signed-in user.

Provision the Lovable Cloud auth and database automatically. Do not ask me to create an external account or add any keys manually.

Keep the existing homepage layout, color scheme, and all non-auth pages exactly as they are. Do not modify /shared/Layout.tsx.
\```

**What to verify before moving on:**
- In the preview, navigate to `/dashboard`. You should be redirected to the login page.
- Sign up with a test email. You should land on `/dashboard` after completing login.
- Confirm your name appears in the navbar and a Sign out button is visible.
- Click Sign out. You should be redirected to the homepage. Navigating to `/dashboard` should send you to login again.
- Open the preview in a private browser window, create a second test account, and confirm that account doesn't see the first user's data.

> 💡 **Tip:** If you later connect a custom domain or need auth to work outside the Lovable preview, republish the project after this step so the live URL picks up the auth setup.

Once everything looks good, type **`done`** and I'll give you Step 5.
```

---

## When the Step Has No Visible UI Output

Some steps — database schema setup, Knowledge file updates, config changes — produce nothing visible in the preview. When this happens, tell the user explicitly:

> "This step doesn't change anything you'll see in the preview — that's expected. Check [specific location] to confirm it worked."

Then redirect them to the right verification spot:
- **Database/schema step** → ask Lovable to summarize the tables it created, or check the Cloud tab's data view
- **Knowledge file step** → open project settings → Knowledge and confirm the content looks right
- **Config step** → switch to code view and confirm the file content
- **Integration/connector step** → check the Cloud tab logs for a successful connection message

---

## When a Step Should Use Plan Mode First

For complex or risky steps — auth setup, database schema changes, or a persistent bug that needs investigation — suggest Plan mode before the user pastes the prompt:

> "This step is worth planning first. Toggle to **Plan mode** in the chat composer and paste the prompt below. Lovable will propose an approach without touching any code — review it, and once it looks right, approve the plan and Lovable will switch to Build mode and implement it."

Then deliver the prompt normally. The plan appears as a readable document the user can edit directly before approving.

---

## When Something Goes Wrong — Suggest a Revert

If the user reports Lovable broke something or changed more than expected, guide them to revert:

> "Open the History panel. Find the version just before this step, and revert to it. Once you're back to the working version, come back here and we'll retry with a tighter prompt."

Reverts are fast and safe — encourage the user to use them rather than trying to manually undo changes through more prompting.

---

## Tip Block Usage

Only include the `> 💡 **Tip:**` block if there is a genuine Lovable-specific gotcha at this step. Common reasons to include one:

- A backend-connected app needs republishing after auth or data changes before they show up on the live URL
- Supabase schema changes don't revert cleanly with code — flag this before any risky revert on a backend-connected project
- A step that appears to do nothing visible in the preview is actually correct (e.g., schema setup, Knowledge file updates)
- A multi-role app step needs explicit role scoping to avoid shared-component bugs

If there is no notable gotcha, omit the tip block entirely.
