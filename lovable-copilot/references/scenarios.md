# Common Lovable Build Scenarios

Playbooks for situations that regularly come up between steps. Use these as the right move to make — not scripts to read verbatim.

---

## Preview Is Blank or White Screen

**Signals:** Empty white preview pane, app was working and now shows nothing.

**Move:** This is almost always a build error, a `vite.config.ts` issue, or security headers that got introduced.
> "Ask Lovable to investigate: 'The preview is showing a blank white screen. Please check vite.config.ts and the browser console for errors, and fix the cause.'"

If that doesn't resolve it: have the user open the browser console (right-click → Inspect → Console tab) and paste any red error text into the chat.

---

## Preview Not Found / Sandbox Spinning Up Forever

**Signals:** "Preview not found," a sandbox that never finishes loading, or a stuck spinner.

**Move:** Usually temporary.
> "Try a hard refresh first (Cmd+Shift+R / Ctrl+Shift+R). If it's still stuck after that, it may be a sync issue with GitHub if this project is connected — otherwise give it another minute and refresh again."

---

## Lovable Changed More Than You Asked For

**Signal:** A small prompt resulted in unrelated pages, styles, or features changing.

**Move:** Revert first, then retry with a tighter prompt.
> "Open the history panel and revert to the version just before this change. Then we'll retry with explicit guardrails."

Rewrite the prompt to add:
> "Keep all existing pages, styles, and functionality exactly as they are. Only change [the specific thing]. Do not modify anything else."

---

## Stuck in an Error Loop After Multiple "Fix" Attempts

**Signal:** The user has re-prompted a fix 2-3+ times and the error persists or gets worse.

**Move:** Stop re-prompting the same way. Switch to Plan mode.
> "Let's step out of Build mode for a second. Switch to Plan mode and paste this: 'Please investigate this error without changing anything yet. If needed, plan a revert to the last working version and fix from there.' [paste the exact error text]"

If Plan mode's proposed fix still doesn't land after one attempt, suggest reverting to the last known-good version and re-approaching the feature in smaller steps.

---

## Supabase-Connected Project Breaks After a Revert

**Signal:** The user reverted to an earlier version and now the app errors out around data, or the database schema doesn't match what the reverted code expects.

**Move:** This is a known asymmetry — Supabase schema changes don't revert cleanly with code versions.
> "Reverting code doesn't revert your Supabase database schema. Ask Lovable: 'Please validate the SQL schema at this version and ensure there are no breaking changes between the code and the current database schema.'"

If the schema is genuinely out of sync, this may require a manual SQL fix — treat it as a case to slow down on rather than push through with more reverts.

---

## Edge Function Errors

**Signal:** A backend function isn't working, an integration silently fails, or an API call from the app errors out.

**Move:**
> "Open the Cloud tab in the Lovable editor and check Logs for the specific error message. Then paste it into the chat: 'This edge function is failing with this error: [paste]. Please investigate and fix.'"

Also worth checking: that all required secrets/environment variables are set. A small, unrelated code change to the edge function (even a comment) can trigger a redeploy if it seems stuck.

---

## Project Feels "Too Far Gone" — Considering Starting Over

**Signal:** Many failed fix attempts, tangled features, the user wants to start clean but keep what they learned.

**Move:** Don't rebuild from zero in the same project. Use Remix.
> "Let's Remix this project — it clones the current project into a fresh copy, so the original with its full history stays intact as a reference. We'll rebuild the messy part with tighter prompts, using the original as a reference for anything that's already working well."

Note: Remixing requires disconnecting Supabase first if the project is connected to an external Supabase instance.

---

## User Asks Which Backend to Use

**Signal:** "Should I use Lovable Cloud or Supabase?"

**Move:** One direct recommendation, one sentence of reasoning.
> "Use Lovable Cloud unless you specifically want to manage your own Supabase project outside Lovable — Cloud needs zero external setup and covers auth, database, storage, and edge functions out of the box."

---

## User Asks About Build Order (Frontend-First vs Back-to-Front)

**Signal:** "Should I connect the backend now or build the UI first?"

**Move:**
> "Frontend-first, if you're newer to this — build the UI and flows with mock data, then connect Lovable Cloud once the design feels right. It avoids early database errors and keeps you iterating fast. Back-to-front only makes sense if you're comfortable debugging schema issues as you go."

---

## User Describes a Problem Vaguely

**Signal:** "Something's broken" / "It doesn't look right" / "This isn't working"

**Move:** Don't guess. Ask for a screenshot immediately.
> "A screenshot would help me see exactly what's happening — feel free to attach one, or select the element with the Edit tool and describe what should change."

Once you have it: read the preview, any visible error banner, and the browser console if shown. Respond based on what's actually visible.

---

## User Asks Where Something Is in the Lovable UI

**Signal:** "Where's version history?" / "Where do I add a custom domain?" / "Where are edge function logs?"

**Move:** Give exact navigation, current as of 2026:

- **Version/edit history:** History panel in the project — works like Google Docs, revert to or preview any past version
- **Knowledge file:** Project settings → Knowledge
- **Plan vs Build mode toggle:** In the chat composer, next to the message input
- **Cloud logs (edge functions, auth, storage):** Cloud tab in the editor
- **Publish / custom domain:** Publish button, top-right of the editor
- **GitHub connection:** Project settings → GitHub

If the UI has moved (Lovable updates often), say: "Lovable's interface updates regularly — if you don't see it where I described, a screenshot will help me guide you directly."

---

## Tone Guide

These rules apply throughout every build.

**When things break:** Start with the fix, not the diagnosis.
> ✅ "No worries — this is a common one. Here's what to do: [fix]"
> ❌ "It looks like the issue might be related to..."

**Celebrating progress:** Briefly acknowledge the completed step before delivering the next one.
> "🎉 Step 3 done — the dashboard is wired up. Here's Step 4."

**Never imply the user did something wrong.** Even if they skipped verification or pasted a prompt in the wrong mode — respond with the fix, not the judgment.

**Short sentences in instructions.** Especially UI actions.
> ✅ "Open the History panel."
> ❌ "In order to access your version history, you'll want to find the panel that tracks changes over time and click into it."

**Jargon:** Define it the first time, inline.
> "Switch to Plan mode (a mode where Lovable proposes an approach without changing any code yet)."

After the first use, you can use the term without explaining it again.

**Real content over placeholders, always** — this applies to your own prompts too, not just Lovable's output. Never write example copy as "Lorem ipsum" or "Feature 1, 2, 3" when giving the user a prompt to paste.
