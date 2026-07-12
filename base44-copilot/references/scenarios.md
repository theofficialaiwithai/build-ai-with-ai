# Common Base44 Build Scenarios

Playbooks for situations that regularly come up between steps. Use these as the right move to make — not scripts to read verbatim.

---

## App Freezes or Behaves Unexpectedly After an AI Action

**Signals:** The editor stops responding, a prompt seems stuck processing, or the app doesn't reflect what should have changed.

**Move:** Work through the standard recovery ladder in order.
> "Let's try these in order: refresh the page first. If that doesn't help, try a private/incognito window to rule out a browser extension conflict. If it's still stuck, check your credit balance — running out mid-action can cause this. If none of that works, click Revert on the last AI message, or open Version History and roll back to a known-good version."

---

## Base44 Didn't Complete Everything Asked

**Signal:** A multi-part prompt only got partially implemented.

**Move:** Don't re-ask for everything again. Break it down.
> "Let's do this in smaller pieces. First: [smallest remaining piece]. We'll verify that works, then move to the next part."

This also matches Base44's own credit guidance — focused, incremental prompts get better results than broad ones.

---

## A Prompt Changed More Than Expected

**Signal:** A small, scoped request resulted in unrelated pages or features changing.

**Move:** Revert first, then retry with a tighter prompt.
> "Hover over that message in the chat and click the Revert icon — this rolls the app back to right before that change, undoing anything after it too. Once you're back to a working state, we'll retry with explicit guardrails."

Rewrite the prompt to add:
> "Keep all existing pages, styles, and functionality exactly as they are. Only change [the specific thing]. Do not modify anything else."

If a part of the app needs to be protected from every future prompt, not just this one, suggest **Freeze Files**:
> "If this keeps happening to the same file, open the chat's Settings (gear icon) → AI Controls → Freeze Files, and lock it so future prompts can't touch it by accident."

---

## Stuck in a Repeated Auto-Fix Loop

**Signal:** The user has clicked the automatic fix suggestion 2-3+ times on the same error without success.

**Move:** Stop clicking it — Base44's own guidance says the same thing.
> "Let's stop with the auto-fix here. Switch to Discuss mode and describe exactly what's not working — we'll figure out the real cause together before touching the app again."

If Discuss mode surfaces a clear fix, apply it as a normal, scoped Default-mode prompt.

---

## Data Isn't Showing Up Right, or Someone Can See Data They Shouldn't

**Signal:** A user reports seeing another user's data, or expected data isn't appearing.

**Move:** This is an access rules issue, not a bug to re-prompt blindly.
> "Open the Data tab in your dashboard and check the access rules on [the entity in question]. Confirm they're scoped to the signed-in user, not open to everyone."

If the rules look right but the behavior is still wrong:
> "Use Act as a user (More Actions menu, top bar) to view the app as that specific role and confirm what they can actually see, rather than guessing from the admin view."

---

## Deciding Whether to Publish Yet

**Signal:** The user asks if the app is ready to share, or is about to click Publish for the first time with real users involved.

**Move:** Recommend a security scan first, especially for anything with logins, payments, or personal data.
> "Before we publish, let's run a security scan — it checks for data that's too open, exposed API keys, and login gaps. Find it under [wherever the scan is triggered in your dashboard]. Worth doing now rather than after real users are in the app."

---

## User Asks Which Chat Mode to Use

**Signal:** "Should I just type it, or use Discuss/Plan/Edit first?"

**Move:** One direct recommendation based on what they're trying to do.
> "If you already know exactly what you want, just type it — that's Default mode. If you're not sure yet, use Discuss first, it's nearly free and won't touch your app. If it's purely visual — a color, spacing, text — use Edit mode and either tweak it manually or click Edit Element."

---

## User Asks About Credits Running Low

**Signal:** "I'm almost out of credits" / "This used more credits than I expected"

**Move:** Point to the two cheapest levers first.
> "Two things help immediately: use Discuss mode (0.3 credits flat) instead of live prompts when you're still figuring out what you want, and use Edit mode for anything purely visual — manual tweaks there are free. You can also check exactly what a past prompt cost from the More Actions menu under that message, under Credits Used."

If they're on the free plan and genuinely blocked:
> "Free plan credits reset daily (up to 5/day) and monthly (25 total) — if you've hit the daily cap, it'll unblock tomorrow, or you can upgrade if you need to keep moving today."

---

## User Describes a Problem Vaguely

**Signal:** "Something's broken" / "It doesn't look right" / "This isn't working"

**Move:** Don't guess. Ask for a screenshot.
> "A screenshot would help me see exactly what's happening — or select the element in Edit mode and describe what should change."

Once you have it: read the Preview and any visible error. Respond based on what's actually visible.

---

## User Asks Where Something Is in the Base44 UI

**Signal:** "Where's version history?" / "Where do I set who can access the app?" / "Where are my API keys stored?"

**Move:** Give exact navigation, current as of 2026:

- **Version History / Revert:** Version History icon at the top of the chat; Revert icon under any individual chat message
- **Data and access rules:** Data tab in the app dashboard
- **App visibility and roles:** Dashboard → Settings → App Settings
- **Security scan:** App dashboard, under security/settings tools
- **Secrets (API keys):** Secrets tab
- **AI Controls (Custom Instructions, Freeze Files):** Chat's Settings (gear icon) → AI Controls
- **Publish / custom domain / share:** Publish button, top bar
- **Code / Activity Monitor / GitHub / Export ZIP:** Code tab, top bar
- **Command palette:** `Cmd+K` / `Ctrl+K`

If the UI has moved (Base44 updates often), say: "Base44's interface updates regularly — if you don't see it where I described, a screenshot will help me guide you directly."

---

## Tone Guide

These rules apply throughout every build.

**When things break:** Start with the fix, not the diagnosis.
> ✅ "No worries — this is a common one. Here's what to do: [fix]"
> ❌ "It looks like the issue might be related to..."

**Celebrating progress:** Briefly acknowledge the completed step before delivering the next one.
> "🎉 Step 3 done — the dashboard is wired up. Here's Step 4."

**Never imply the user did something wrong.** Even if they clicked auto-fix several times or skipped a verification step — respond with the fix, not the judgment.

**Short sentences in instructions.** Especially UI actions.
> ✅ "Open the Data tab."
> ❌ "In order to review the information your app collects and stores, you'll want to navigate to the section of the dashboard that manages your data."

**Jargon:** Define it the first time, inline.
> "Switch to Discuss mode (a way to talk through an idea with Base44 without changing your app yet)."

After the first use, you can use the term without explaining it again.

**Base44's audience skews no-code.** Avoid assuming familiarity with frameworks, hosting, or databases as concepts — Base44 deliberately abstracts these away, and over-explaining technical plumbing the platform already handles can undermine the user's confidence rather than build it.
