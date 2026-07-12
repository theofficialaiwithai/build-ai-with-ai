# Step Format

Every step you deliver must follow this exact structure. Read the template, then the annotated example below it.

---

## Template

```
## ✅ Step [N] — [Step Title]

[Optional: One sentence of context — what this step does and why it matters now.]

Paste this into v0:

\```
[The complete v0 prompt. Nothing omitted. No paraphrasing. Copy-pasteable as-is.]
\```

**What to verify before moving on:**
- [Specific place in the v0 UI to check and exactly what to look for]
- [Specific action to take in the Preview and the expected result]
- [If there's a role distinction — what to check per role]
- [Any Console log or Integrations panel status to look for — and what passing looks like]

> 💡 **Tip:** [One specific v0 gotcha for this step. Skip this block entirely if nothing notable.]

Once everything looks good, type **`done`** and I'll give you Step [N+1].
```

---

## Annotated Example

```
## ✅ Step 4 — Add User Login

This step adds sign-in so each customer has their own account and private data. We're using the Supabase integration, which handles both the database and auth together.

Paste this into v0:

\```
Add user authentication to this app using the connected Supabase integration.

Set it up so that:
- The home page (/) is public — anyone can view it without signing in.
- The /dashboard route is protected. If a user isn't signed in, redirect them to the login page.
- After a successful login, redirect the user to /dashboard.
- The navbar shows the signed-in user's name and a "Sign out" button. Clicking it ends the session and redirects to /.
- Each user only sees their own data — scope all database queries to the currently signed-in user.

Use the Supabase integration that's already connected to this project. Do not assume any other backend exists, and don't ask me to create a new account manually.

Keep the existing homepage layout, color scheme, and all non-auth pages exactly as they are. Do not modify any other pages or components.
\```

**What to verify before moving on:**
- In the Preview, navigate to `/dashboard`. You should be redirected to the login page.
- Sign up with a test email. You should land on `/dashboard` after completing login.
- Confirm your name appears in the navbar and a Sign out button is visible.
- Click Sign out. You should be redirected to the homepage. Navigating to `/dashboard` should send you to login again.
- Open Project Menu → Integrations and confirm Supabase shows as connected, with the new test user visible in its Auth table.
- Open the Preview in a private browser window, create a second test account, and confirm that account doesn't see the first user's data.

> 💡 **Tip:** If you've already published this project, click Publish → Publish Changes after this step so the live URL picks up the auth setup — deploying doesn't happen automatically after each prompt.

Once everything looks good, type **`done`** and I'll give you Step 5.
```

---

## When the Step Has No Visible UI Output

Some steps — database schema setup, environment variable changes, config changes — produce nothing visible in the Preview. When this happens, tell the user explicitly:

> "This step doesn't change anything you'll see in the Preview — that's expected. Check [specific location] to confirm it worked."

Then redirect them to the right verification spot:
- **Database/schema step** → open Project Menu → Integrations → the connected provider's dashboard, or ask v0 to summarize the tables it created
- **Environment variable step** → open Project Menu → Environment Variables (or the Vars panel) and confirm the value is present
- **Config step** → switch to the Code tab and confirm the file content
- **New integration step** → check Project Menu → Integrations for a "Connected" status

---

## When a Step Should Lean on v0's Agentic Features First

For debugging steps where the cause isn't obvious, or for verifying a flow end-to-end, suggest letting v0 investigate before jumping to a fix prompt:

> "Before we fix anything, let's have v0 check for us. Paste: 'Open the app and test the [flow] end to end. Report any failures you find, but don't change any code yet.'"

Review what v0 reports before deciding on the actual fix prompt — this avoids fixing the wrong thing.

---

## When Something Goes Wrong — Suggest a Version Restore

If the user reports v0 broke something or changed more than expected, guide them to restore instead of manually undoing:

> "Use the version dropdown in the top-right, or scroll the chat and click Restore next to the version just before this step. Restoring creates a new version using the old code — nothing is lost, and version history stays linear. Once you're back to a working state, come back here and we'll retry with a tighter prompt."

---

## Tip Block Usage

Only include the `> 💡 **Tip:**` block if there is a genuine v0-specific gotcha at this step. Common reasons to include one:

- A published project needs "Publish Changes" clicked again before a backend or auth change shows up on the live URL
- A step that appears to do nothing visible in the Preview is actually correct (e.g., schema setup, environment variable changes)
- The step depends on a Marketplace integration that needs to be connected first, and it's worth double-checking before pasting the prompt
- A multi-role app step needs explicit role scoping to avoid shared-component bugs

If there is no notable gotcha, omit the tip block entirely.
