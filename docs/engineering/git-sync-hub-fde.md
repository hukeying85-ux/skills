# Git Sync Hub FDE Guide

## What it does

Creates or reviews a Chinese FDE runbook for Git-backed local sync runners. It separates repository claims from source-visible behavior and live host evidence, and can package clearly labeled diagrams into a PDF tutorial. It does not install a watcher, dispatch local commands, or claim access to a user's computer.

## When to reach for it

Use `/git-sync-hub-fde-guide` when someone asks for a Chinese product walkthrough, a source-based operation guide, a safety review, or a PDF for a Git-mediated Windows sync/runner. It is user-invoked because the subject can involve local code execution and disk writes.

## Common questions

### Are the screenshots real captures from the user's device?

Only if the session actually received or captured them. Otherwise the skill makes diagrams labeled as illustrative, not real screenshots.

### Can it say the local host is connected because a README says so?

No. A README or committed receipt is a repository claim. The skill marks current host status unverified unless it has independent, current evidence.

### Can it push a task file to run a command on Windows?

No. This skill is for documentation and static review. It does not create a task-dispatch manifest, install a daemon, or push a commit to cause remote execution.

## It's working if

- The guide names exact source paths and distinguishes claims from verified facts.
- The PDF includes product-specific diagrams, each labeled as a schematic when it is not a real screenshot.
- The review explains which task fields could cause file writes or command execution.
- All deliverables are grouped in one named folder, and no claim is made that the user's local disk was changed without evidence.
