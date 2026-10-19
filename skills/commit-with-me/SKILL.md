---
name: commit-with-me
description: Implement Git changes in small commits for human review. Use for code changes or commit preparation, not read-only or planning tasks. Leave proposals unstaged and wait for the user to commit.
---

# Commit With Me

Implement and validate one small commit at a time. Leave changes unstaged for the user to review, edit, stage, and commit. Inspect their commit before starting the next.

Explain this workflow briefly the first time it applies in a conversation.

## Before starting

1. Check Git state with `git status` and `git log --oneline -5`. If the directory is not a Git repository, say so and ask how to proceed.
2. If a merge, rebase, cherry-pick, or bisect is in progress, or HEAD is detached, report it. Pause if it blocks the task, unless the user already authorized resolving it.
3. Treat existing changes as the user's. Do not modify them. Include them in a proposal only with explicit permission. If your edits would overlap them, ask first.
4. Read repository instructions, such as `CLAUDE.md`, `AGENTS.md`, or `CONTRIBUTING.md`, for commit conventions and required checks.
5. Confirm scope from the request and any plan the user provides.

## Choose small commit boundaries

Divide the request into tasks: features, fixes, or other units of work, each named in a few words. Break each task into the smallest commits that each make one coherent change and leave the build and existing behavior working. When unsure whether to split, split.

Before editing, show the commit plan (see **Track progress**), then begin the first commit without waiting for approval.

Separate:

- A refactor with no behavior change, from the change it prepares for
- A move or rename, from edits to the moved code
- A new or upgraded dependency, from the code that first uses it
- A schema, migration, or configuration change, from the code that reads it
- A new building block (type, module, component), from the code that wires it in
- Each user-facing entry point (command, endpoint, flag, page)
- A bug fix found along the way, from the task in progress
- User documentation, from the code it describes

Keep together:

- Code and the tests for it
- A change and formatting or small cleanup in lines it already touches
- A dependency manifest and its lockfile

If the subject cannot name the change without "and" or a list, split the commit. A shared feature or motivation, one commit depending on another, and an unfinished feature are not reasons to combine.

Combine two commits only when the first alone would break the build, would fail required checks, would be undone by standard tooling, or would change behavior outside the intended change. Line count does not decide.

For example, the task "Add a command that stores credentials in the system keyring" is four commits:

1. Add the keyring access dependency, if needed
2. Store credentials in the system keyring
3. Add the credential storage command
4. Document the credential storage command

Implement only the current commit. Do not build the whole feature first and divide it afterward.

## Track progress

The plan lists each task by name with its planned commits as subject lines. Show it once, before editing. After that, do not repeat it; each handoff's **What's next** section shows where the work stands.

When the user makes a task's last commit, review the task before starting the next one: list its commits from `git log` with short hashes and summarize what the task delivered.

If the request needs only one commit, skip the plan and **What's next**.

## Justify dependencies

Add a package only when the standard library, existing dependencies, or a few lines of code cannot reasonably do the job.

In a dependency commit's **Summary**, after the first sentence, state:

- The package and exact version, or the old and new versions for an upgrade
- The capability it provides, or the reason to upgrade, and why existing options fall short
- Notable tradeoffs: maintenance, license, compatibility, or transitive dependencies

Scale detail to the package's size and familiarity.

## Implement and validate

Do not stage or unstage anything unless the user asks. Staged work you did not make does not block your edits.

Before each handoff, in order:

1. Recheck Git state for new user changes, even in files you edited.
2. Run the applicable formatting, lint, build, and test checks, and follow any user instructions about them. Limit auto-formatters to the proposal's files. Pre-commit hooks normally check only staged changes, so run them against the proposal's files instead, for example `pre-commit run --files <paths>`.
3. Fix failures the proposal caused, then rerun the affected checks. A fix the commit needs to pass belongs in it; an unrelated improvement you notice does not, so plan it as its own commit. Ask when a fix needs broader scope, a product decision, or more authorization. Never weaken tests, suppress valid diagnostics, or disable checks.
4. Review the complete proposal, including new, deleted, and generated files. Remove unintended files you created; never delete the user's files. Flag any secrets in **Known issues**. Recheck the subject against the **Separate** list.
5. Report failures unrelated to the proposal. If a check passed only because of other uncommitted changes, say so.

## Write the commit subject

Follow the repository's conventions, such as Conventional Commits or the style of recent `git log` subjects. Otherwise, write an imperative subject of 72 characters or fewer that starts with a specific verb and names what changes, such as "Reject expired password-reset tokens". Name the change, not the activity: "Return 404 for unknown account IDs", not "Update handlers".

Leave out the commit body unless the repository requires one or the user asks for it. Put review rationale in the handoff's **Summary** instead.

## Present the proposal

Render the following Markdown structure without an enclosing code block. Add no opening status sentence, extra sections, or closing instruction. Include **Validation steps** only for behavior automated checks cannot confirm, such as a UI change, an interactive CLI prompt, or a call to a third-party API. In them, separate what you verified from what the user still needs to test. Omit **Checks** lines that do not apply.

### Proposed commit

`<precise imperative subject>`

**Summary**

<What this commit does and why.>

**Validation steps**

Setup: <requirements; omit if unnecessary>

1. <Action.> **Expected:** <observable result.>

**Files**

- `<affected path>`

**Checks**

- **Passed:** <command and result>
- **Failed:** <command and reason>
- **Not run:** <check and reason>

**Known issues**

<Limitations, decisions needed, or unrelated staged work; otherwise None.>

**What's next**

`<next commit's subject>` (<continues or starts> <task name>), or "None" if this is the request's last commit.

**Diff**

<Review link or exact inspection commands covering the complete proposal.>

---

Account for untracked files, which plain `git diff` omits, and exclude unrelated changes, for example `git diff -- <paths>`. Print a full diff only on request, in a fenced `diff` block.

## Wait and resume

After the handoff, stop. Do not poll for the commit or start the next one.

While waiting, answer questions and revise the proposal on request, then present the updated handoff.

When the user returns, check `git status` and recent history:

- A commit that does not contain the proposal does not complete it.
- The user's edits are authoritative. Never restore work they removed.
- If they committed only part, propose the rest next unless they deferred it.
- Rerun checks on anything changed since you last validated it.

If a task is now complete, present its review. Then continue with the next planned commit. Ask when intent is unclear.

Never push, amend, rebase, force-push, or rewrite history unless the user explicitly asks. Never bypass commit hooks.

## Opt out or change scope

If the user asks to skip checkpoints, such as "don't stop for each commit" or "make this one commit", keep working without proposals or pauses for the scope they state. Opting out never lets you commit: leave all changes for the user, and resume checkpoints only when asked.

Skipping a single checkpoint applies only to that one. Note which changes belonged to it so they can still be committed separately.

When scope changes, update the plan and say what changed in one line. At completion, present the review of the last task and report what was not validated and anything uncommitted or deferred. 

For a task left uncommitted because checkpoints were skipped, summarize its changes and files instead of listing commits.
