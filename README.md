# Nova Plugin Kit

Date: 2026-09-21

A barebones starter kit showing how to package a Nova plugin for a Compass marketplace, with one skill and one subagent ready to copy and adapt.

## Structure

```text
nova-plugin-kit/
├── marketplace.json
├── NOVA.md
└── plugins/
    └── 01-example-plugin/
        ├── .compass-plugin/
        │   └── plugin.json
        ├── agents/
        │   └── content-reviewer.md
        └── skills/
            └── summarize-text/
                └── SKILL.md
```

## Components

- **Marketplace registry:** `marketplace.json` advertises available plugins and points to each plugin directory.
- **Plugin manifest:** `.compass-plugin/plugin.json` defines plugin metadata and explicitly lists its agents and skills.
- **Subagent:** `content-reviewer.md` gives Nova a focused, read-only reviewer it can delegate editorial work to.
- **Skill:** `summarize-text/SKILL.md` is an inline reusable workflow invoked with text, a file path, or a summary goal.
- **Agent guidance:** `NOVA.md` tells Nova how to inspect, extend, and validate this repository safely.

## Try the components

After installing or loading the plugin in Nova, use requests such as:

```text
Summarize README.md for a new contributor.
Ask the content reviewer to review README.md for clarity and completeness.
```

For a simple project-local installation, copy the components into the consuming project's Compass directory:

```powershell
New-Item -ItemType Directory -Force .compass\agents, .compass\skills | Out-Null
Copy-Item plugins\01-example-plugin\agents\content-reviewer.md .compass\agents\
Copy-Item -Recurse plugins\01-example-plugin\skills\summarize-text .compass\skills\
```

## Validate the repository

Run these checks from the repository root:

```powershell
Get-Content marketplace.json -Raw | ConvertFrom-Json | Out-Null
Get-Content plugins\01-example-plugin\.compass-plugin\plugin.json -Raw | ConvertFrom-Json | Out-Null
Test-Path plugins\01-example-plugin\agents\content-reviewer.md
Test-Path plugins\01-example-plugin\skills\summarize-text\SKILL.md
```

All four commands should complete without a JSON error, and both `Test-Path` calls should return `True`.

## Extend it

1. Add a new component under the plugin's `agents/` or `skills/` directory.
2. Register its relative path in `.compass-plugin/plugin.json`.
3. Add another plugin entry to `marketplace.json` only when creating a separate plugin package.
4. Update this README and run the validation commands.

Co-authored by [Nova](https://www.compassap.ai/portfolio/nova.html)
