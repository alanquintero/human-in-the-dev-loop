# Agent installation (planned)

**Unreleased:** Codex marketplace installation is available for the development version. Stable release commands below remain planned. Codex CLI installation from a local checkout has been checked; workflow runs and other hosts remain unverified. See [release checks](releasing.md) before publishing.

To install in Codex now, follow [Codex setup](local-development.md#codex), which includes a single terminal line for GitHub installation and a route for an existing clone. For other agents or contribution setup, follow the rest of the [local usage and development guide](local-development.md). The tagged release commands below remain planned.

## Verification status

| Area | Recorded evidence | Remaining work |
| --- | --- | --- |
| Codex CLI, local checkout | Registration, installation, bundled-resource copying, update, and uninstall checked on macOS with CLI 0.160.0 on 2026-10-05. | Fresh-agent workflows and tagged-release installation. |
| Codex CLI, GitHub marketplace | Registration succeeded on 2026-10-05, but the served revision had `NOT_AVAILABLE` and installation was rejected. | Repeat remote installation and marketplace upgrade against the available catalog and eventual release tag. |
| Copy installer and package metadata | Automated fixtures cover destination handling, complete resource copying, and manifest/catalog contracts. | Skill discovery and workflow runs in each advertised host. |
| Skill workflows | Brief author use of `repo-learning-tutor` and partial `task-tech-tutor` fixture runs are recorded in local author notes. | Complete representative runs for all fourteen current skills, including installed-resource loading and cross-workflow handoffs. |
| Other native, desktop, and cloud routes | Setup instructions are provided; observed host verification remains pending. | Test each route before describing it as verified. |

See [Codex's installation record](local-development.md#verification-status) for details. Local author notes are not distributed release evidence. Publish actual results and skipped checks with the release using the [release record format](releasing.md#verification-record); do not infer workflow success from installation or fixture checks.

## Shared format, different installation routes

Every skill ships once in `skills/<name>/SKILL.md` using the [Agent Skills format](https://agentskills.io/specification). Copy its entire directory, including scripts, references, and assets. An agent's skill folder belongs in the consuming project or user profile; cloning this repository alone does not install its skills.

The copy installer uses these documented locations; native plugin and skill-management routes are described below. Project paths are relative to the consuming project; `~` means the user's home directory, including on Windows.

| Agent / installer name | Project skills | Personal skills | Official instructions |
| --- | --- | --- | --- |
| Codex / `codex` | `.agents/skills/` | `~/.agents/skills/` | [Codex skills](https://learn.chatgpt.com/docs/build-skills) |
| Claude Code / `claude` | `.claude/skills/` | `~/.claude/skills/` | [Claude skills](https://code.claude.com/docs/en/skills) |
| GitHub Copilot / `copilot` | `.github/skills/` | `~/.copilot/skills/` | [Copilot skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills) |
| Cursor / `cursor` | `.cursor/skills/` | `~/.cursor/skills/` | [Cursor skills](https://cursor.com/help/customization/skills) |
| Gemini CLI / `gemini` | `.gemini/skills/` | `~/.gemini/skills/` | [Gemini skills](https://geminicli.com/docs/cli/using-agent-skills/) |
| Windsurf / Devin Desktop Cascade / `windsurf` | `.windsurf/skills/` | `~/.codeium/windsurf/skills/` | [Cascade skills](https://docs.devin.ai/desktop/cascade/skills) |
| Cline / `cline` | `.cline/skills/` | `~/.cline/skills/` | [Cline skills](https://docs.cline.bot/customization/skills) |
| OpenCode / `opencode` | `.opencode/skills/` | `~/.config/opencode/skills/` | [OpenCode skills](https://opencode.ai/docs/skills/) |

Cascade's current project folder is `.devin/skills/`; the installer uses its documented `.windsurf/skills/` compatibility path. Several agents also read `.agents/skills/`; use one installation route per agent to avoid duplicate skills.

Personal files stay on the local machine unless the agent explicitly syncs them. For remote or cloud sessions, install and commit project skills into the repository that session runs, or use the host's supported plugin/sync route. Cursor syncs personal skills from `~/.cursor/skills/` when enabled. Claude Cowork and cloud sessions use account skills; Claude cloud sessions can also read committed project skills. Local personal folders do not supply those sessions.

## Install one skill

You can install a complete skill directory independently. This repository's plugin routes install the whole bundle; use a standalone skill route when you want only one workflow. Include all references, scripts, and assets rather than downloading `SKILL.md` alone. A workflow that needs another skill for a later handoff will explain that; supplying sufficient standalone inputs avoids unnecessary dependencies.

### From a local checkout

With Python 3.9+, run from this repository's root to preview installing only `repo-learning-tutor` into an existing Codex project. Replace the project path with your own:

```sh
python3 scripts/install-skills.py --agent codex --scope project --project "/path/to/your-project" --skill repo-learning-tutor --dry-run
```

If the preview is correct, install:

```sh
python3 scripts/install-skills.py --agent codex --scope project --project "/path/to/your-project" --skill repo-learning-tutor
```

For personal installation across projects:

```sh
python3 scripts/install-skills.py --agent codex --scope user --skill repo-learning-tutor
```

These copy to `<project>/.agents/skills/repo-learning-tutor/` or `~/.agents/skills/repo-learning-tutor/`, respectively. Change `codex` to an installer agent name from the [installation matrix](#shared-format-different-installation-routes); replace the skill name with any [catalog entry](../README.md#skills-in-development). Repeat `--skill` to select several. Existing destinations are refused without overwriting. The same selection works with a development checkout now or a reviewed tagged checkout after release.

Without Python, copy the entire `skills/repo-learning-tutor/` folder into the chosen agent's discovery folder. To update or uninstall, follow the [copy-installer guidance](#copy-installer).

### From GitHub without a manual clone

The third-party [Skills CLI](https://github.com/vercel-labs/skills#options) supports GitHub sources, skill selection, agent selection, and copying complete skill folders. With Node.js 22.20+ (including npm) and Git, run from your consuming project's root:

```sh
npx skills add alanquintero/human-in-the-dev-loop --skill repo-learning-tutor --agent codex --copy
```

The default scope is the current project. For personal installation instead:

```sh
npx skills add alanquintero/human-in-the-dev-loop --skill repo-learning-tutor --agent codex --copy --global
```

No persistent source checkout needs to be managed by the user; the CLI downloads source as needed. `--skill` selects one workflow, `--agent` selects the destination agent, and `--copy` chooses copies rather than symlinks. Follow the CLI's confirmation prompts. Its agent identifiers differ from this repository's Python installer: for example, use `claude-code`, `gemini-cli`, or `github-copilot` instead of `claude`, `gemini`, or `copilot`. Consult its [agent list](https://github.com/vercel-labs/skills#supported-agents) for destinations; do not assume every host uses the same folder as our installer.

This installs from the repository's default development branch, which must contain the selected skill. For a stable release, use a direct GitHub skill-directory URL at the actual published tag, as supported by the CLI, instead of the default-branch shorthand. Stable release remains planned. The commands above were checked against upstream documentation; remote installation, updates, and agent discovery through this CLI remain unverified for this repository.

Use `npx skills list --agent codex` to inspect project installs, adding `--global` for personal installs. For removal, use `npx skills remove repo-learning-tutor --agent codex` with the same scope; consult the CLI's [management commands](https://github.com/vercel-labs/skills#other-commands) for updates. Its overwrite and tracking behavior is separate from our conflict-refusing Python installer. Choose one route and scope to avoid duplicate installations. Restart or reload the agent, check discovery and bundled references, then invoke the standalone skill; in Codex, use `$repo-learning-tutor`.

## Copy installer

After release, download a tagged source archive or clone the release tag, then run from this repository with Python 3.9+. On Windows use `py -3` instead of `python3` if needed. No third-party Python packages are required.

Preview a project installation, then repeat without `--dry-run` to install:

```sh
python3 scripts/install-skills.py --agent copilot --scope project --project "/path/to/your-project" --dry-run
```

For personal installation:

```sh
python3 scripts/install-skills.py --agent claude --scope user --dry-run
```

Replace `--agent` with any name in the table. By default all skills are selected; use `--skill <name>` (repeatable) for individual skills. The script copies complete folders without changing their contents or relying on symlinks. It refuses existing destination names, including dangling symlinks, before copying any selected skill. Empty sources and unknown skill names fail without installing anything. It checks directory names and the presence of `SKILL.md`, not YAML metadata or workflow behavior.

Restart the agent or use its documented reload action, check its skills list, and invoke a skill. For example, Gemini offers `/skills reload` and `/skills list`; Claude Code plugin skills use `/human-in-the-dev-loop:<name>`.

For updates, review and remove only the previously installed skill directories before installing the new tagged version. To uninstall copied skills, remove those directories. Other skills and agent settings are outside the installer's scope. Filesystem errors during copying may leave a partial installation; the script reports the error and exits unsuccessfully.

Manual installation is also supported: copy `skills/<name>/` to `<table-path>/<name>/`. Keep the folder name and all supporting resources intact.

## Codex and ChatGPT Work plugin

The root `plugin.json` packages `skills/`. `.agents/plugins/marketplace.json` catalogs that plugin with `AVAILABLE` installation for development use. This does not publish a stable release or list the plugin in the universal public directory. [OpenAI packaging documentation](https://developers.openai.com/plugins/build/plugins) describes this route for Codex and Work mode in the ChatGPT desktop app.

After release, register the marketplace using its actual tag (replace the example `v0.1.0`):

```sh
codex plugin marketplace add alanquintero/human-in-the-dev-loop --ref v0.1.0
codex plugin add human-in-the-dev-loop@human-in-the-dev-loop
```

The second command installs the whole plugin with its skills. You can also install through the Plugins Directory by selecting Human in the Dev Loop. Marketplace registration alone does not install it. See [OpenAI's developer commands](https://learn.chatgpt.com/docs/developer-commands) for the CLI syntax. Standalone Codex skills can instead use the copy installer. This plugin route does not imply installation into every ChatGPT chat surface.

## Claude Code plugin

`.claude-plugin/plugin.json` lets Claude Code discover the same root `skills/`; it does not duplicate the workflows. The Claude marketplace's `plugins` array stays empty until release because its catalog uses a different format from OpenAI's. [Claude marketplace documentation](https://code.claude.com/docs/en/plugin-marketplaces) describes registration and installation.

After release, register a checkout of the reviewed release tag, then install:

```sh
claude plugin marketplace add /path/to/tagged/human-in-the-dev-loop
claude plugin install human-in-the-dev-loop@human-in-the-dev-loop
```

Use Claude Code's plugin manager to update or uninstall. Use the copy installer instead for standalone skills; it needs no marketplace.

For local usage or development before release, `claude --plugin-dir /path/to/human-in-the-dev-loop` loads the entire checkout without marketplace registration. See the [local usage and development guide](local-development.md#claude-code) and [Claude's development guide](https://code.claude.com/docs/en/plugins/create).

## GitHub Copilot CLI plugin (planned)

Copilot CLI supports the root Agent Plugins 1.0 manifest and the canonical `skills/` layout. After release, from a checkout of the reviewed release tag:

```sh
copilot plugin install .
copilot plugin list
```

This installs the whole plugin. Copilot CLI also accepts a GitHub repository or Git URL as a plugin source. Use `copilot plugin uninstall human-in-the-dev-loop` to remove it; consult [GitHub's plugin reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-plugin-reference) for updates and source formats. For local development, follow [the Copilot setup](local-development.md#github-copilot). IDE and cloud skill-folder setup are separate routes; do not infer those installs from a CLI plugin install.

## Cursor local Agent Plugin (planned)

Cursor supports the root Agent Plugins manifest, as described in its [plugin reference](https://cursor.com/docs/reference/plugins). After release, follow [local usage and development guide](local-development.md#cursor) using the reviewed tagged checkout: copy `plugin.json` and the complete `skills/` directory into `~/.cursor/plugins/local/human-in-the-dev-loop/`, then reload and check **Customize**. Local import controls, precedence, refresh, and removal follow the same development route.

Cursor's [GitHub marketplace import](https://cursor.com/docs/skills#installing-skills-from-a-repository) requires its own `.cursor-plugin/marketplace.json`. This repository does not ship that catalog or a reviewed Cursor marketplace listing. Use local Agent Plugin loading or the copy installer; shared packaging alone does not supply marketplace distribution.

## Gemini CLI native skill management (planned)

Gemini's CLI can install all skills from the `skills/` directory. After release, from the reviewed tagged checkout:

```sh
gemini skills install ./skills --scope user
gemini skills list
```

For project scope, run from the consuming project with the tagged checkout's absolute `skills/` path and `--scope workspace`. Unlike this repository's conflict-refusing copy installer, Gemini's native installer can replace same-name destinations. For development, `gemini skills link` keeps the skill directories connected to the editable source; see [local usage and development guide](local-development.md#gemini-cli). [Gemini's management guide](https://geminicli.com/docs/cli/using-agent-skills/) and [installer implementation](https://github.com/google-gemini/gemini-cli/blob/main/packages/cli/src/utils/skillUtils.ts) document these routes.

## Compatibility limits

Installation makes instructions discoverable; workflow execution also requires the tools and access declared by each skill. A model provider alone does not define skill installation. An agent hosting that model must support skills and the workflow's capabilities. Add other hosts only after checking their documented mechanism; do not claim universal or tested support from the shared file format alone.
