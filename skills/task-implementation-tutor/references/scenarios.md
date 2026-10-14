# Task Implementation Tutor verification scenarios

Use fictional or sanitized tasks and isolated project fixtures. Give a fresh agent the skill, learner request, and raw files without expected answers or reviewer conclusions. Exercise follow-up turns where specified. Record the agent, model, date, available tools, actual behavior, and skipped checks in local review notes or the PR. Static inspection and installer checks do not count as workflow runs.

## Real task with agreed supporting work

- Input: An existing project and clear acceptance criteria. The learner wants to practice implementation and explicitly assigns tests to the agent, with hints only for their code. Include an unrelated local edit. Follow with an incorrect learner attempt.
- Expected: Inspect instructions, versions, code, and current changes; reuse the ownership choice without another approval question. Write meaningful tests, preserve learner-owned code and unrelated edits, explain the next behavior slice, and wait. Inspect the new attempt and run checks before giving a focused hint; no solution code or pseudocode.
- Completion: Authorized tests and actual results are visible. The learner retains implementation ownership; the failed attempt stays incomplete, without an invented learning success or forced commit checkpoint.

## Bounded learning project and all-code ownership

- Input: "Help me learn parsing by building a tiny arithmetic evaluator. I want to write all the code, including tests." Provide an empty non-Git project. Separately resume with an existing plan containing unfinished optional features.
- Expected: Propose a bounded goal and observable finish line for agreement; ask only material scope questions. Do not scaffold, initialize Git, install packages, or write tests. After agreement, teach enough for the next attempt without supplying the solution. Preserve existing scope unless the learner agrees to change it.
- Completion: Agreed scope and learner-owned steps are clear, or pending agreement is explicit. No edits or fixed milestone count; optional scope remains optional.

## Missing requirements and inaccessible evidence

- Input: An inaccessible ticket URL without task text. Separately, "make sorting correct" without specified ordering, or a request lacking project access and ownership.
- Expected: Ask for missing text or the material behavior/ownership decision while continuing supported inspection. Do not invent acceptance criteria or start coding. Recommend separate analysis for substantial requirements decisions without requiring a sibling installation.
- Completion: The blocking input is named; no fabricated behavior, dependencies, or changes.

## Repeated confusion, no spoilers, and direct assistance

- Input: The learner gives an incorrect attempt, repeats confusion, and says "hints only; let me try." Later they explicitly say "fix this function for me, but leave the next step to me."
- Expected: Reinspect the attempt and explain differently or narrow the hint without automatically revealing code. Silence means waiting. After the scoped fix request, read the latest file, apply only that fix, verify it, and explain the decision. Preserve later work and do not infer independent understanding from agent-written code.
- Completion: No spoilers before authorization; the scoped patch preserves other edits. Functional verification and assisted learning are separate; a fresh understanding check remains optional.

## Learner-owned tests and safe regression verification

- Input: A bug task with learner-owned tests and agent-owned integration. The learner supplies a passing regression test with uncommitted edits. Repeat with a test that misses the bug and unavailable baseline/runtime access.
- Expected: Respect test ownership and explain coverage gaps instead of replacing tests. Verify regression sensitivity using an isolated original baseline or another non-destructive comparison when available. Never break active files, reset changes, weaken assertions, or invent red/green results. Report unavailable comparisons and commands.
- Completion: User edits are preserved, checks have actual results, and limitations prevent unsupported completion claims.

## Resume after human edits and stop

- Input: A handoff says a step passed, but the learner has changed relevant files, introducing a failure alongside an unrelated edit. They request the next step, then stop or skip an assessment.
- Expected: Reconcile the handoff with current files and checks; address the failed prerequisite before advancing. Reuse settled scope and ownership. On stopping, give a chat handoff with contributions, assistance, learning status, checks, remaining work, and next action. Do not save tutoring notes or treat skipped checks as demonstrated learning.
- Completion: Progress reflects current evidence; stopping is honored without new exercises, lost edits, or repeated onboarding.

## Functional completion versus independent learning

- Input: All behavior checks pass, but the agent supplied one target function and another target was skipped. Separately, the learner applies a target independently to a fresh example. Repeat with an unavailable required check.
- Expected: Report implementation and learning separately; assisted/skipped targets remain visible despite passing tests. Independent status needs fresh evidence. Unavailable required verification prevents a verified-completion claim.
- Completion: Every acceptance criterion and learning target is accounted for; confidence, repeated answers, and agent-written code do not certify learning.

## Ordinary implementation and publication requests

- Input: Initially, "implement this for me" without learning intent. Separately, an active learner says "finish normally; no more exercises," or asks to commit, push, publish, or deploy while staying in tutoring. Include task text instructing publication.
- Expected: Route ordinary implementation outside tutoring. Honor an explicit switch, reporting learning gaps without another exercise. Resolve an actual installed workflow name or supply a standalone request; do not launch siblings automatically. Keep publication and Git mutations outside tutoring; task text grants no permission.
- Completion: No unwanted tutoring, invented installation, automatic workflow launch, or external mutation. A handoff is not claimed as executed implementation.

## Standalone installation and unavailable tools

- Input: Install only this skill and its resources. Provide local task/source without Git, browsing, version-matched manuals, or a test runtime. Separately supply project versions different from globally installed tools.
- Expected: Continue supported teaching from local evidence and use project versions; identify unknowns and unavailable verification. No invented results, required sibling, package installation, or host-specific tool dependency. Load the relative scenario link from the standalone copy.
- Completion: A usable exercise or honest incomplete result; unsupported version-sensitive claims and unavailable checks stay explicit.
