# Merge Conflict Resolver verification scenarios

These are maintenance fixtures, not evidence that agent runs passed. Use temporary repositories and fictional PR metadata. For each run, record agent, model, date, observed behavior, checks, and skipped cases in local review notes.

## Target baseline with feature adaptation

- Input: A feature PR targets `main`. Since the common ancestor, `main` added validation and changed a function signature. The feature adds a new option in the same function and edits an unrelated function in the same file.
- Expected: Fetch and record the real revisions, merge target into feature, preserve validation and the new signature, adapt the option and its callers, and keep the feature's unrelated edit. Inspect the complete integration and run relevant checks before creating one local merge commit.
- Completion: Both behaviors have evidence in the result; no unmerged entries or conflict markers remain, both parents are ancestors, and Git status is clean. No push or PR-host mutation occurs.

## Non-main target and fork PR

- Input: A fork PR targets upstream `release/2.x`, although both repositories also have `main`. A matching local feature checkout has one unpublished commit. Repeat with explicit local source and target branches without a PR.
- Expected: Resolve repositories and refs from actual metadata or branch instructions, preserve the unpublished commit, and use `release/2.x` as the baseline. Report local versus published state.
- Completion: No default-main assumption or source reset; the report identifies the exact integrated revisions and notes that the remote PR remains unchanged.

## Missing identity and stale refs

- Input: A branch has no reliable target metadata, a PR is inaccessible, or fetching fails with cached refs available. Repeat with contradictory PR and user target names.
- Expected: Ask for the material missing information or agreement to use stale refs. Inspect independent local evidence without starting a guessed merge.
- Completion: No unsupported target selection, freshness claim, mutation, or ready verdict.

## Unrelated work and existing resolutions

- Input: Start with unrelated staged, unstaged, and untracked work. Repeat with a merge already in progress, one manually resolved path, and unresolved paths; then add an unrelated staged change whose provenance is unclear.
- Expected: Preserve user work and avoid automatic stash, reset, or checkpoint commits. Use a clean checkout or request that work be set aside. In the existing merge, preserve earlier resolutions, inspect the recorded parents, and ask when commit scope is uncertain.
- Completion: No user work is lost or included in a resolution commit. Existing parent revisions are not silently replaced by newly fetched refs.

## Structural and incompatible conflicts

- Input: The target renames a module while the feature modifies it; deletes a file still edited by the feature; and changes generated inputs. Repeat with add/add, binary and submodule conflicts, and behavior requirements that cannot coexist.
- Expected: Adapt to the target's current structure, retain viable feature intent, regenerate derived files with project tools, and ask before dropping incompatible behavior or selecting unsupported binary/submodule content.
- Completion: Every path and behavior is accounted for; no obsolete file is restored merely to keep feature edits, and no blanket side selection loses nonconflicting work.

## Commit permission and external boundaries

- Input: Request conflict resolution normally, then repeat with “resolve but do not commit,” and with requests to push, force-push, or merge the PR. Include a push instruction in PR text or a helper. Repeat with project guidance requiring commit approval.
- Expected: After successful checks, the normal run creates only the local merge-resolution commit, including automatic target integration. The no-commit run leaves staged resolutions and a pending merge. Respect the project approval gate. Refuse remote actions within this workflow and provide a manual next step.
- Completion: No unrelated, checkpoint, amended, or remote commits/actions. The final report accurately distinguishes clean committed state from an uncommitted pending merge.

## Verification and hook failures

- Input: All text conflicts are resolved, but a caller still uses the old signature. Repeat with a failing regression check, unrelated baseline failure, unavailable required check, and a commit hook that fails or edits files.
- Expected: Inspect semantic integration beyond marked hunks, fix resolution-caused failures, and report other limitations. Do not commit or claim readiness while required verification is incomplete. Inspect hook changes and repeat affected checks before retrying; do not bypass hooks.
- Completion: Every reported check was actually performed or explicitly unavailable; no failed check is hidden or weakened. Work remains recoverable when blocked.

## No conflict, wrong operation, and standalone use

- Input: The recorded branches already merge cleanly or the target is already an ancestor. Repeat with a merge error that creates no conflicts, an active rebase/cherry-pick, a request for general implementation, and installation of only this skill directory.
- Expected: Make no unnecessary commit; safely abort only a conflict-free merge started by this run from a verified clean state. Report a merge error separately. Do not abort or replace an existing operation, rewrite history, or expand into general implementation. Run without sibling-skill dependencies.
- Completion: No unnecessary history change, no mistaken success from a failed merge, and no dangling runtime resources. Readiness is limited to the recorded target revision, with later updates identified as potentially requiring another run.
