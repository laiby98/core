# Project handoff: Home Assistant Core university assignment

Use this file to resume the project on another computer or in a new Codex chat. It records the repository state and the project context known on 2026-09-26. Keep it updated when the branch, scope, results, or deliverables change.

## Resume prompt for Codex

After cloning the repository, start a new Codex chat from the repository root and use this prompt:

> Read `AGENTS.md`, `PROJECT_HANDOFF.md`, and `artifacts/laiba-chatgpt-handoff.md` before doing any work. Treat them as the project context. Preserve the approved assignment scope, verified facts, existing artifacts, and repository rules. Check the current Git status because it may be newer than the snapshot recorded in the handoff. Clearly distinguish newly verified results from historical results. Do not open a pull request or post anything externally without my review.

This preserves the useful project context, but it is not a raw export of every Codex message. Codex chat history is normally stored by the Codex service or client outside this Git repository, so cloning Git alone does not transfer the chat interface's conversation history. This handoff and the files under `artifacts/` are the portable record available in the repository.

## Repository snapshot

- Repository: Home Assistant Core
- Working directory when this handoff was created: `/workspaces/core`
- Personal remote: `https://github.com/laiby98/core.git`
- Upstream remote: `https://github.com/home-assistant/core.git`
- Branch: `university-demo`
- Commit: `6f2d2aab3105efaf9f38af7b9bfb50e237bbe0a8`
- Commit subject: `Customize demo notification for university assignment`
- Remote state at capture time: local `university-demo` matched `origin/university-demo` before adding this handoff file and the untracked artifacts.
- Base shown in local history: `a7a11cc65a4` on `origin/dev`

The branch's committed change customizes the example persistent notification in `homeassistant/components/demo/__init__.py`. It changes the title to `University Assignment Demo` and the message to explain that the notification comes from a local Home Assistant Core code change.

## Assignment context

- Student: Laiba
- Group size: 7
- Course work: Group Assignment 1 — Program Comprehension
- Selected subject: Home Energy Consumption, represented by Home Assistant's `energy` integration
- Main source directory: `homeassistant/components/energy`
- Laiba's assigned role: Person 1 — Scope and Comprehension Techniques
- Expected group submission: a 4–6 page PDF report
- Expected length of Laiba's section: approximately two-thirds to three-quarters of a page

The analysis focuses on how household electricity statistics are configured, validated, stored, and exposed to the Energy dashboard. Solar production, battery flow, device-level consumption, and calculated costs are included when they clarify that scenario.

The architectural boundary is important: the Energy integration does not talk directly to a physical meter. Device integrations provide sensor states and long-term statistics. Energy selects and validates those statistics, stores preferences, creates supporting values such as calculated costs, and exposes information to the dashboard.

The two complementary comprehension techniques are:

1. Systematic repository search and code reading, beginning with the manifest and following setup, WebSocket handlers, `EnergyManager`, storage, listeners, sensors, validation, and nearby tests.
2. Focused execution and test-based experimentation, using narrow existing tests to check the behavior inferred from static reading.

### Scope boundaries

In scope:

- Household grid-electricity consumption.
- Selection, validation, and storage of energy statistics.
- Energy preferences passed through the WebSocket API.
- Solar production, battery flow, device consumption, and calculated costs where relevant.
- Boundaries among the frontend, Home Assistant Core, recorder/history services, and device integrations.

Out of scope:

- Physical meter protocols.
- Detailed internals of every sensor-producing device integration.
- A complete reconstruction of recorder, frontend, or Home Assistant Core.
- Gas and water consumption except where a shared interface clarifies the architecture.

## Verified evidence recorded for the assignment

The detailed evidence and the current draft are in `artifacts/laiba-chatgpt-handoff.md`. Key source locations recorded there are:

