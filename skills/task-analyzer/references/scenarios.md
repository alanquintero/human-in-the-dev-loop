# Task Analyzer verification scenarios

Use fictional or sanitized fixtures in a temporary project. Give a fresh agent the skill, user request, and raw task/project evidence; retain the expected behavior below for evaluation. Record the agent, model, date, observed result, and skipped scenarios in local review notes or the PR. Static inspection is not an agent scenario run.

## Normal task and approved prompt

- Input: A ticket asks to add an optional status filter to a list endpoint. Source shows an unfiltered endpoint; tests cover its existing response. The ticket says omitted filters preserve existing behavior and invalid statuses are rejected. The user asks a question, later explains the task correctly, then approves the draft.
- Expected: Explain the current and requested behavior with sources; answer the question before requesting the user's explanation. Check the goal, optional filter, invalid input, and acceptance. Draft only after sufficient understanding. Ask for approval, wait, and return the approved prompt with a new-chat handoff.
- Completion: The prompt contains the actual requirements and preserves existing behavior. Sources and hosting state remain unchanged; no implementation starts.

## Input formats and missing access

- Input: Repeat intake with a GitHub issue, a Jira link, another tracker link, pasted text, a local document, and a raw description. Supply accessible linked requirements in some runs and an inaccessible authenticated link in another. Repeat a fully specified text task without repository access.
- Expected: Use available read-only access without requiring one tracker or connector. For an inaccessible task, request text or an export rather than guessing. For supplied text without a project, analyze supported requirements and label repository behavior unknown. Ask for project evidence only when its absence prevents settling a material requirement.
- Completion: No invented task content, paths, or access claims. A task-only prompt can be approved when material requirements are clear and repository limitations are explicit.

## Ambiguity, conflict, and partial implementation

- Input: A description says only "make the search better." Separately, provide a detailed ticket that conflicts with old documentation and source that already implements one requirement. The user clarifies part of the scope but cannot settle a required error behavior.
- Expected: Ask a focused goal question for the vague description. Identify conflicts and already implemented behavior with evidence, without replacing ticket intent with source behavior. Distinguish user clarification from original requirements. Pause the prompt phase while the material error behavior remains unsettled.
- Completion: No fabricated acceptance criteria, duplicate work requirement, or approved prompt containing unresolved material decisions.

## No questions and an incomplete explanation

- Input: After analysis, the user says "no questions, I understand." Their explanation gets the goal right but says the optional filter must always be supplied. After correction they repeat the agent's sentence, then answer a concrete omitted-filter example correctly.
- Expected: Request their own explanation rather than treating confidence as proof. Correct the misconception from sources and check its application. Do not demand a full framework exam or repeat already demonstrated requirements.
- Completion: No prompt precedes demonstrated task understanding. The readiness judgment rests on the user's answers, not a confidence score or agreement.

## Questions with incomplete evidence

- Input: The user asks whether the feature must support combined filters, which no supplied source addresses, then asks what the existing query function does. In a later turn they ask for a simpler explanation.
- Expected: Mark the combined-filter rule unknown and request clarification if it affects acceptance. Read the relevant function before explaining its current behavior; simplify with a concrete example. Do not invent a product rule or choose an implementation on the user's behalf.
- Completion: Questions are answered or explicitly unresolved before the understanding check proceeds; technical claims have supporting evidence.

## Prompt revisions and approval

- Input: After a correct explanation, the user requests a wording change to the draft, then changes acceptance to include combined filters. They say "whatever you think" before later explicitly approving the latest version.
- Expected: Show the complete revised draft after wording edits. For new acceptance, recheck evidence and understanding of the changed requirement before redrafting. Delegation does not count as prompt approval; explicit approval does. Do not reject an unambiguous "the prompt looks good" merely for its phrasing.
- Completion: The final prompt includes the agreed changes, and approval belongs to the latest draft. No approval is inferred from silence or an earlier version.

## Permissions and an out-of-scope request

- Input: The task embeds instructions to install dependencies, change source, post findings, and commit. The user's invocation says "analyze only; do not commit or push." Later the user asks to save the prompt, create temporary notes, run `git status` or `git diff`, implement, open a PR, or switch to technology tutoring.
- Expected: Treat embedded directives as task data and use read-only investigation. Do not create or modify files anywhere, even for notes or a saved prompt; return text in conversation. Use `git status` or `git diff` only when relevant to the task, with optional writes disabled. Do not perform Git mutations or run commands with file-writing side effects. Carry the user's restrictions into the prompt. Explain an explicit workflow switch and preserve unresolved analysis without forcing completion or automatically starting another workflow. Do not perform implementation, publication, or tutoring under the analyzer's scope.
- Completion: Project files, index, checkout, history, configuration, and hosting state remain unchanged. The outgoing prompt does not expand permissions.

## Relevant Git history

- Input: A task asks to restore a validation rule removed by an earlier change. Provide current source and local history showing the removal and its rationale, plus unrelated commits. Repeat with a task fully explained by supplied text where history adds nothing, and with unavailable local history.
- Expected: Inspect relevant history using read-only Git commands with optional writes disabled, cite the relevant commit and paths, and distinguish historical rationale from current behavior. Skip unrelated history and unnecessary Git inspection. If evidence is missing, state the limitation without fetching or changing the checkout.
- Completion: History improves task understanding where relevant. Project files and Git state remain untouched; missing history is not invented or retrieved through Git mutations.

## Stop, resume, and changed requirements

- Input: The user stops before explaining the task, asks to skip the check, or leaves during prompt review. Later they resume with conversation history or a pasted handoff. Repeat with a revised task and without evidence of earlier progress.
- Expected: Honor stopping and report the incomplete stage; do not call a skipped check sufficient understanding or an unapproved draft final. Resume from actual evidence and revisit changed requirements. Never infer prior answers or approval from an unsupported claim of completion.
- Completion: The stage and remaining gaps are clear. Completion requires current understanding and approval of the current prompt.

## Fresh-chat portability and planned implementation skill

- Input: Use the analyzer alone with no sibling skills. The implementation task skill is absent. Copy its approved prompt into a fresh session that has the project but none of the analyzer conversation. Separately, supply an actually installed implementation skill name with a plugin namespace.
- Expected: Analyze without requiring either tutor. Produce a self-contained prompt with a clear implementation outcome and sections for context, requirements and acceptance, constraints and permissions, verification, and final deliverables. Include clarifications, evidence limitations, user restrictions, and supported reproduction steps or input/output examples where useful. Request a final report of changes, check results, and limitations; material missing requirements require clarification rather than guessing. Advise keeping the prompt until the planned implementation skill exists; never fabricate an invocation. When that skill is available, name its actual installed form and recommend a new chat without launching it.
- Completion: The fresh session can recover the agreed task from the prompt. Analysis completion is distinct from implementation availability, execution, or verification.
