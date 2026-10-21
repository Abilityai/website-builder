---
name: build-website
description: Build a website from a plain-language brief, or change one this agent already runs, end to end to a live URL — scaffold, private GitHub repo, Vercel deploy.
user-invocable: true
argument-hint: "<brief> | <site-name>: <change>"
allowed-tools: Read, Write, Edit, Bash, Glob, Grep, Skill, mcp__trinity__ask_operator, mcp__trinity__get_my_ask, mcp__trinity__withdraw_ask
metadata:
  version: "1.0"
  changelog:
    - "1.0: Initial version — thin front end over create-website (library first, plugin fallback), resume-first routing for a pending ask"
---

# Build Website

> ℹ️ **First, set expectations:** before anything else, print one short line with this skill's version and its most recent change — the top entry of `metadata.changelog` above — e.g. `build-website vX.Y — recent: <summary>`. Then proceed.

## Step 1: Pick the builder

Use the skills-library copy when it is assigned, because the platform keeps it on the latest released version:

```bash
ls ~/.claude/skills/create-website/SKILL.md 2>/dev/null && echo library || echo plugin
```

- `library` → the builder is `/create-website`
- `plugin` → the builder is `/create-agent:website` (installed from `template.yaml`; current as of this agent's creation)

Say which one you are using in your first line.

## Step 2: Resume, new site, or a change?

- **Resume first.** `ls ~/sites/*.pending.md 2>/dev/null`. If a pending file exists and the argument is `<name>: resume`, an answer to its question, or carries nothing new about another site → pass `<name>: resume` to the builder. It reads the pending file and `get_my_ask` and continues from where it stopped. Never start a second build of a site that is still pending.
- The argument starts with `<name>:` and `~/sites/<name>` exists → it is a **change**. Pass `<name>: <change>` to the builder; its headless rule 6 edits, rebuilds, pushes and redeploys.
- Otherwise it is a **new site**. Pass the whole brief to the builder unchanged. If a site with the derived name already exists, treat it as a change only if the brief clearly refers to it; otherwise the builder picks a new name.

## Step 3: Run the builder

Invoke the builder with the Skill tool and the argument from Step 2. It runs headless: it asks at most once through `ask_operator`, writes `~/sites/<name>.pending.md` and ends the turn while waiting. Tell the person the question is in their Inbox and that replying "go on" (or answering here) resumes the build.

## Step 4: Answer

Start with the live URL. Then: the GitHub repo, the pages built (or what changed), and every assumption or `TBD` the builder listed. If a key was missing, name it and say a person adds it in this agent's settings as a credential.
