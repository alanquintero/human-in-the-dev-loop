# Releasing the plugin

Both manifests are prepared for the first release, `0.1.0`; the version change alone does not publish or verify it. The README's unreleased notice remains until this checklist succeeds. The OpenAI marketplace entry is `AVAILABLE` for development installs. The Claude Code catalog stays empty, and [stable release installation instructions](installation.md) remain planned. Users and contributors can follow the [local usage and development guide](local-development.md) before release.

Packaging checks use the README's explicit unreleased status to keep the Claude catalog closed during release preparation, even with a release version in the manifests. A `-dev` version must retain that notice. After verification, the release transition removes the notice and enables the reviewed Claude catalog together; the checks then require its plugin identity and root source to match.

For development distribution through GitHub, push the commit containing the `AVAILABLE` catalog before sharing the installation command. Verify remote installation in an isolated profile, then upgrade an existing marketplace snapshot and retry installation. A local catalog edit does not update the remote repository or previously downloaded snapshots.

## First release

1. Complete the intended skills and catalog them in the README. Run the [contribution checks](../CONTRIBUTING.md), including representative agent scenarios.
2. Set the release version in both `plugin.json` and `.claude-plugin/plugin.json`; review identity, license, prerequisites, and package contents. Run the installer/packaging checks in CONTRIBUTING.md. Confirm each catalog's name and source resolve to the plugin root.
3. In an isolated release test checkout, confirm the OpenAI marketplace policy is `AVAILABLE`. Add `{ "name": "human-in-the-loop", "source": "./" }` to the Claude marketplace's `plugins` array. Keep the shared Claude catalog empty until the checks below succeed; Codex development installs remain available.
4. Test every advertised native plugin route. For Codex, register `codex plugin marketplace add .`, then install with `codex plugin add human-in-the-loop@human-in-the-loop`; test ChatGPT Work separately through the desktop Plugins Directory. For Claude Code, run `claude plugin validate .` and `claude --plugin-dir .`, then test `claude plugin marketplace add .` and `claude plugin install human-in-the-loop@human-in-the-loop`. For Copilot CLI, run `copilot plugin install .` and check `/skills list`. For Cursor, copy the test plugin files into its local plugin directory and reload. Follow [provider development setup](local-development.md) for paths, invocation, refresh, and removal. In fresh chats, invoke packaged skills and load bundled resources. Registration and installation change local agent configuration; use test profiles.
5. Test Gemini's native `skills install` and development `skills link` routes, including user/workspace scopes, source edits, same-name replacements, and uninstall. For each copy-install route in the installation matrix, install into a temporary consuming project using the script and run a representative skill in that agent. Check personal scope in isolated profiles. Verify resource loading, missing prerequisites, invocation boundaries, and updates/uninstall without affecting unrelated skills. For cloud claims, repeat in that host's remote environment with committed project skills or its documented sync/plugin route; local installation does not verify a cloud route.
6. Record agent/client version, model, OS, date, scope, install route, observed results, and skipped checks. Mark each route as tested or unverified in the installation guide; don't infer support for every host or chat surface from a successful copy. Correct failed routes before advertising them as verified.
7. Prepare the release commit with reviewed catalog changes. Publish it and a matching version tag (for example, `v0.1.0`) with release notes. Repeat every advertised native plugin, Gemini skill-management, and copy-install route against the tagged source in clean test setups. Include `codex plugin marketplace add alanquintero/human-in-the-loop --ref v0.1.0`, Claude marketplace registration from a tagged checkout, and the copy installer from the tagged archive. Substitute the actual tag.
8. After the release's advertised routes succeed, remove the README's unreleased notice and replace planned commands with verified commands pinned to the tag. Retain explicit limitations for unverified routes. If verification fails, keep the warning and correct the package before announcing availability.

## Documentation for the release

Prepare the README and guides before publishing, but change their release status only after the checklist succeeds:

- Keep the audience, starting points, complete skill catalog, workflow diagram, example prompts, and documentation links accurate for the shipped version. Check the diagram's rendering on GitHub, including narrow screens; its handoffs must match current skill boundaries and optional steps.
- Make the verified tagged-release installation the primary README command. Keep default-branch commands in the development guide, labeled as development installation. Replace the example tag with the actual published tag.
- Update the installation verification table with observed route results, dates, client versions, and remaining limitations. Planned routes must not become verified solely because a release tag exists.
- Replace unreleased/planned wording only where backed by release evidence, including README, installation, local-development, repository-setup, and contribution guidance. Preserve limitations on unsupported or unverified hosts.
- Publish release notes listing the shipped skills, prerequisites, relevant permission boundaries, tested routes, workflow coverage, and known limitations. Link to a public verification record; Git-ignored author notes alone cannot substantiate user-facing claims.

## Verification record

Use this format for actual runs in the PR or release notes. It is a blank template, not evidence of completion:

| Source revision/tag | Agent/client version | Model | OS | Scope and installation route | Skill/scenario | Observed result | Skipped checks or limits |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Record the examined source | Record the client | Record the model, or unavailable | Record the OS | Record profile/project and route | Record the workflow and input | Record what actually happened | Record what remains unverified |

Include the run date, tools available, and relevant fixture revisions or sanitized inputs. Separate installer/packaging checks, discovery/resource-loading checks, and behavioral scenario runs. Exercise a complete analysis-to-development handoff, optional planning, both development review choices, and review/feedback handoffs in addition to individual skills. Check missing inputs, changed revisions, user corrections, and approval boundaries without performing forbidden actions. See [CONTRIBUTING.md](../CONTRIBUTING.md) and each skill's scenarios for completion criteria.

## Later releases

Bump both plugin manifests' versions when shipped behavior changes. Repeat scenario and installation checks, document behavior changes, publish a matching tag, and update the installation guide's pinned commands. Development on the default branch must not silently replace the release users installed.
