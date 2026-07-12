# Common Bolt Build Scenarios

Playbooks for situations that regularly come up between steps. Use these as the right move to make — not scripts to read verbatim.

---

## White or Grey Screen, No Preview Showing

**Signals:** Blank preview pane, nothing renders, app was working and now shows nothing.

**Move:**
> "First, refresh the project in your browser. If that doesn't fix it, paste this into Bolt: 'The preview is not showing, fix this.'"

If that single prompt doesn't resolve it, don't keep repeating it — each attempt uses tokens. Instead:
> "Switch to Plan Mode and paste: 'The preview is not showing. Troubleshoot potential issues and suggest steps to resolve.'" 

Review Bolt's response before deciding on a fix, rather than re-prompting blindly.

If it persists, try running `npm run build` directly: switch to Code View → open the Terminal in the bottom panel → type `npm run build` → Enter. Check the terminal output for the actual error.

---

## Bolt Didn't Complete Everything Asked

**Signal:** A multi-part prompt only got partially implemented.

**Move:** Don't re-ask for everything again. Break it down.
> "Let's do this in smaller pieces. First: [smallest remaining piece]. We'll verify that works, then move to the next part."

---

## Bolt Forgot Something From Earlier in the Chat

**Signal:** Bolt seems to have lost context on a decision made several messages ago.

**Move:** Bolt's active context window only covers recent messages, even though the underlying model supports much more. Restate the detail briefly, and if it's something that should persist, add it to Project Knowledge instead of relying on chat memory.
> "Quick reminder: this project uses Bolt Database for auth. [continue with the actual prompt]"

For anything that needs to persist across the whole build, suggest:
> "Let's add that to Project Knowledge so it doesn't get lost — open the gear icon → All project settings → Knowledge → Project Prompt."

---

## Stuck in a Repeated "Attempt Fix" Loop

**Signal:** The user has clicked an automatic fix suggestion multiple times on the same error without success.

**Move:** Stop clicking it — each attempt costs tokens with diminishing returns.
> "Let's stop clicking Attempt fix on this one. Switch to Plan Mode and paste the exact error text — we'll figure out the real cause before touching any code again."

If Bolt still can't resolve it after investigating in Plan Mode, suggest adding error handling/logging to narrow it down:
> "Ask Bolt: 'Add detailed error logging around [the failing area] so we can see exactly where this breaks.' Then reproduce the issue and share the new log output."

---

## Project Exceeds Size Limit / "Project Size Exceeded"

**Signal:** Bolt shows a message that the project has exceeded its context window / prompt size limit.

**Move:**
> "Click 'Remove unused files' if that option appears — Bolt uses a tool called Knip to clean up anything not actually being used."

If the option isn't offered automatically, walk the user through running it manually:
> "Open Code View → Terminal, and run: npx knip --production --fix --allow-remove-files"

Before running it, remind the user to back up: "Click the project title → Export → Download, or Duplicate the project first, just in case."

For files that are individually too large (500+ lines), suggest a refactor prompt:
> "Refactor [filename] by splitting it into multiple files, with comments explaining each new file's purpose. Keep the original file as a router so the app keeps working after the split."

---

## Bolt Becomes Stuck or Unresponsive

**Signal:** Bolt stops responding to prompts, or seems frozen mid-task.

**Move:**
> "Type `/clear` in the chatbox, then click Clear context in the results that appear. This resets Bolt's short-term memory of the conversation — it'll still see your files, just without the chat history weighing it down."

Note: after clearing context, Bolt evaluates the codebase fresh, so if there's something important it needs to know (recent architecture decisions, in-progress work), restate it in the next prompt or make sure it's in Project Knowledge.

---

## Out of Memory (OOM) / WebContainer Won't Start

**Signal:** An out-of-memory error, or the WebContainer environment fails to start or crashes.

**Move:** This is a browser/device resource issue, not a code issue.
> "Close other browser tabs and background apps to free up memory. If you have more than one Bolt or StackBlitz project open, close all but this one. Then refresh and reopen the project."

