---
name: betterprompt
description: BetterPrompt. Turns a rough request into a sharp, complete prompt, offers 2-3 rewrites to pick from (or new ones, or the user's own changes), then runs the chosen one. For a new project it first asks which stack to use. Use when the user types /betterprompt, asks to improve, rewrite or sharpen a prompt or request before Claude acts on it, or starts a new project from scratch.
---

# BetterPrompt

Brief the user's request the way you brief a subagent: rewrite it into a prompt that needs no follow-up questions, let them choose, then do it.

## 1. Find what they actually want

- The request is the text after `/betterprompt`. If empty, use their previous message; if there is none, ask what they want done.
- Read `~/.claude/betterprompt/preferences.md` if it exists: what this user chose over your recommendations before.
- Ground it before you write: a few cheap reads so the prompt names real paths, names and dates. Resolve "this", "it" and "the X" from the current folder and the conversation. When something they name isn't in the folder, look where the project keeps it (a plans folder, a sibling repo, its docs) before guessing. Don't start the task itself.
- Pin down the outcome behind their words: what done looks like, who uses the result, and what answer would disappoint them.
- List what you can't settle from context. Don't ask about it (except a new project's stack, step 2): let the options settle it differently.

## 2. New project: settle the stack

If the request starts a new project (an app, site, API or tool built from scratch), settle the stack before writing options. If the folder already has code, read the stack from its manifests, put it in every option's **Settled** part, and go to step 3.

Call AskUserQuestion once with up to 4 questions, one per layer, most important first: platform and codebase (web, mobile or desktop; language and framework), database, authentication, payments. Skip a layer the project doesn't need or the user already named; for an optional layer like payments, include a "None" choice. Each question gets a short header naming the layer and 2-3 choices that fit this project, with "(Recommended)" at the end of the first one's label: their past choice from preferences if it fits, otherwise your pick. Each description says why and what it costs. Prefer mainstream, well-documented, managed options with a free tier. "Other" lets them name their own.

Put the answers in every option's **Settled** part, along with your picks for layers you didn't ask about (hosting, email, file storage). Name those picks in step 4's "Added to your request" line so they can change them.

## 3. Write 2-3 options

Each option is a complete prompt a fresh session could run cold, under 200 words. Include each part that applies, nothing else:

- **Goal**: the outcome and who it's for, in one line. Add a role only when it changes behavior ("adversarial reviewer").
- **Inputs**: exact paths, URLs, names and dates; say which to read in full.
- **Settled**: decisions already made ("don't re-argue X").
- **Scope**: what to do and what to leave alone ("do not edit any file").
- **Criteria**: numbered checks for what good looks like.
- **If stuck**: what to do when an input is missing or a step fails. Report it; never guess or work from partial input.
- **Output**: format, structure, length cap.

Every option delivers what they asked for: if they said "fix it", every option fixes something, and a report-only option fits only a request to check or explain. Options differ in how (scope, depth, order such as measure first, or how an open question is settled), not in wording. If the request is already sharp, make option 1 nearly verbatim and say so in the "Added to your request" line.

## 4. Let them pick

Call AskUserQuestion once, one question, not multiSelect:

- question: "Which prompt should I run?" then a new line "Added to your request:" and what the options add, a few words each (e.g. "exact files, no edits, 300-word limit"). This teaches them to write it themselves next time.
- options: the rewrites, your pick first with "(Recommended)" at the end of its label. If preferences show they chose a kind of option over your pick before, recommend that kind. Label: 1-5 words naming the difference. Description: what that prompt does differently. Preview: the full prompt text. Last option: label "Different options", description "Show me new rewrites".
- The tool adds "Other" itself; that is where the user types changes. Don't add your own.

Then:

- **A rewrite** → apply any notes they added to it, then step 5.
- **Different options** → write 2-3 new rewrites unlike any shown so far and ask again.
- **Other** → apply their change to the option it's about (or to all), and ask again with the revised ones.

Without AskUserQuestion, print the question, then the options as numbered code blocks, and ask for a number, "more", or what to change.

## 5. Remember, then run it

Update `~/.claude/betterprompt/preferences.md` only when the user overrode you: they picked a choice or option you didn't recommend, or typed a change that would apply again ("always wants a word limit"). Accepting your recommendation says nothing about their taste, so then record nothing. Write one plain line per preference, such as "web apps: SvelteKit over Next.js" or "fix requests: narrow fix over measure-first". Keep earlier lines unless the new choice contradicts one, and stay under 20 lines. Store choices only, never request text, file contents, paths or secrets. The first time you create the file, tell them in one line where it is and that deleting it resets BetterPrompt. If they say not to remember, skip this.

Then treat the picked prompt as the user's new request and carry it out now, exactly as if they had typed it, so the project's own rules and skills still apply. If they said they only want the prompt, print it in one code block to copy and stop.
