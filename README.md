# BetterPrompt

A [Claude Code](https://claude.com/claude-code) skill that turns a rough request into a sharp, complete prompt before Claude acts on it.

Claude writes very good prompts for its own subagents: exact inputs, settled decisions, scope, criteria, what to do when something is missing, and an output format. Most people don't type prompts like that. BetterPrompt has Claude do it for you, then lets you choose.

## How it works

1. **Finds what you actually want.** Claude reads the files, folder or URL you mention so the prompt names real paths and names.
2. **Writes 2-3 rewrites.** Each one is a complete prompt, and they differ in a choice that matters (scope, depth, report vs. changes), not just wording.
3. **You pick.** Choose one, ask for different options, or type what to change.
4. **Claude runs it** as if you had typed the better prompt yourself.

## Install

For all your projects:

```bash
git clone https://github.com/fayofye/betterprompt ~/.claude/skills/betterprompt
```

For one project only, run this from the project root:

```bash
git clone https://github.com/fayofye/betterprompt .claude/skills/betterprompt
```

Start a new Claude Code session afterwards.

## Use

```
/betterprompt review the roadmap against last week's plan and tell me what's missing
```

Claude also uses it when you ask it to improve, rewrite or sharpen a prompt.

## Example

You type:

> check if the signup flow works

One of the options you get back:

> Test the signup flow of the app in this repo end to end and report what breaks.
>
> Inputs: the signup page at `app/signup/page.tsx` and its API route `app/api/signup/route.ts`. Read both in full.
> Scope: test only; do not edit any file.
> Criteria: 1. a valid signup creates an account and logs in; 2. duplicate email, weak password and empty fields show a clear error; 3. no errors in the server log.
> If stuck: if the dev server won't start, report the error instead of testing partially.
> Output: a pass/fail line per criterion, then each failure with steps to reproduce. Under 300 words.

## Update

```bash
git -C ~/.claude/skills/betterprompt pull
```

## License

MIT