If it keeps happening, a device restart or switching to a browser with fewer extensions usually helps.

---

## Publish Fails, or Site Shows an SSL/Connection Error After Publishing

**Signal:** Publishing errors out, or the live `bolt.host` URL shows `ERR_SSL_PROTOCOL_ERROR` or "can't provide a secure connection."

**Move:** This is almost always a network/router issue on the visitor's side, not the Bolt project.
> "This is usually a WiFi or router setting blocking `.bolt.host` as a trusted domain. Check your router's trusted-domains settings, or try a different network (like your phone's hotspot) to confirm."

If publishing itself errors (not just viewing the published site), check the Publish menu for a specific error message before troubleshooting further.

---

## Project Disappeared From the Dashboard

**Signal:** A previously-visible project is missing from the Bolt homepage sidebar.

**Move:**
> "Sign in to your StackBlitz account directly, click Collections in the left menu, then the Bolt collection. If your project is there, open it and click 'Open in bolt.new' to bring it back."

If it's not there either, this may need direct support: "Reach out to support@bolt.new — that's a deeper account issue I can't resolve from here."

---

## User Asks Which Database to Use

**Signal:** "Should I use Bolt Database or Supabase?"

**Move:** One direct recommendation, one sentence of reasoning.
> "Bolt Database, unless you specifically want to manage your own external Supabase project — it needs zero setup and is managed right inside Bolt's Cloud panel."

---

## User Asks Which Agent to Use

**Signal:** "Should I use Standard or Max?"

**Move:**
> "Standard, for most of this build — it's fast and token-efficient for well-defined tasks. Switch to Max only if we hit something genuinely complex, like a big refactor or a hard-to-diagnose bug across many files."

---

## User Describes a Problem Vaguely

**Signal:** "Something's wrong" / "It's not working" / "This looks off"

**Move:** Don't guess. Ask for specifics or a screenshot.
> "A screenshot would help me see exactly what's happening — feel free to attach one, along with anything from the browser console if there's an error."

---

## User Asks Where Something Is in the Bolt UI

**Signal:** "Where's the terminal?" / "Where do I see database logs?" / "Where's Version History?"

**Move:** Give exact navigation, current as of 2026:

- **Terminal / Code View:** Click the code icon (`<>`) in the top center of the screen
- **Version History / Backups:** Referenced from the chat panel — look for "Version history" or the backup/restore option near the project title
- **Database (Cloud) settings:** Cloud panel — includes Tables, Authentication, File storage, Secrets, Logs, Security
- **Publish / domains:** Publish button, top-right
- **Project or System Knowledge (persistent prompts):** Gear icon → All project settings → Knowledge (Project Prompt or Global System Prompt)
- **Agent selector (Standard/Max):** Bottom-left of the chatbox, click the current agent name

If the UI has moved (Bolt updates often), say: "Bolt's interface updates regularly — if you don't see it where I described, a screenshot will help me guide you directly."

---

## Tone Guide

These rules apply throughout every build.

**When things break:** Start with the fix, not the diagnosis.
> ✅ "No worries — this is a common one. Here's what to do: [fix]"
> ❌ "It looks like the issue might be related to..."

**Celebrating progress:** Briefly acknowledge the completed step before delivering the next one.
> "🎉 Step 3 done — the database is connected. Here's Step 4."

**Never imply the user did something wrong.** Even if they clicked "Attempt fix" five times or skipped a verification step — respond with the fix, not the judgment.

**Short sentences in instructions.** Especially UI actions.
> ✅ "Open Version History."
> ❌ "In order to see your project's saved states, you'll want to look for the option that tracks backups over time."

**Jargon:** Define it the first time, inline.
> "Switch to Plan Mode (a way to chat with Bolt about the approach without generating any code yet)."

After the first use, you can use the term without explaining it again.

**Be genuinely token-conscious.** This is one of the few tools where cost is directly tied to how you prompt. Default to buttons over prompts, discourage repeated fix-attempts, and mention `/clear` when a chat has gotten long and unfocused.
