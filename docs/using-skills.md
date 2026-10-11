# Using the skills

Use these workflows in the repository you want to learn or change. Install through your [agent's setup route](local-development.md), select the actual installed skill name and namespace, then send a request like the examples below. Examples are message text after selection, not universal slash commands.

The [README diagram](../README.md#how-the-skills-connect) shows the main handoffs. Start where you need help; you do not have to complete every skill for every task. Learning and analysis do not authorize implementation, and approving a handoff prompt approves its wording, not execution.

## Learn a new repository

Select `repo-learning-tutor`:

```text
Use Orientation mode. I am joining this team and want to understand
the repository's main responsibilities and one representative flow.
Explain one small area at a time, then check my understanding.
```

Orientation provides context before asking questions. Practice asks you to inspect, predict, trace, and explain existing behavior, with hints when needed. Choose either mode, switch when useful, and ask for easier or harder questions. A senior engineer can focus on an unfamiliar module or flow without repeating lessons they already know. The first project map still inventories all modules at a high level.

For an unfamiliar task technology, select `task-tech-tutor`:

```text
Here is the task: [paste its text or an accessible link].
Identify the project technologies and versions relevant to it.
I know the language but need help with the framework concepts.
Teach those concepts without designing the task's solution.
```

Both tutors build understanding rather than implement changes. They check independent explanations or predictions before recording demonstrated learning. You can skip topics or stop; unchecked concepts remain visible. Neither tutor installs dependencies or starts services.

## Understand and plan a task

Select `task-analyzer` when requirements need clarification:

```text
Analyze this task with me: [paste requirements or an accessible link].
Check the current behavior in this repository, explain the expected
change, and help me identify missing decisions. After checking my
understanding, prepare an implementation prompt for my approval.
```

The analyzer separates task requirements from current code and assumptions, answers questions, and asks you to explain the task in your own words. Its approved prompt includes requirements, acceptance criteria, evidence, constraints, and verification expectations.

Use `task-planner` when the understood task spans responsibilities, has dependent steps, or needs discussion of the approach. For example, adding an optional delivery date may need a database migration, API validation, a form input, and compatibility checks for existing orders. Planning orders those changes and their checks.

Skip planning for a straightforward change with exact expected behavior and clear verification, or an input that already contains an adequate plan. Several file edits alone do not require planning. If expected behavior or acceptance criteria remain unclear, return to analysis; planning organizes how to implement agreed requirements.

Invoke planning in the analysis chat, or paste the approved analyzer prompt into a fresh planning chat. The planner produces one approved prompt containing both requirements and plan. Copy it into a new development chat. Skills do not launch these handoffs automatically.

## Develop from clear requirements

Select `task-developer` and supply the approved prompt, or a sufficient standalone request:

```text
Implement this task using complete implementation:
[paste the self-contained requirements, acceptance criteria,
constraints, and verification expectations].
Verify the changes and stop for my review. Keep commits, pushing,
and PR publication manual.
```

Direct development does not require tutoring, analysis, or planning when the request is sufficient. A task link alone can go directly to development only for a very easy, evidenced change with clear behavior and verification; otherwise the developer asks for analysis first. Inaccessible links need pasted text or an accessible export.

### Choose how to review implementation

| Choice | What happens |
| --- | --- |
| Complete implementation | Implement and verify the whole task, then stop for human review. The user can commit the complete change afterward. |
| Smaller increments | Agree coherent parts and dependencies. Implement and verify one part, stop for review and corrections, then wait for its manual commit and authorization before the next part. |

Planning carries an explicit choice into the final prompt; direct development asks before editing if no choice exists. A plan or task size does not automatically select increments. Keep related code and tests together; there is no fixed file count, line limit, or commit per plan step.

The developer provides commit text after explicit review and acceptance of the latest changes. Final PR text follows review and approval of the combined task. Increment acceptance does not establish task completion; all acceptance criteria need combined verification, and smaller-increment delivery includes confirming the final manual checkpoint commit. Manual checks accompany changes needing human observation, even when automated checks pass.

## Review changes and respond to feedback

- **Your changes:** Select `task-self-review` with requirements and the local changes, branch range, or PR link. It reports readiness for human review, with evidence and limitations. Findings require your decision before fixes. After accepted fixes, it reports checks and provides commit text for manual use while asking you to inspect the latest changes. Its verdict does not replace the developer's human acceptance gate.
- **Someone else's PR:** Select `pr-reviewer` with the PR link. Explain the task, perform your manual review, and bring your findings back. The agent evaluates those findings before adding its own. Accepted comments stay in chat for manual posting; the workflow remains read-only.
- **Feedback on your PR:** Select `pr-feedback-handler` with a PR link or specific comment links. It reads the discussion, assesses the latest requests, and proposes fix plans for approval. Approved fixes are implemented locally and verified; replies and commit text remain drafts for manual use.

## Handle other daily work

| Need | Skill and outcome |
| --- | --- |
| Unexpected behavior, failing tests, or a broken build | `task-debugger` establishes a cause through evidence and permitted isolated experiments, then provides a fix handoff. It does not implement application fixes or manage production incidents. Unresolved product decisions need analysis before development. |
| An existing PR is too broad | `pr-splitter` proposes boundaries for approval, then guides manual construction and verification of each PR and the combined result. Git and hosting mutations remain manual; artifacts stay in chat. It does not split a future implementation plan. |
| Merge conflicts | `merge-conflict-resolver` preserves target changes and integrates feature intent. It can stage resolutions and create a verified local merge commit unless prohibited; it never pushes or merges the hosted PR. |
| A discovery needs an issue | `github-issue-drafter` follows repository templates and examples to produce a title and description for manual publication. |
| An existing skill needs improvement | `skill-improver` uses conversation feedback or a supplied gap and source to make focused local edits for human review. It preserves required decisions and permissions. Noticing a gap only prompts an offer; maintenance needs a user request. |

## Notes, boundaries, and resuming

Tutors save learning progress under `.ai-skill-local/` when safe persistence is available. They locally exclude the folder through Git's `info/exclude`, verify it stays ignored and untracked, and preserve legacy notes. If persistence fails, learning continues in chat. They never stage or commit notes or edit project/global ignores. Debugger artifacts require authorization and stay in an isolated scratch workspace outside the active repository. Other workflows keep auxiliary output in chat or edit the source deliverables their tasks require.

Commits, pushes, PR publication, issue publication, and review comments generally stay manual. The merge resolver's verified local staging and merge-commit exception does not extend to development, feedback handling, or skill maintenance. Approval never silently broadens a workflow's scope.

To resume in a new chat, supply the latest approved prompt or handoff and pending decisions. For incremental work, include agreed boundaries, implemented/accepted/committed states, checkpoint evidence, checks, and remaining work. The next session rechecks current repository evidence; it must not assume an earlier approval covers later changes.

## What is verified?

These examples describe intended usage, not passing workflow runs. See [verification status](installation.md#verification-status) and the [release checklist](releasing.md). Discovering a skill and loading its bundled resources are separate from proving its workflow behaves correctly.
