# Git procedures and sources

Read when establishing comparisons or preparing the approved manual construction guide. These are adaptable examples, not commands for the agent to execute. Replace placeholders with verified, safely quoted values and explain effects before showing the user mutating commands.

## Evidence and comparisons

Read-only examples, run in the identified repository:

```sh
git status --short --branch --untracked-files=all
git worktree list --porcelain
git rev-parse --verify '<target-ref>^{commit}'
git rev-parse --verify '<head-ref>^{commit}'
git merge-base --all <target-sha> <head-sha>
git log --oneline <target-sha>..<head-sha>
git diff --no-ext-diff --no-textconv --name-status <merge-base-sha> <head-sha>
git diff --no-ext-diff --no-textconv --binary <merge-base-sha> <head-sha>
git diff --no-ext-diff --no-textconv <target-sha> <head-sha>
git diff --no-ext-diff --no-textconv
git diff --no-ext-diff --no-textconv --cached
```

Record target tip, head, and merge base separately. The review diff from merge base to head shows changes introduced by the topic; the target-tip-to-head delta also includes target divergence. Inspect history and merge parents where necessary, not just the final diff. Multiple merge bases, shallow history, missing objects, or provider/local discrepancies require explanation or additional user-supplied evidence before treating scope as established. Status and operation metadata distinguish current user work from those committed snapshots.

