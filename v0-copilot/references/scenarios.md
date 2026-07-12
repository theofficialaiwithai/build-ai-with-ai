# Common v0 Build Scenarios

Playbooks for situations that regularly come up between steps. Use these as the right move to make — not scripts to read verbatim.

---

## Preview Is Blank, Errors, or Won't Load

**Signals:** Empty Preview pane, app was working and now shows nothing, or a visible runtime error.

**Move:** Check the Console panel first — it's the fastest path to the real cause.
> "Open the Console panel and check the Logs tab for the dev server's output, and the Terminal tab if you want to run something directly. Paste any red error text here."

If the cause isn't obvious from logs, ask v0 to investigate directly:
> "Ask v0: 'The preview isn't loading. Check the sandbox logs and browser console for the cause and fix it.'"

---

## v0 Didn't Complete Everything Asked

**Signal:** A multi-part prompt only got partially implemented.

**Move:** Don't re-ask for everything again. Break it down.
> "Let's do this in smaller pieces. First: [smallest remaining piece]. We'll verify that works, then move to the next part."

This also matches v0's own guidance — complex builds go better broken into steps than requested all at once.

---

## Deployment Fails or Shows Errors/Warnings

**Signal:** Publishing errors out, or the deployment popover shows warnings after publishing.

**Move:** Use v0's built-in fixer first — it's fast and free most of the time.
> "Click **Fix with v0** in the deployment popover. It'll diagnose the error and apply a fix automatically — you get up to 20 free uses a day on code you haven't manually edited."

If that doesn't resolve it:
> "Let's go back one version to the last working state, and we'll retry this step with a tighter prompt."

If the deployment is blocked entirely rather than erroring:
> "This might be a team Deployment Policy restricting where v0 can publish from. Check with whoever owns your Vercel team settings — they can adjust or override it for this project."

---

## Stuck in a Repeated Fix Loop

**Signal:** The user has re-prompted a fix 2-3+ times and the error persists or worsens.

**Move:** Stop re-prompting the same way.
> "Let's step back. Ask v0 directly: 'Investigate this error without changing anything else yet — I want to understand the root cause first.' [paste the exact error text]"

If that still doesn't land, restore the last working Version and re-approach the feature in smaller steps.

---

## Something Changed That Shouldn't Have

**Signal:** A small prompt resulted in unrelated pages, styles, or features changing.

**Move:** Restore first, then retry with a tighter prompt.
> "Use the version dropdown in the top-right, or scroll the chat and click Restore next to the version just before this change. Restoring creates a new version with the old code, so nothing is lost."

Rewrite the prompt to add:
> "Keep all existing pages, styles, and functionality exactly as they are. Only change [the specific thing]. Do not modify anything else."

---

## Database or Auth Isn't Working

**Signal:** Data isn't saving, sign-in fails, or a query errors out.

**Move:** First confirm the integration is actually connected — this is the most common cause.
> "Open Project Menu → Integrations and confirm Supabase (or whichever backend you're using) shows as connected. If it's not there, we need to connect it before this step will work."

If it's connected and still failing:
> "Ask v0: 'This [Supabase/Neon/Upstash] query is failing with this error: [paste]. Investigate and fix without modifying unrelated files.'"

---

## Sandbox Feels Stuck, Slow, or "Not Responding"

**Signal:** The sandbox seems frozen, or the Preview doesn't reflect the latest changes.

**Move:** Sandboxes stop when idle and resume automatically — a hard refresh is usually enough.
> "Try a hard refresh first (Cmd+Shift+R / Ctrl+Shift+R). Sandboxes go idle after inactivity and restart on their own when you come back — give it a few seconds after refreshing."

If a session has been open a very long time (approaching the 5-hour session cap), a fresh reload of the chat resolves it.

---

## User Asks Which Backend to Use

**Signal:** "Should I use Supabase or Neon?" / "What's the difference?"

**Move:** One direct recommendation, one sentence of reasoning.
> "Supabase, unless you only need a plain SQL database with no built-in auth — it's the only Marketplace option that bundles Postgres, authentication, and realtime together, so it covers almost everything most apps need."

---

## User Asks About Build Order

**Signal:** "Should I connect the database now or build the UI first?"

**Move:**
> "UI first with placeholder data, if you're newer to this — get the design and flow feeling right, then connect Supabase once the shape of the app is settled. It avoids debugging schema issues while you're still deciding what the app should look like."

---

## User Describes a Problem Vaguely

**Signal:** "Something's broken" / "It doesn't look right" / "This isn't working"

**Move:** Don't guess. Ask for a screenshot.
> "A screenshot would help me see exactly what's happening — or select the element with Design Mode and describe what should change."

Once you have it: read the Preview, any visible error, and the Console logs if shown. Respond based on what's actually visible.

---

## User Asks Where Something Is in the v0 UI

**Signal:** "Where's version history?" / "Where do I add a custom domain?" / "Where are the environment variables?"

**Move:** Give exact navigation, current as of 2026:

- **Versions:** Dropdown in the top-right of the chat, or scroll the chat and click Restore next to any earlier generation
- **Code editor:** "Code" tab next to Preview
- **Design Mode:** Cursor/design icon in the composer, or `Option+D` (Mac) / `Alt+D` (Windows/Linux)
- **Console (logs/terminal):** Console panel, tabs for Logs and Terminal
- **Integrations (databases, payments, AI models):** Project Menu `...` → Integrations, or the Connect panel in the chat sidebar
- **Environment variables:** Project Menu `...` → Environment Variables, or the Vars panel in the chat sidebar
- **Publish / custom domain:** Publish button, top-right; Publish → Customize Domain, or Project Menu → Domains
- **GitHub connection:** Project Menu `...` → GitHub
- **Permission mode (terminal autonomy):** Composer's Tools dropdown (plus icon) → Permissions

If the UI has moved (v0 updates often), say: "v0's interface updates regularly — if you don't see it where I described, a screenshot will help me guide you directly."

---

## Tone Guide

These rules apply throughout every build.

**When things break:** Start with the fix, not the diagnosis.
> ✅ "No worries — this is a common one. Here's what to do: [fix]"
> ❌ "It looks like the issue might be related to..."

**Celebrating progress:** Briefly acknowledge the completed step before delivering the next one.
> "🎉 Step 3 done — the dashboard is wired up. Here's Step 4."

**Never imply the user did something wrong.** Even if they clicked Fix with v0 several times or skipped a verification step — respond with the fix, not the judgment.

**Short sentences in instructions.** Especially UI actions.
> ✅ "Open the Code tab."
> ❌ "In order to see the underlying source, you'll want to switch over to the tab that shows your project's files."

**Jargon:** Define it the first time, inline.
> "Open Design Mode (a way to visually tweak elements in the live preview without writing a prompt)."

After the first use, you can use the term without explaining it again.

**Precision on v0's own terms matters.** A "Project" spans multiple chats and controls deployment; a single chat is not a "Project." Getting this wrong will confuse a user trying to follow along in the real UI.
