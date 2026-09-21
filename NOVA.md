# Nova Repository Instructions

This repository is a minimal reference marketplace for Compass plugins. Keep it small, readable, and usable as a copy-and-adapt example.

## How discovery works

1. Read root `marketplace.json` to discover marketplace metadata and plugin entries.
2. Resolve each plugin's `source` relative to the repository root.
3. Read `<plugin>/.compass-plugin/plugin.json` to discover that plugin's components.
4. Resolve `agents` and `skills` paths relative to the plugin directory.
5. Read component frontmatter before deciding whether it matches the user's request.

The root registry advertises plugins; the plugin manifest owns component registration. A file merely existing under `agents/` or `skills/` is not enough — keep the manifest references explicit.

## Repository rules

- Preserve the layout `plugins/<number>-<plugin-name>/`.
- Keep plugin IDs and component names in kebab-case.
- Store each skill at `skills/<skill-name>/SKILL.md`.
- Store each subagent as one Markdown file under `agents/`.
- Use relative paths beginning with `./` in plugin manifests.
- Keep `marketplace.json` and every `plugin.json` valid JSON.
- Never add credentials, tokens, private URLs, or user-specific paths.
- Make the smallest change that satisfies the request.
- Do not add dependencies or executable scripts unless the component genuinely requires them.

## Skill contract

A skill starts with YAML frontmatter containing at least:

```yaml
---
name: skill-name
description: Clear trigger-oriented description
argument-hint: "[expected input]"
context: inline
---
```

The body must use `$ARGUMENTS` when caller input is required. Instructions should define the outcome, key steps, boundaries, and missing-input behaviour.

## Subagent contract

A subagent starts with YAML frontmatter containing at least:

```yaml
---
name: agent-name
description: Clear purpose and invocation trigger
model: sonnet
permissionMode: default
color: green
tools: Read, Grep, Glob
---
```

Grant only the tools needed for its mission. Define a focused role, an observable workflow, and explicit boundaries. A review-only agent must not receive write tools.

## Change workflow

1. Inspect the target plugin manifest and neighbouring components.
2. Add or edit one focused component.
3. Update `.compass-plugin/plugin.json` when component paths change.
4. Update root `marketplace.json` only for marketplace-facing plugin metadata or plugin additions.
5. Update `README.md` when user-facing structure or usage changes.
6. Validate JSON parsing, referenced paths, frontmatter delimiters, and name/path consistency.

## Validation

Use PowerShell from the repository root:

```powershell
$marketplace = Get-Content marketplace.json -Raw | ConvertFrom-Json
$marketplace.plugins | ForEach-Object {
  $pluginRoot = Join-Path $PWD $_.source
  $manifestPath = Join-Path $pluginRoot '.compass-plugin\plugin.json'
  $manifest = Get-Content $manifestPath -Raw | ConvertFrom-Json
  @($manifest.agents) + @($manifest.skills) | ForEach-Object {
    $componentPath = Join-Path $pluginRoot $_
    if (-not (Test-Path $componentPath -PathType Leaf)) {
      throw "Missing component: $componentPath"
    }
  }
}
```

Then inspect the Git diff. Do not commit unless the user explicitly asks.
