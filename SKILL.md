---
name: betterprompt
description: BetterPrompt. Turns a rough request into a sharp, complete prompt, offers 2-3 rewrites to pick from (or new ones, or the user's own changes), then runs the chosen one. Use when the user types /betterprompt, or asks to improve, rewrite or sharpen a prompt or request before Claude acts on it.
---

# BetterPrompt

Brief the user's request the way you brief a subagent: rewrite it into a prompt that needs no follow-up questions, let them choose, then do it.

## 1. Find what they actually want

- The request is the text after `/betterprompt`. If empty, use their previous message; if there is none, ask what they want done.
- Ground it before you write: a few cheap reads (the files, folder, repo or URL they mention, and the conversation so far) so the prompt names real paths, names and dates. Don't start the task itself.
- Pin down the outcome behind their words: what done looks like, who uses the result, and what answer would disappoint them.
- List what you can't settle from context. Don't ask about it: let the options settle it differently.

## 2. Write 2-3 options

Each option is a complete prompt a fresh session could run cold. Include each part that applies, nothing else:

- **Goal**: the outcome and who it's for, in one line. Add a role only when it changes behavior ("adversarial reviewer").
- **Inputs**: exact paths, URLs, names and dates; say which to read in full.
- **Settled**: decisions already made ("don't re-argue X").
- **Scope**: what to do and what to leave alone ("do not edit any file").
- **Criteria**: numbered checks for what good looks like.
- **If stuck**: what to do when an input is missing or a step fails. Report it; never guess or grade partial input.
- **Output**: format, structure, length cap.

Options differ in a choice that changes the result (scope, depth, deliverable such as report vs. changes, or how an open question is settled), not in wording. Every option keeps the user's intent and adds no work they didn't ask for. If the request is already sharp, say so and make option 1 nearly verbatim.

## 3. Let them pick

Call AskUserQuestion once, one question, not multiSelect:

- question: "Which prompt should I run?"
- options: the rewrites, your pick first with "(Recommended)" at the end of its label. Label: 1-5 words naming the difference. Description: what that prompt does differently. Preview: the full prompt text. Last option: label "Different options", description "Show me new rewrites".
- The tool adds "Other" itself; that is where the user types changes. Don't add your own.

Then:

- **A rewrite** → apply any notes they added to it, then step 4.
- **Different options** → write 2-3 new rewrites unlike any shown so far and ask again.
- **Other** → apply their change to the option it's about (or to all), and ask again with the revised ones.

Without AskUserQuestion, print the options as numbered code blocks and ask for a number, "more", or what to change.

## 4. Run it

Treat the picked prompt as the user's new request and carry it out now, exactly as if they had typed it. If they said they only want the prompt, print it in one code block to copy and stop.
