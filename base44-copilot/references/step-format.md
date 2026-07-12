# Step Format

Every step you deliver must follow this exact structure. Read the template, then the annotated example below it.

---

## Template

```
## ✅ Step [N] — [Step Title]

[Optional: One sentence of context — what this step does and why it matters now.]

Paste this into Base44:

\```
[The complete Base44 prompt. Nothing omitted. No paraphrasing. Copy-pasteable as-is.]
\```

**What to verify before moving on:**
- [Specific place in the Base44 UI to check and exactly what to look for]
- [Specific action to take in the Preview and the expected result]
- [If there's a role distinction — what to check per role, using Act as a user]
- [Any Data tab or access-rule check to look for — and what passing looks like]

> 💡 **Tip:** [One specific Base44 gotcha for this step. Skip this block entirely if nothing notable.]

Once everything looks good, type **`done`** and I'll give you Step [N+1].
```

---

## Annotated Example

```
## ✅ Step 4 — Add User Login

This step adds sign-in so each customer has their own account and private data. Base44 handles authentication automatically — there's no separate setup needed.

Paste this into Base44:

\```
Add user authentication to this app.

Set it up so that:
- The home page is public — anyone can view it without signing in.
- The Dashboard page is protected. If a user isn't signed in, redirect them to the login page.
- After a successful login, redirect the user to the Dashboard.
- The navbar shows the signed-in user's name and a "Sign out" button. Clicking it ends the session and returns to the home page.
- Each user should only ever see their own data — never another user's.

Keep the existing homepage layout, color scheme, and all non-auth pages exactly as they are. Do not modify any other pages.
\```

**What to verify before moving on:**
- In the Preview, navigate to the Dashboard. You should be redirected to the login page.
- Sign up with a test email. You should land on the Dashboard after completing login.
- Confirm your name appears in the navbar and a Sign out button is visible.
- Click Sign out. You should be redirected to the home page. Navigating to the Dashboard should send you to login again.
- Open the Data tab and check the access rules on any user-scoped entity — confirm they're limited to the signed-in user.
- Create a second test account in a private browser window and confirm it doesn't see the first account's data. You can also use Act as a user (More Actions menu, top bar) to check this without a second account.

> 💡 **Tip:** This is a good moment to run a security scan, since auth and per-user data are exactly what it checks for — worth doing now rather than waiting until right before publish.

Once everything looks good, type **`done`** and I'll give you Step 5.
```

---

## When the Step Has No Visible UI Output

Some steps — access rule changes, custom roles, Secrets/API key setup — produce nothing visible in the Preview. When this happens, tell the user explicitly:

> "This step doesn't change anything you'll see in the Preview — that's expected. Check [specific location] to confirm it worked."

Then redirect them to the right verification spot:
- **Access rules / entity step** → open the Data tab and review the entity's access rules directly
- **Custom role step** → open Settings → App Settings → roles, and confirm the role and its permissions appear as described
- **Secrets/API key step** → open the Secrets tab and confirm the key is present (never visible in plain text once saved — that's expected)
- **AI Controls step (Custom Instructions / Freeze Files)** → open the chat's Settings (gear icon) → AI Controls and confirm the setting is saved

---

## When a Step Should Use Discuss Mode First

For steps with real ambiguity — an unusual permissions model, a workflow with several edge cases, anything the user seems unsure about — suggest Discuss mode before the user pastes a live prompt:

> "This step is worth talking through first. Switch to Discuss mode and describe what you're trying to achieve — Base44 will help you think it through without touching your app or using much credit. Once it feels clear, switch back and we'll turn it into a real prompt."

Then deliver the actual build prompt normally once the approach is settled.

---

## When Something Goes Wrong — Suggest a Revert

If the user reports Base44 broke something or changed more than expected, guide them to revert rather than manually undoing or re-prompting:

> "Hover over the message in the chat that caused this and click the Revert icon. That rolls the app back to right before that change. Once you're back to a working version, come back here and we'll retry with a tighter prompt."

For anything more serious, point to Version History instead:
> "Open Version History at the top of the chat, find a version from before things went wrong, and either revert your editor to it or publish it directly if your live app is affected."

---

## Tip Block Usage

Only include the `> 💡 **Tip:**` block if there is a genuine Base44-specific gotcha at this step. Common reasons to include one:

- A step involving auth, payments, or personal data is a natural point to mention running a security scan
- A step that appears to do nothing visible in the Preview is actually correct (e.g., access rules, Secrets)
- The step is a good candidate for Freeze Files if it protects something that shouldn't change later
- A multi-role app step needs explicit role scoping to avoid one role seeing another's data

If there is no notable gotcha, omit the tip block entirely.
