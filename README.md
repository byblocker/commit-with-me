# commit-with-me

**Distill complex requests into concise commits**

## Install

```bash
npx skills add byblocker/commit-with-me
```

## What it does

A skill to split requested work into small, independently reviewable commits. No "oneshots". 

Runs required formatting, lint, build, and checks before each commit "handoff", which includes a suggested commit subject.

## Why?

Agents often work at a request-level granularity, regardless of complexity. This behavior suffices for small, simple requests (e.g., bug fixes with known causes, renames, and small refactors), but larger, more ambiguous requests result in a diff, albeit functional, that is too difficult to read and validate. Over time, rubber-stamping these large diffs distances developers from their own codebase.

To keep you in the loop, `commit-with-me` instructs agents to iterate in commits small enough to review at a glance. Each change is left **unstaged** for you to review, edit, and commit. The agent only continues after you commit.

## Usage

Asking for a code change in a Git repository triggers the skill. To invoke it directly:

| Agent | Command |
| --- | --- |
| Claude Code | `/commit-with-me` |
| OpenAI Codex | `$commit-with-me` |

While a change is pending, you may ask for further revisions or edit it yourself. Your edits are authoritative, and the agent never reverts them.

Not every request requires this granularity. Say "Add this to the current commit" to skip a single checkpoint, or "Pause `commit-with-me` until I ask to resume" to work without checkpoints. 

Regardless, the agent never commits for you, and every change remains in your working tree until you commit.

## License

[MIT](LICENSE)
