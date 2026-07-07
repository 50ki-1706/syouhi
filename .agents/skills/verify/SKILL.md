---
name: verify
description: Run project verification commands after changes to keep the codebase clean and correct.
---

# Verify

## Purpose

Preserve code quality after any code change. This skill runs the project's own checks to catch lint, format, and build issues early.

## When to use

Run verification after modifying source code, configuration, tests, or generated artifacts, and before reporting a task complete.

## Prerequisites

- Inspect `package.json` scripts to confirm the available commands.
- Use the package manager declared in the `packageManager` field. Currently this project uses `pnpm`.

## Verification commands

Run the checks in this order.

### 1. Lint (primary)

```bash
pnpm run lint
```

This runs `oxlint`, `oxfmt --check`, and `ray lint`.

### 2. Fix lint or format issues if safe

If `pnpm run lint` reports fixable issues, run:

```bash
pnpm run fix-lint
```

Then re-run:

```bash
pnpm run lint
```

Review any changes `fix-lint` makes before continuing.

### 3. Build for non-trivial changes

If the change affects runtime behavior or the build, and lint now passes, run:

```bash
pnpm run build
```

This runs `ray build`.

## Remediation loop

- Read the failure output carefully.
- Make the smallest maintainable fix that resolves the failure.
- Do not change unrelated files.
- Re-run the failing command(s).
- Repeat until all checks pass.

## Working tree hygiene

- Inspect changed files before and after verification.
- Do not hide or ignore failures.
- Do not commit changes as part of this skill.
- Report any pre-existing changes that are unrelated to the current task separately.

## Final report format

After verification, report:

- Commands run.
- Pass or fail status for each command.
- Changes made to fix issues.
- Any remaining issues or warnings.
