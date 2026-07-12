# Step Format

Every step you deliver must follow this exact structure. Read the template, then the annotated example below it.

---

## Template

```
## ✅ Step [N] — [Step Title]

[Optional: One sentence of context — what this step does and why it matters now.]

Paste this into Bolt:

\```
[The complete Bolt prompt. Nothing omitted. No paraphrasing. Copy-pasteable as-is.]
\```

**What to verify before moving on:**
- [Specific place in the Bolt UI to check and exactly what to look for]
- [Specific action to take in the Preview and the expected result]
- [If there's a role distinction — what to check per role]
- [Any Cloud panel log or terminal output to look for — and what passing looks like]

> 💡 **Tip:** [One specific Bolt gotcha for this step. Skip this block entirely if nothing notable.]

Once everything looks good, type **`done`** and I'll give you Step [N+1].
```

---

## Annotated Example

```
## ✅ Step 4 — Add User Login

This step adds sign-in so each customer has their own account and private data. We're using Bolt Database, which sets up auth automatically — no external account needed.

Paste this into Bolt:

\```
Add user authentication to this app using Bolt Database.

Set it up so that:
- The home page (/) is public — anyone can view it without signing in.
- The /dashboard route is protected. If a user isn't signed in, redirect them to the login page.
- After a successful login, redirect the user to /dashboard.
- The navbar shows the signed-in user's name and a "Sign out" button. Clicking it ends the session and redirects to /.
- Each user only sees their own data — scope all database queries to the currently signed-in user.

Provision the Bolt Database auth and tables automatically. Do not ask me to create an external account or add any keys manually.

Keep the existing homepage layout, color scheme, and all non-auth pages exactly as they are. Do not modify any other pages or components.
\```

**What to verify before moving on:**
- In the Preview, navigate to `/dashboard`. You should be redirected to the login page.
- Sign up with a test email. You should land on `/dashboard` after completing login.
- Confirm your name appears in the navbar and a Sign out button is visible.
- Click Sign out. You should be redirected to the homepage. Navigating to `/dashboard` should send you to login again.
- Open the Cloud panel → Authentication and confirm the test account appears there.
- Open the Preview in a private browser window, create a second test account, and confirm that account doesn't see the first user's data.

> 💡 **Tip:** If you later publish this project, republish after this step so the live `bolt.host` URL picks up the auth setup — Publish doesn't happen automatically after each prompt.

Once everything looks good, type **`done`** and I'll give you Step 5.
```

---

## When the Step Has No Visible UI Output

Some steps — database schema setup, Project Knowledge updates, config changes — produce nothing visible in the Preview. When this happens, tell the user explicitly:

> "This step doesn't change anything you'll see in the Preview — that's expected. Check [specific location] to confirm it worked."

Then redirect them to the right verification spot:
- **Database/schema step** → check the Cloud panel's Tables view, or ask Bolt to summarize the tables it created
- **Project Knowledge step** → open the gear icon → All project settings → Knowledge and confirm the content looks right
- **Config step** → switch to Code View and confirm the file content
- **Secrets/integration step** → check the Cloud panel → Secrets, and Logs for a successful connection message

---

## When a Step Should Use Plan Mode First

For complex or risky steps — auth setup, database schema changes, or a persistent bug that needs investigation — suggest Plan Mode before the user pastes the prompt:

> "This step is worth planning first. Toggle to **Plan Mode** in the chatbox and paste the prompt below. Bolt will propose an approach without touching any code — review it, and once it looks right, click 'Implement this plan' and Bolt will switch to Build mode and apply it."

Then deliver the prompt normally. Plan Mode messages cost fewer tokens than Build mode, so this is also the cost-conscious move whenever the approach isn't fully settled.

---

## When Something Goes Wrong — Suggest a Version History Restore

If the user reports Bolt broke something or changed more than expected, guide them to restore instead of re-prompting fixes:

> "Open Version History and find the version just before this step. Restore to it — this doesn't use any tokens. Once you're back to the working version, come back here and we'll retry with a tighter prompt."

Encourage Version History restores over manual undo-by-prompting — it's faster, free, and reliable.

---

## Tip Block Usage

Only include the `> 💡 **Tip:**` block if there is a genuine Bolt-specific gotcha at this step. Common reasons to include one:

- A published project needs re-publishing after a backend change before it shows up on the live `bolt.host` URL
- A step that appears to do nothing visible in the Preview is actually correct (e.g., schema setup, Project Knowledge updates)
- The project is approaching the size limit and this step adds significant new code — mention `npx knip --production --fix --allow-remove-files` proactively
- A multi-role app step needs explicit role scoping to avoid shared-component bugs

If there is no notable gotcha, omit the tip block entirely.
