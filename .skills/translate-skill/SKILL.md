---
name: translate-skill
description: Use this project skill when translating, refreshing, or reviewing sscodeai/skills content for the Japanese localized repository sscodeai/skills-ja-JP. Applies to skill files, README content, CLAUDE.md, CONTEXT.md, docs, and other upstream user-visible prose where behavior-critical identifiers must be preserved.
---

# Translate Skill

Use this skill to localize upstream `sscodeai/skills` content into Japanese for `sscodeai/skills-ja-JP`.

This skill is for **content localization**, not Git synchronization.

## Operating principle

Translate user-facing English prose into natural Japanese for native Japanese developers while preserving all behavior-critical content exactly.

The target repo is an independent Japanese localized edition. It should receive localized content, not upstream repository metadata.

## What to translate

Translate natural-language prose, including:

```text
README explanations
skill instructions
skill descriptions
agent-facing guidance
maintainer-facing guidance
docs prose
examples that are written as prose
```

## What to preserve exactly

Do not translate or rewrite:

```text
directory names
skill names
slash commands
CLI commands
code blocks
inline code
file paths
package names
tool identifiers
API identifiers
environment variable names
frontmatter keys
JSON/YAML/TOML keys
Markdown link URLs
behavior-critical labels
```

Preserve Markdown structure, heading levels, list nesting, tables, link targets, relative paths, and code fences.

## Repository-path localization

When translating user-facing installation examples, replace the upstream repo path:

```text
sscodeai/skills
```

with the localized repo path:

```text
sscodeai/skills-ja-JP
```

Only make this replacement where the command or prose is telling users how to install or use the localized repo.

Do not remove attribution to the upstream project.

## Frontmatter rules

Preserve frontmatter keys exactly.

For frontmatter values:

- Keep `name` values unchanged.
- Keep identifiers, commands, paths, package names, URLs, and tool names unchanged.
- Translate `description` only when it is natural-language prose.
- Flag ambiguous values rather than guessing.

## Japanese localization style

Use Japanese that is:

```text
natural for native Japanese developers
concise
practical
accurate
not overly formal
consistent across the repo
```

Prefer clear developer-facing Japanese. Avoid translationese and excessive politeness.

Keep common engineering terms in English or standard katakana when that is clearer for Japanese developers. Examples include:

```text
issue
PRD
TDD
ADR
CLI
API
CI
codebase
refactor
mock
red-green-refactor
review
agent
skill
```

Translate surrounding prose naturally even when these terms remain in English.

## Workflow for a single file

When translating one file:

1. Identify the file path and file type.
2. Classify the file as translatable prose, mixed content, config/metadata, or non-translatable.
3. Protect behavior-critical spans before translation.
4. Translate only natural-language prose.
5. Restore protected spans exactly.
6. Check that commands, code blocks, paths, URLs, identifiers, and frontmatter keys are unchanged.
7. Check that localized install commands use `sscodeai/skills-ja-JP`.
8. Return the translated file content or a patch, plus any review flags.

## Workflow for a repo refresh

When refreshing from upstream:

1. Treat upstream as a content source, not as Git history.
2. Identify new, changed, and removed content files.
3. Translate new and changed prose-bearing files.
4. Copy or preserve non-translatable support files only when they are in scope.
5. Preserve the localized repo's README positioning and install path.
6. Complete the validation steps below after syncing structure, content, indexes, and behavior-critical spans.
7. Add a concise entry to the README sync log.
8. Record validation results, translation executor, and translation strategy summary in README.
9. Flag ambiguous files, removed files, or risky transformations for maintainer review.
10. Summarize translated files, copied or preserved files, removed files, skipped files, validation results, and review flags.

## Validation steps

After every upstream content refresh, complete and record these checks:

1. Run `node scripts/check-translation.mjs` to verify Markdown structure, frontmatter, README install path, and license invariants.
2. Check public skill index consistency: skills listed in `.claude-plugin/plugin.json` must appear in the top-level public `README.md` index; skills not listed in the plugin should not be presented as public top-level install targets.
3. Compare against the in-scope file list from `upstream/main`, confirming no upstream files are missing and no stale files remain unless local policy keeps them.
4. Check behavior-critical content in shared Markdown files: frontmatter keys and `name` values unchanged, fenced code blocks balanced, paths, commands, URLs, and identifiers preserved.
5. Run `git diff --check` and `git diff --cached --check` to confirm patch hygiene.
6. Check that the README sync log points to the latest upstream short SHA and includes the local sync commit when available; do not leave unresolved placeholders after commit.
7. Scan for stale install paths and old repo names that still point at upstream install commands.
8. Run `node scripts/audit-english.mjs` as a manual review queue. This script will report many acceptable English terms, commands, examples, and identifiers; use it as a review aid, not a hard failure gate.

Validation results should be recorded in the top-level `README.md` as a short checklist. Do not paste full command output into README.

## README sync log

Each upstream refresh should update the top-level `README.md` sync log with one short entry.

The entry should include:

- the refresh date in `YYYY-MM-DD` format
- the upstream source revision, usually `sscodeai/skills@<short-sha>`
- the local sync commit, if it already exists; otherwise use a short pending note and replace it after commit
- a one-sentence description of the visible content change

Keep this record concise. Do not move the detailed translation workflow into README; link to this skill instead.

Example:

```text
- 2026-07-05: Synced upstream `sscodeai/skills@272f99b`, local commit `pending`. Initialized Japanese localization infrastructure and repository metadata.
```

## Required output format for review

When reviewing a translation refresh, provide:

```text
Changed files:
- ...

Translated files:
- ...

Copied or preserved files:
- ...

Removed or stale files:
- ...

Review flags:
- ...

README sync log:
- ...

Validation results:
- ...

Invariant checks:
- install commands point to sscodeai/skills-ja-JP
- code blocks preserved
- frontmatter keys preserved
- paths and identifiers preserved
- Markdown structure preserved
```

## Fail-closed rule

When unsure whether text is behavior-critical, preserve it and flag it.

Do not silently rewrite anything that could affect installation, skill discovery, command execution, file references, API calls, tool use, or agent behavior.