- `homeassistant/components/energy/manifest.json:2-9`: the `energy` domain, `system` integration type, `calculated` IoT class, and dependencies on `websocket_api`, `history`, and `recorder`.
- `homeassistant/components/energy/__init__.py:24-37`: registration of the Energy WebSocket API, frontend panel, sensor platform, and `cost_sensors` runtime mapping.
- `homeassistant/components/energy/websocket_api.py:44-52`: registration of preference, information, validation, solar forecast, and fossil-energy WebSocket commands.
- `homeassistant/components/energy/websocket_api.py:124-145`: the administrator-only `energy/save_prefs` command and delegation to `EnergyManager.async_update()`.
- `homeassistant/components/energy/data.py:754-806`: preference loading and defaults, updates, delayed persistence, and update listeners in `EnergyManager`.
- `tests/components/energy/test_websocket_api.py:50-106`: tests for default preference retrieval and saving.
- `tests/components/energy/test_websocket_api.py:172-203`: tests covering updated preferences, persistence, configured state, cost-sensor information, and solar forecast domains.

The following historical test result was recorded at commit `6f2d2aab3105efaf9f38af7b9bfb50e237bbe0a8`:

```text
uv run --no-sync pytest -q tests/components/energy/test_websocket_api.py::test_get_preferences_default tests/components/energy/test_websocket_api.py::test_save_preferences
..                                                                       [100%]
2 passed in 1.29s
```

Treat this as a previously verified result. Run it again on the new laptop before claiming that the current checkout passes.

## Project files to preserve

These files were untracked when this handoff was created. They must be added and committed before pushing if they are to appear after a fresh clone:

- `PROJECT_HANDOFF.md`: this portable repository and session handoff.
- `artifacts/laiba-chatgpt-handoff.md`: the authoritative detailed assignment context, evidence, draft, and instructions for future AI assistance.
- `artifacts/laiba-person-1-scope-and-techniques.md`: Laiba's concise Person 1 contribution.
- `artifacts/laiba-energy-artifact/README.md`: preview and hosting instructions for the static artifact.
- `artifacts/laiba-energy-artifact/index.html`: the self-contained static website; it needs no build or external assets.

Preview the website from the repository root with:

```bash
python3 -m http.server 8000 --directory artifacts/laiba-energy-artifact
```

Then visit `http://localhost:8000`.

## Save the handoff to Git

Review the files first, then run:

```bash
git status --short
git diff -- PROJECT_HANDOFF.md artifacts/
git add PROJECT_HANDOFF.md artifacts/
git commit -m "Add university assignment handoff and artifacts"
git push origin university-demo
```

The repository's AI policy requires a human to review, understand, and be able to explain every submitted change. Do not open issues or pull requests autonomously. If a pull request is eventually prepared, preserve every part and every unchecked checkbox in `.github/PULL_REQUEST_TEMPLATE.md`.

## Set up the new laptop

Install Git and the prerequisites needed by Home Assistant, configure Git authentication for GitHub, and then run:

```bash
git clone https://github.com/laiby98/core.git
cd core
git switch university-demo
git remote add upstream https://github.com/home-assistant/core.git
script/setup
```

If `upstream` already exists, do not add it again; confirm it with `git remote -v`. Repository instructions require `script/setup` when entering a new environment or worktree. If `uv` cannot find the required Python version because it is outdated, update `uv` using the command documented in `AGENTS.md`, then rerun `script/setup`.

Confirm the restored state with:

```bash
git status --short --branch
git log -5 --oneline --decorate
test -f PROJECT_HANDOFF.md
test -f artifacts/laiba-chatgpt-handoff.md
```

Before committing future code, follow `AGENTS.md`. Use `python3` in the active environment, run relevant tests with `uv run --no-sync pytest`, and finish code sessions with:

```bash
uv run --no-sync prek run --all-files
```

## Context rules for future work

- Preserve the approved integration, scope, revision, file paths, symbols, and verified evidence unless newer inspection establishes a change.
- Label proposals and inferences separately from facts verified against code or tests.
- Do not claim the whole Energy integration was tested; only the two listed tests have a recorded passing result.
- Do not invent supervisor feedback, personal reflection, or contributions from the other six group members.
- Ask Laiba for her own learning and reflection when the assignment requires a personal perspective.
- Keep the contribution readable enough that Laiba can understand and explain it orally.
- Recheck line numbers after rebasing or updating from upstream because source locations can move.
- Never place passwords, access tokens, cookies, private keys, or other credentials in this handoff or anywhere committed to Git.
