# Human in the Loop

Portable AI skills for the software development lifecycle, with human judgment at the decisions that need it.

> **Unreleased — in development.** Codex marketplace installation is available for trying the development version. Stable release and workflow verification remain pending.

`repo-learning-tutor` has had brief author use; workflow verification across supported hosts and release checks remain pending for all thirteen skills.

## Skills (in development)

### Repository learning

Build general knowledge of a project independently of a particular development task.

| Skill | Purpose |
| --- | --- |
| [repo-learning-tutor](skills/repo-learning-tutor/SKILL.md) | Learn an unfamiliar code or documentation repository through orientation, guided practice, and local progress notes. |

### Task development

For a particular task, follow this order: understand its technologies, understand its requirements, optionally plan the implementation steps, then implement it in a new session. Optionally self-review the changes before requesting human PR review, then handle feedback when it arrives.

| Step | Skill | Purpose |
| --- | --- | --- |
| 1 | [task-tech-tutor](skills/task-tech-tutor/SKILL.md) | Learn the technologies and specific framework concepts the task requires before analyzing it. |
| 2 | [task-analyzer](skills/task-analyzer/SKILL.md) | Understand the task, explain it in your own words, and approve a prompt for a separate implementation session. |
| 2a (optional) | [task-planner](skills/task-planner/SKILL.md) | Choose the review workflow, review ordered steps, and approve one prompt containing requirements and plan. |
| 3 | [task-developer](skills/task-developer/SKILL.md) | Implement and verify the whole task or agreed increments, with manual commits and text gated by human review. |
| 4 (optional) | [task-self-review](skills/task-self-review/SKILL.md) | Check local changes or a draft or open PR against task requirements, report readiness for human review, and fix findings the user accepts. |
| 5 (when feedback arrives) | [pr-feedback-handler](skills/pr-feedback-handler/SKILL.md) | Assess PR comments and their replies, propose fixes for approval, implement approved plans, and draft replies and commit text for manual use. |

Invoke `task-planner` after analysis in the same chat, or paste the approved analyzer prompt into a fresh planning chat. Copy the planner’s final combined prompt into the development chat.

Choose **complete implementation** for one review after the whole task is implemented and verified, with the complete change available for a manual commit afterward. Choose **smaller increments** to implement and verify one agreed coherent part at a time: review it, request corrections, accept it, then manually commit it before authorizing the next part. Planning settles and carries this choice into the approved prompt; direct development asks before changing code. An explicit existing choice is reused. Task size and a plan do not select increments automatically. Final PR text requires review and approval of the combined task; staging, commits, pushing, and publication remain manual.

- **Use planning** when the requirements are understood but the implementation needs coordination: changes span several components, steps depend on one another, the approach needs discussion, or work will span multiple sessions. For example, adding an optional delivery date to orders may require a database migration, backend validation and API changes, a form input, and checks that existing orders still work. A plan makes the order and verification of those changes explicit.
- **Skip planning** when the implementation and verification are straightforward, or the supplied prompt already contains an adequate plan. For example, correcting a validation message with exact replacement text and an existing test can go directly to `task-developer`. Several file edits alone do not require a separate planning step.

If expected behavior or acceptance criteria are still unclear, return to `task-analyzer` before planning. Planning organizes how to implement agreed requirements.

Use these additional development skills independently of the numbered steps when reorganizing an existing PR, investigating failures, or resolving merge conflicts.

| Skill | Purpose |
| --- | --- |
| [task-debugger](skills/task-debugger/SKILL.md) | Investigate unexpected behavior, failing tests, or broken builds; establish the cause and hand off implementation without fixing application code. |
| [pr-splitter](skills/pr-splitter/SKILL.md) | Propose an approved split of an existing large PR, then guide manual construction and verification of smaller independent or stacked PRs. |
| [merge-conflict-resolver](skills/merge-conflict-resolver/SKILL.md) | Preserve the target branch’s changes, integrate the feature changes around them, verify the resolution, and optionally commit it locally without pushing. |

Use `pr-splitter` for changes already implemented in a PR or local branch, rather than a future implementation plan. Approve the proposed boundaries before construction guidance; Git mutations and hosted actions stay manual. The skill checks each increment and the combined result, and keeps split artifacts in chat.

### Reviewing another contributor's PR

| Skill | Purpose |
| --- | --- |
| [pr-reviewer](skills/pr-reviewer/SKILL.md) | Validate task understanding, wait for manual review, assess the user's findings, then review for additional issues and draft accepted comments for manual posting. Read-only throughout. |

### Issue reporting

| Skill | Purpose |
| --- | --- |
| [github-issue-drafter](skills/github-issue-drafter/SKILL.md) | Draft an issue title and description from the conversation or a supplied report, following repository examples and templates, for the user to publish manually. |

### Skill maintenance

| Skill | Purpose |
| --- | --- |
| [skill-improver](skills/skill-improver/SKILL.md) | Improve existing skills from conversation feedback or a described gap, preserving core rules and permissions, and leave local changes for human review. |

Invoke `skill-improver` in the conversation containing the feedback, or supply a short description and the target skill's source. Requested maintenance permits local skill edits; noticing a gap in another workflow only prompts an offer. Existing human decision steps and permission boundaries stay intact. PR feedback replies remain manual; the merge resolver's local resolution-commit exception is not extended to maintenance or other skills.