Git documents `git diff A...B` as merge-base-to-B and `git diff A B` (also `A..B`) as a direct endpoint comparison. Diff dot notation is not a commit range. [Git diff manual](https://git-scm.com/docs/git-diff). GitHub documents three-dot PR comparisons and notes that diff limits or attributes can hide files; inspect full local data or exports instead of relying only on the displayed diff. [GitHub branch comparisons](https://docs.github.com/en/pull-requests/reference/branches#comparing-branches-in-pull-requests).

## Preserve the original; build fresh branches

Recommended practice for this workflow: construct new branches from pinned approved bases in separate worktrees. Git provides the mechanism; this preference is not a universal PR splitting rule.

The following manual commands create a backup ref to committed head, a new branch, and a linked worktree. Require verified revisions, unused branch names, a safe unused destination outside the current checkout, repository support for worktrees, and enough disk space. Check project hooks/tooling and submodule limitations before using a worktree. They do not copy uncommitted user work or mutate the original branch tip, but worktrees share repository objects and refs.

```sh
git branch <unused-backup-branch> <original-head-sha>
git worktree add -b <new-pr-branch> <unused-worktree-path> <approved-base-sha>
```

Run construction commands inside that new worktree after confirming its branch and clean status. For independent PRs, use the approved target revision for each. For a stack, construct the first increment there, then base the next branch on the user's verified first-increment commit. Repeat upward. Never use `-B` or `--force` to bypass an existing branch/path safeguard. [Git worktree manual](https://git-scm.com/docs/git-worktree).

## Move changes by approved ownership

Select a mechanism after inspecting the actual diff; commit messages alone do not prove boundaries.

**Aligned commits:** manual `git cherry-pick <verified-commit-sha>` replays an entire commit and normally creates a new commit. It updates the new branch, index, and working tree and can conflict. Inspect its prerequisites and resulting diff. A merge commit needs a justified mainline parent; do not guess it or replay target integration commits as feature work. `--no-commit` still writes the index and working tree, so it is not an inspection command. [Git cherry-pick manual](https://git-scm.com/docs/git-cherry-pick/2.45.0).

**Mixed commits/files:** reconstruct the assigned semantic edits in the new worktree from the pinned original diff. For an existing text file on a compatible baseline, the user can select hunks:

```sh
git restore --patch --source=<original-head-sha> -- <specific-path>
```

Explain first that this replaces selected working-tree content, may discard existing edits, and operates against a snapshot, not an approved change manifest. Use it only in the clean construction worktree and reject hunks containing prerequisite-only or other-PR changes. Do not use whole-file restore when it imports another concern or removes newer target work. Where hunks cannot express the boundary, guide manual edits or a reviewed patch containing only the approved edits. Never assume an interactive hunk is an atomic behavior. [Git restore manual](https://git-scm.com/docs/git-restore).

New files may need explicit reconstruction; deletions and renames need intentional removal/movement by the user; binary replacements cannot be split by text hunks. Pair generated files with their inputs and use the repository's generator, checking for output drift rather than accepting unrelated regeneration. Track modes and submodule commits in the coverage map. A shared helper belongs in a prerequisite when consumers require it; do not copy it into two independent diffs.

After inspecting the working diff, instruct the user to stage only approved paths or hunks, inspect `git diff --cached`, run the relevant checks, inspect status again for generated/unrelated work, and commit manually according to repository conventions. For partially staged files, tests run on working-tree content may include unstaged changes: require verification of the actual committed snapshot in a clean worktree. Hooks may alter files or fail; inspect their outcome instead of assuming the intended snapshot was committed.

Git's book documents splitting a commit with interactive rebase, mixed reset, partial staging, and new commits. That rewrites commit identities and is different from choosing PR boundaries. Prefer fresh reconstruction here; if discussing a user-chosen rewrite, explain backup, clean-state, collaborator, conflict, and recovery prerequisites before tailored commands. Never recommend rewriting the original published branch by default. [Pro Git: splitting a commit](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History#_splitting_a_commit).

## Verify the result

For each user-created increment, inspect both comparisons with recorded immutable revisions:

```sh
git diff --no-ext-diff --no-textconv <intended-base-sha>...<increment-head-sha>
git diff --no-ext-diff --no-textconv <intended-base-sha> <increment-head-sha>
git diff --check <intended-base-sha> <increment-head-sha>
```

Reconcile differences between review and direct deltas with ancestry; inspect commits as well as net content to catch repeated prerequisite replay. Request the repository's checks for the committed increment, not only the original head. Record exit status and output. Whitespace checks do not prove functional correctness.

For a stack on the same original baseline, its final tip is the combined candidate. For independent PRs, guide the user to create a disposable integration branch/worktree on that baseline and merge each new branch there in the approved order. Explain that integration writes refs, commits/index/files and may conflict; all such operations are manual. Check alternate orders when order independence is claimed and overlapping edits make that claim uncertain. Run integrated behavior checks and inspect:

```sh
git diff --exit-code --no-ext-diff --no-textconv <original-head-sha> <combined-head-sha>
```

Exit 0 with no differences establishes tracked-tree equality; exit 1 shows differences to explain, and another failure is not a successful comparison. This includes tracked binary contents, modes, paths and submodule pointers, not untracked artifacts or the runtime behavior of submodule contents. Requirements and per-increment checks remain necessary. If target drift required adaptations, the user must also integrate the original change onto the same newer baseline in a separate disposable worktree, preserving original history. Compare those two combined trees and account for target changes and approved adaptations. Without a comparable reference result, equivalence remains unverified.

## Provider guidance, only when relevant

- **GitHub:** changing a PR's base can remove commits from its timeline and make inline comments outdated. Preserve original links and re-anchor concerns manually; inspect the resulting comparison. [Changing the base branch](https://docs.github.com/en/pull-requests/how-tos/create-pull-requests/changing-the-base-branch-of-a-pull-request).
- **GitLab:** its documented CLI stacked-diff feature is an experiment, not a portable prerequisite. `glab stack save` stages, commits, creates a branch and switches; `glab stack sync` pushes and creates chained MRs. The agent must not execute these convenience commands. If relevant to the user's chosen tooling, describe their effects and verify current availability. [GitLab stacked diffs](https://docs.gitlab.com/user/project/merge_requests/stacked_diffs/).
- **Other hosts:** use their actual source/target terminology and official documentation for comparisons, dependencies, retargeting, reviews and checks. Do not assume GitHub or GitLab behavior.

Recommended stack practice: merge prerequisites first; after each merge, verify the actual target revision and descendant ancestry. With squash or rebase merges, simply retargeting may expose prerequisite commits again. Reconstruct the descendant's owned delta on the new target, or explain a tailored manual rebase that excludes the already-delivered prerequisite using its recorded old tip. Reinspect the full diff and rerun checks before further merging. Do not blindly cherry-pick the descendant's entire history or assume a hosted dependency link enforces merge order.

Sources researched on 2026-10-11. These documents establish mechanisms and provider behavior; coherent boundaries, preserving the original, and ownership/aggregate checks are recommendations and constraints of this skill, not a universal standard.
