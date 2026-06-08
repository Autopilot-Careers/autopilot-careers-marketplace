# Autopilot Careers — Claude plugin marketplace

A public [Claude Code / Claude Desktop plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces) for the **Career on Autopilot** project. It catalogs the `write-resume-plugin`, which adds resume writing, job analysis, and career planning tools to Claude.

This repository contains only the marketplace catalog (`.claude-plugin/marketplace.json`). It does **not** vendor the plugin source — the entry *references* the plugin in the public [`autopilot-careers-community`](https://github.com/Autopilot-Careers/autopilot-careers-community) mirror.

## Usage

Once installed, try:

```
/write-resume for <LinkedIn job URL>
```

The plugin bundles three cloud MCP servers (HTTP) hosted at `https://mcp.alc0der.dev`:

| Server              | Endpoint           | Purpose                          |
| ------------------- | ------------------ | -------------------------------- |
| `linkedin-fetcher`  | `/fetcher`         | Fetch LinkedIn job postings      |
| `oh-my-cv-render`   | `/renderer`        | Render resumes / CVs             |
| `bullet-embeddings` | `/bullet-embeddings` | Bullet-point semantic matching |

## Add the marketplace

In Claude Code or Claude Desktop:

```
/plugin marketplace add Autopilot-Careers/autopilot-careers-marketplace
```

## Install the plugin

```
/plugin install write-resume-plugin@autopilot-careers
```

- Plugin: `write-resume-plugin`
- Version: `1.18.1`
- Category: Productivity

## How it resolves

This marketplace resolves **publicly with no access caveats**. Both steps work out of the box:

```
/plugin marketplace add Autopilot-Careers/autopilot-careers-marketplace
/plugin install write-resume-plugin@autopilot-careers
```

The plugin entry uses a `git-subdir` source pointing at the public mirror [`Autopilot-Careers/autopilot-careers-community`](https://github.com/Autopilot-Careers/autopilot-careers-community) (branch `main`), subdirectory `packages/agent-marketplace` (which contains `.claude-plugin/plugin.json`). Claude Code sparse-clones only that subdirectory, so install is fast and requires no authentication.

The mirror is kept in sync from the upstream Career on Autopilot project automatically via [Copybara](https://github.com/google/copybara). You install from the mirror; you do not need access to the upstream project.

### Why `git-subdir`?

The [marketplace schema](https://code.claude.com/docs/en/plugin-marketplaces#plugin-sources) defines `git-subdir` for plugins that "live inside a subdirectory of a git repository," using a sparse partial clone to minimize bandwidth for monorepos. That is exactly this plugin's situation (`packages/agent-marketplace` inside the mirror repo), so it is the most correct reference-based source available. We deliberately did **not** use a relative `./` path, because that would require vendoring the plugin files into this repo.

## Codex version

A Codex-compatible build of the plugin is distributed separately as release zip assets named `write-resume-plugin-codex-*.zip` (for example `write-resume-plugin-codex-1.17.13.zip`), not through this marketplace catalog.

## License

[MIT](./LICENSE) © 2026 Autopilot Careers