## Local skill artifacts

The tutors save learning documents in the target project's hidden `.ai-skill-local/` folder, with separate `repo-learning-tutor/` and `task-tech-tutor/` subfolders. They locally exclude the whole folder through Git's `info/exclude` and verify it stays untracked and ignored before writing. They never stage or commit notes or edit the project's `.gitignore`; if safe persistence is unavailable, learning continues in chat. Existing root-level notes are preserved when resuming in the new location.

Other workflows keep conversational output in chat or edit the source files their tasks require. Task Debugger keeps authorized auxiliary artifacts in `.ai-skill-local/task-debugger/` inside an isolated scratch workspace outside the active repository.

## Install in Codex now

With Git and a Codex CLI that supports `codex plugin`, run this single line in your terminal (macOS/Linux or PowerShell 7+) once the default branch on GitHub contains the `AVAILABLE` catalog:

```sh
codex plugin marketplace add alanquintero/human-in-the-loop && codex plugin add human-in-the-loop@human-in-the-loop
```

Codex downloads the repository, registers its marketplace, and installs the whole plugin. You do not need to clone it first or run the Python installer. The line combines two Codex commands; registration must succeed before installation runs. In older PowerShell, run the two commands separately.

Maintainers must push the commit that enables the catalog before sharing this GitHub command; a local `AVAILABLE` change does not update GitHub's marketplace.

If you already cloned the repository, run this from its root instead:

```sh
codex plugin marketplace add . && codex plugin add human-in-the-loop@human-in-the-loop
```

Register the marketplace once. After registration, `codex plugin add human-in-the-loop@human-in-the-loop` is enough. In a fresh Codex CLI 0.160.0 profile, cloning and entering the directory alone did not register the marketplace.

Restart Codex, open a fresh chat in the project where you want to use the skills, and select `repo-learning-tutor`, `task-tech-tutor`, `task-analyzer`, `task-planner`, `task-developer`, `task-self-review`, `pr-reviewer`, `pr-feedback-handler`, `task-debugger`, `merge-conflict-resolver`, `pr-splitter`, or `skill-improver` from the skill picker. See [Codex setup and troubleshooting](docs/local-development.md#codex) for verification, updates, and an old marketplace showing `NOT_AVAILABLE`. Commands follow [OpenAI's CLI reference](https://learn.chatgpt.com/docs/developer-commands).

## Supported agents (planned)

| Agent | Installation route |
| --- | --- |
| OpenAI Codex | Plugin marketplace or copy installer |
| OpenAI ChatGPT Work | Plugin marketplace |
| Anthropic Claude Code | Direct local plugin, plugin marketplace, or copy installer |
| GitHub Copilot | Local plugin (CLI) or copy installer |
| Cursor | Local Agent Plugin or copy installer |
| Google Gemini CLI | Native skill install/link or copy installer |
| Windsurf / Devin Desktop Cascade | Copy installer |
| Cline | Copy installer |
| OpenCode | Copy installer |

These setup instructions follow each provider's official documentation. Codex CLI installation from a local checkout has been checked; fresh-agent workflow runs and other hosts remain unverified. ChatGPT support refers to Work's plugin route. See [the release checklist](docs/releasing.md) before publishing a stable release.

## Local usage and development

Follow the [local usage and development guide](docs/local-development.md) to clone the repository and try all its skills in your chosen agent. Contributing changes is optional. The guide covers every listed agent, including refresh and uninstall steps. To contribute, see [CONTRIBUTING.md](CONTRIBUTING.md).

## Release installation (planned)

Follow the [installation guide](docs/installation.md) for commands, prerequisites, official discovery paths, and cloud-session limitations. Each skill stays in one source directory; agent-specific installation puts it where that agent discovers skills.

## Layout

```text
plugin.json          Plugin identity and development version
skills/              Shipped skill sources, each containing SKILL.md
.agents/plugins/     Codex marketplace catalog (development install available)
.claude-plugin/      Claude Code manifest and empty unreleased catalog
.agents/plans/        Local task notes and plans (ignored by Git)
scripts/             Portable copy installer and its verification guide
tests/               Installer and packaging checks using temporary fixtures
docs/                Installation, authoring, setup, and release guides
AGENTS.md            Shared instructions for agents working on this repository
CLAUDE.md            Pointer to the shared instructions
CONTRIBUTING.md      Contribution and verification workflow
LICENSE              MIT license
```

## Add a skill

1. Read the [authoring guide](docs/skill-authoring.md).
2. Create `skills/<skill-name>/SKILL.md` with metadata and a focused workflow.
3. Add supporting files only when needed, and record representative scenarios and expected outcomes.
4. Follow [CONTRIBUTING.md](CONTRIBUTING.md) to verify the change.
5. Add the finished skill to this README with its purpose and a relative link.

Skills use the [Agent Skills format](https://agentskills.io/specification) and are distributed together using [plugin packaging](https://developers.openai.com/plugins/build/plugins). Document special requirements with each skill.

## Agent setup

See the [repository setup guide](docs/agent-setup.md) for shared instructions, skill locations, and local planning files.

## License

[MIT](LICENSE).
