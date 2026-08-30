# CLAUDE.md

Guidance for Claude Code and other AI assistants working in this repository.

## Current state of this repository

**This repository is an empty scaffold.** As of the latest commit it contains
exactly two tracked files:

```
README.md    # one line: "# living-crisis"
CLAUDE.md    # this file
```

There is no application code, no build system, no dependency manifest
(`package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, …), no test suite,
no linter or formatter configuration, and no CI workflows. The single commit on
`main` is the initial commit that created the README.

Everything below is therefore split into two parts: **what is actually true
today**, and **what to establish when the first real code lands**. Do not infer
a technology stack, directory layout, or set of commands from the repository
name — nothing in the repository specifies one.

## What is actually true today

### Repository

- Remote: `https://github.com/zax-lab/living-crisis`
- Default branch: `main`
- History: a single initial commit; no tags, no releases.

### Build / test / lint commands

None exist. There is nothing to build, run, or test. If you are asked to "run
the tests" or "start the app", the correct answer is that no such entry point
is defined yet — say so rather than guessing at a command.

### Conventions

None are established yet. The first substantive change to this repository is
what will set them.

## Working in this repository

### Before assuming anything

Because the repository is empty, the usual orientation shortcuts do not apply.
When you pick up a task here, re-check the current state first — code may have
landed since this file was last updated:

```sh
git ls-files                     # what is actually tracked
git log --oneline -20            # what has happened recently
ls -a                            # config and dotfiles at the root
```

If `git ls-files` shows real source files, this document is out of date. Update
it as part of your change (see *Keeping this file current* below).

### Git workflow

- Branch off `main` for all work; do not commit directly to `main`.
- Push with `git push -u origin <branch-name>`.
- Open a pull request for the pushed branch. Draft is a fine default.
- Write commit messages in the imperative mood with a short subject line, and a
  body explaining *why* when the reason is not obvious from the diff.

### Scope discipline

An empty repository makes it tempting to add scaffolding nobody asked for —
a framework, a CI pipeline, a lint config, a directory tree. Don't. Add exactly
what the task requires. Foundational choices (language, framework, package
manager, test runner) are the maintainer's to make; if a task requires one and
it hasn't been specified, ask rather than picking silently, because the first
choice made here becomes the default for everything after it.

## When the first real code lands

The first substantive change should establish, and record here:

1. **Language and runtime**, with the version constraint and how it is pinned.
2. **Dependency manifest and lockfile**, plus the install command.
3. **Entry point** — how to run the thing locally.
4. **Test command**, and where tests live relative to source.
5. **Lint/format commands**, and whether they are enforced in CI.
6. **Directory layout**, with a sentence on what belongs in each top-level
   directory.

Once those exist, replace the *"What is actually true today"* section above
with concrete content:

- **Commands** — the exact invocations a contributor runs, including the
  narrow ones (a single test file, a single package) that are cheaper than
  running the whole suite during iteration.
- **Architecture** — the handful of facts that are not obvious from reading
  one file: how the major pieces fit together, where the boundaries are, which
  patterns are load-bearing, and any non-obvious constraint that has bitten
  someone before.
- **Conventions** — naming, error handling, logging, module organization, and
  anything a change would be rejected in review for violating.

Favor the non-obvious over the discoverable. A reader can run `ls` and read
imports on their own; what they cannot recover cheaply is intent, history, and
the reasoning behind a constraint.

## Keeping this file current

Treat this file as part of the code. When a change alters how the project is
built, tested, run, or laid out, update the corresponding section in the same
pull request. A CLAUDE.md that describes a state the repository has left is
worse than none at all — it produces confident, wrong actions.
