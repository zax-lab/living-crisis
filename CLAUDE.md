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
no linter or formatter configuration, and no GitHub Actions workflows. Before
the commits that added this file, `main` held exactly one commit: the initial
commit that created the README.

There is, however, a **deployment target already configured outside the
repository** (see *Deployment* below), and it commits the project to a specific
stack. That is the one piece of real signal about where this is headed.

Everything below is split into **what is actually true today** and **what to
establish when the first real code lands**. Note the distinction the deployment
config forces: a stack has been *chosen*, but nothing in the repository
*implements* it. Do not treat the configured framework as though the code for it
exists.

## What is actually true today

### Repository

- Remote: `https://github.com/zax-lab/living-crisis`
- Default branch: `main`
- History: the initial commit that created the README, plus the commits that
  added this file. No tags, no releases.

### Build / test / lint commands

None exist in the repository. There is nothing to build, run, or test locally.
If you are asked to "run the tests" or "start the app", the correct answer is
that no such entry point is defined yet — say so rather than guessing.

### Deployment (configured, but not yet functional)

The repository is connected to a **Vercel** project named `living-crisis`,
configured outside the repo (there is no `vercel.json` tracked here):

| Setting | Value |
| --- | --- |
| Framework preset | `docusaurus-2` |
| Build command | `docusaurus build` |
| Node version | `24.x` |

**Every deployment so far has failed, including production on `main`.** The
build errors with:

```
sh: line 1: docusaurus: command not found
Error: Command "docusaurus build" exited with 127
```

This is expected, not a regression: the build command needs a `package.json`
with Docusaurus installed, and the repository has neither. Production on `main`
has been in this state since the initial commit. Any pull request opened against
this repository will therefore show a failing Vercel deployment until the site
is actually scaffolded — **a red Vercel check on a PR is a pre-existing
condition, not something that PR broke.** Verify before assuming otherwise, but
do not attempt to "fix" it by scaffolding a site as a side effect of an
unrelated change.

The practical implication: **the intended stack is Docusaurus 2 on Node 24**,
and that decision is already made. A first scaffold should satisfy
`docusaurus build` rather than introduce a different framework, or the Vercel
project settings need updating to match whatever is chosen instead.

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
what the task requires.

This applies with particular force to the failing Vercel deployment. It is
visible on every PR and it looks like something to fix, but scaffolding a
Docusaurus site to turn that check green is a large, foundational change that
no small task authorizes. Leave it red unless scaffolding the site *is* the
assigned task.

The framework question is settled (Docusaurus 2), but the remaining
foundational choices — package manager, test runner, lint/format tooling,
content structure — are the maintainer's to make. If a task requires one and it
hasn't been specified, ask rather than picking silently: the first choice made
here becomes the default for everything after it.

## When the first real code lands

The likely first task is scaffolding the Docusaurus site the Vercel project
already expects. Whatever form it takes, it should establish and record here:

1. **Runtime pinning** — Vercel builds on Node `24.x`; pin to match
   (`.nvmrc`, or `engines` in `package.json`) so local and CI agree.
2. **Dependency manifest and lockfile**, plus the install command. The
   lockfile choice fixes the package manager for everyone after.
3. **Build command** — must satisfy Vercel's configured `docusaurus build`,
   or the Vercel project settings must be changed to match.
4. **Local dev entry point** — typically `docusaurus start`.
5. **Test and lint/format commands**, if any, and whether they run in CI. Note
   that there are no GitHub Actions workflows in the repository; the automated
   checks are external — the Vercel deployment, and the Codex GitHub App, which
   reviews a pull request when it is opened, when a draft is marked ready, or
   on an `@codex review` comment.
6. **Content layout** — for a docs site this is the load-bearing structure:
   where pages live, how the sidebar is configured, and where static assets go.

Once those exist, replace the *"What is actually true today"* section above
with concrete content — and update the *Deployment* section, since the
"every build fails" note stops being true the moment the site builds:

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
