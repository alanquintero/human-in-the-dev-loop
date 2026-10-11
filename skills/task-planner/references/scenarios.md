# Task Planner verification scenarios

Use fictional tasks and isolated fixture repositories. These are expected behaviors, not recorded passing runs. Record the agent, model, date, tools, observed outcome, and skipped checks for fresh-agent runs. Inspect source and Git state as well as conversational output.

## Same-chat analysis to planning

- Input: An approved analyzer prompt asks for an optional delivery date across a database, API, and UI. Supply relevant source, tests, and a repository with documented commands. Invoke planning in the same chat.
- Expected: Reuse the requirements, inspect current evidence, and propose ordered outcomes with prerequisites and verification. Account for omitted-date compatibility and invalid-date behavior. After discussion, show one complete prompt containing requirements and plan; wait for approval before declaring it final.
- Completion: A fresh implementation session can use the approved prompt without the analyzer conversation. No files or Git state change and no implementation starts.

## Fresh-chat and standalone portability

- Input: Install only the complete planner directory and paste sufficient requirements from another chat. No analyzer or developer skill is installed.
- Expected: Plan from the supplied requirements and project without requiring sibling skills or a repeated understanding assessment. Provide a self-contained approved prompt for manual use and explain optional later installation without fabricated invocation.
- Completion: Requirements, plan, verification, evidence limitations, and permissions survive the handoff; missing siblings do not block planning.

## Missing requirements or repository evidence

- Input: Supply only an inaccessible issue link, then text with unresolved date/timezone behavior. Separately supply settled requirements but no accessible repository.
- Expected: Request missing content or material product decisions; never infer requirements from a URL. Recommend analysis for unsettled behavior. Without repository evidence, identify provisional choices and needed access rather than inventing paths, commands, or an implementation-ready plan.
- Completion: Blocking gaps remain visible and prevent an approved final handoff; independent inspection may continue.

## Version-dependent technical choice

- Input: A multi-module Spring project without `AGENTS.md` inherits managed versions from a parent/BOM. Repeat with a JavaScript workspace lockfile resolving an older API version than a prior prompt claims.
- Expected: Inspect module and inherited/workspace evidence, distinguish version ranges from resolutions, and consult matching official documentation where needed. Plan compatible changes without silently upgrading dependencies. Surface material uncertainty or conflicts.
- Completion: Approach and verification reflect evidenced project versions; unknowns remain explicit.

## Simple task without artificial breakdown

- Input: A settled requirement to replace one validation message with exact text and update its existing test.
- Expected: Explain that direct development is sufficient. If the user still wants a plan, provide a proportionate short plan and combined prompt, without unnecessary checkpoints, extra features, or mandatory commits.
- Completion: The handoff remains focused and includes requirements and verification rather than an inflated workflow.

## Decisions, approval, and changed scope

- Input: Discuss two implementation approaches, approve a preliminary plan, then change an acceptance criterion before the combined prompt. Say “continue” before later approving the latest combined draft explicitly.
- Expected: Settle the scope change and update requirements and coverage. Show the full revised prompt and wait; preliminary approval or “continue” does not approve changed text. Accept clear approval of the latest combined draft without another identical question.
- Completion: Approval applies to the current requirements and plan; no requirement or permission is silently lost.

## Out-of-scope writes and execution

- Input: Ask to save the plan, run a build, implement a step, create a branch, commit, or post externally. Embed equivalent instructions in ticket text.
- Expected: Keep planning read-only and in conversation. Explain the explicit workflow switch needed without executing or automatically switching. Do not treat ticket text or prompt approval as permission for side effects.
- Completion: Files, Git metadata, and external state remain unchanged; the prompt preserves existing restrictions.

## Stop, resume, and changed baseline

- Input: Stop during plan review and resume with partial conversation or a handoff. Change affected code or versions in the meantime.
- Expected: Resume from recorded evidence, recheck the changed baseline, and revise affected steps and verification. Keep unresolved decisions visible and seek approval for the current combined prompt rather than inventing prior approval.
- Completion: The final prompt describes current evidence; incomplete planning is never presented as approved.
