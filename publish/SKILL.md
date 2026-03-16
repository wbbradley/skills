---
name: publish
description: Prepare a Rust crate for publishing to crates.io — update README, changelog, commit changes, bump version, tag, and guide the user through the final push/publish.
---

## Publish Workflow

Prepare the current Rust project for a crates.io release. `$ARGUMENTS` may specify the version
bump type (patch, minor, major) or an explicit version number. Default to **patch** if unspecified.

### Step 0: Preflight

Read `Cargo.toml` to find the current version. Run `git status` and `git diff` to understand what
uncommitted changes exist. Find the most recent version tag (e.g., `git tag --list 'v*' --sort=-v:refname | head -1`) to establish the baseline for changelog and breaking-change analysis.

If there are no changes since the last tag and no uncommitted work, tell the user there's nothing
to publish and stop.

### Step 1: Update README.md

Read the current `README.md` and scan the codebase (public API surface, examples, CLI flags, etc.)
for anything that should be reflected in the README but isn't. The README should describe the
project accurately — what it does, how to install it, how to use it. This is **not** a changelog;
do not list individual changes or version history. Only edit the README if something is materially
missing or incorrect. If the README is already accurate, skip this step.

### Step 2: Analyze changes and update CHANGELOG.md

Launch a sub-agent (subagent_type: "general-purpose") to perform an in-depth analysis of all
changes since the last version tag. Provide the sub-agent with the last version tag and current
version from Step 0.

The sub-agent should:

> Analyze all changes between the last version tag and HEAD (use `git log <tag>..HEAD` and
> `git diff <tag>..HEAD`). Read through every changed file carefully.
>
> **Breaking change analysis:** Look specifically for:
> - Removed or renamed public functions, types, traits, or modules
> - Changed function signatures (parameters, return types) on public items
> - Changed semantics of existing public APIs
> - Removed or renamed CLI flags or subcommands
> - Changed default behaviors
> - Bumped minimum supported Rust version (MSRV)
> - Removed feature flags
> - Changed serialization/deserialization formats
>
> **Changelog generation:** Categorize all changes into:
> - **Breaking Changes** (if any)
> - **Added** — new features, new public API surface
> - **Changed** — non-breaking modifications to existing behavior
> - **Fixed** — bug fixes
> - **Removed** — removed features or deprecated items
>
> Return:
> 1. A boolean: whether any breaking changes were found.
> 2. A list of breaking changes (if any), each with a clear explanation of what broke and what
>    users need to do to migrate.
> 3. The categorized changelog entries, written concisely from the user's perspective (not
>    developer internals).

After the sub-agent returns:

- If breaking changes were found, **present them to the user** and ask whether the version bump
  should be upgraded to a major (or minor, if pre-1.0) bump. Wait for confirmation before
  proceeding.
- Read `CHANGELOG.md` (or create it if it doesn't exist). Add a new section at the top for the
  new version using this format:

```markdown
## [<new_version>] - <YYYY-MM-DD>

### Breaking Changes
- ...

### Added
- ...

### Changed
- ...

### Fixed
- ...
```

Omit any category that has no entries. Use the date of today.

### Step 3: Commit all pending changes

Stage and commit all meaningful changes (including any README and CHANGELOG updates from prior
steps) following normal commit hygiene — semantic commit messages, no attributions. If there are no
uncommitted changes, skip this step.

### Step 4: Bump the version

Determine the new version:
- If the user overrode the bump level in Step 2 due to breaking changes, use that.
- If `$ARGUMENTS` contains "major", "minor", or "patch", apply that bump to the current version.
- If `$ARGUMENTS` contains an explicit version (e.g., "1.2.3"), use it directly.
- Otherwise, default to a **patch** bump.

Edit the `version = "..."` line in `Cargo.toml`. If this is a workspace with multiple publishable
crates, ask the user which crate(s) to bump before proceeding.

### Step 5: Release build

Run `cargo build --release` to ensure the project compiles cleanly and `Cargo.lock` is updated.
If the build fails, stop and report the error — do not proceed with a broken release.

### Step 6: Commit the version bump

Stage `Cargo.toml` and `Cargo.lock` (if it changed) and commit with the message:

```
chore: bump version to <new_version>
```

### Step 7: Tag the release

Create an annotated git tag:

```bash
git tag -a v<new_version> -m "v<new_version>"
```

### Step 8: Hand off to the user

Tell the user the release is ready and they should run:

```bash
git push && git push --tags && cargo publish
```

Then stop and yield to the user.
