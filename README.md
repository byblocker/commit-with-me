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

## Example handoff

Given "Add a command that stores credentials in the system keyring", the agent plans four commits and presents each one like this:

> ### Proposed commit
>
> `Add an example handoff to the README`
>
> **Summary**
>
> Adds an "Example handoff" section after Usage. It shows a filled-in proposal for the second commit of the keyring example from `SKILL.md`, so readers can see what a checkpoint looks like before they install the skill.
>
> **Validation steps**
>
> 1. Open `README.md` in a Markdown preview or on GitHub after pushing. **Expected:** "Example handoff" appears between Usage and License.
>
> **Files**
>
> - `README.md`
>
> **Checks**
>
> - **Not run:** Markdown lint. The repo has no linter or pre-commit config, and `markdownlint` isn't installed.
>
> **Known issues**
>
> The example omits Validation steps, which is how the skill treats a backend-only change. If you'd rather the example show every section, I can add them.
>
> **Diff**
>
> `git diff README.md`

## License

[MIT](LICENSE)
