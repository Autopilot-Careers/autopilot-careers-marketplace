# Autopilot Careers — Claude plugin marketplace

A public [Claude Code / Claude Desktop plugin marketplace](https://code.claude.com/docs/en/plugin-marketplaces) for the **Career on Autopilot** project. It catalogs the `write-resume-plugin`, which adds resume writing, job analysis, and career planning tools to Claude.

This repository contains only the marketplace catalog (`.claude-plugin/marketplace.json`). It does **not** vendor the plugin source — the entry *references* the plugin in its home repository (see [Prerequisites](#prerequisites--making-it-functional)).

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

## Prerequisites / making it functional

> **Important:** Adding the marketplace will succeed (this repo is public), but **installing the plugin will not fully resolve yet.** The marketplace entry references the plugin source by reference rather than copying it in, and that source is not yet publicly reachable.

The plugin source currently lives in a **private** monorepo, `alc0der/autopilot-careers`, under the subdirectory `packages/agent-marketplace` (which contains `.claude-plugin/plugin.json`). The marketplace entry uses a `git-subdir` source pointing at that repo + path. No GitHub releases have been published yet.

For `/plugin install write-resume-plugin@autopilot-careers` to resolve for the public, **one** of the following must happen:

1. **Make the source repo public** — publish `alc0der/autopilot-careers` (or at least the `packages/agent-marketplace` subdirectory in a public repo). The current `git-subdir` source resolves the moment the repo is reachable, because Claude Code sparse-clones only that subdirectory. *No marketplace change required.*

   *or*

2. **Keep the repo private but grant access** — private repositories are supported. Users authenticate with their git credential helper (for example `gh auth login`), and `GITHUB_TOKEN` / `GH_TOKEN` enables background auto-updates. Each user still needs read access to `alc0der/autopilot-careers`.

   *or*

3. **Publish a public plugin repo / releases** — move the plugin into a dedicated public repo (e.g. `Autopilot-Careers/write-resume-plugin`) and switch the marketplace `source` to a `github` source (`{ "source": "github", "repo": "Autopilot-Careers/write-resume-plugin" }`). Pin releases with `ref`/`sha` as needed.

Until one of the above is done, the marketplace lists the plugin but install will fail to fetch the source.

### Why `git-subdir`?

The [marketplace schema](https://code.claude.com/docs/en/plugin-marketplaces#plugin-sources) defines `git-subdir` for plugins that "live inside a subdirectory of a git repository," using a sparse partial clone to minimize bandwidth for monorepos. That is exactly this plugin's situation (`packages/agent-marketplace` inside a larger monorepo), so it is the most correct reference-based source available. We deliberately did **not** use a relative `./` path, because that would require vendoring the plugin files into this repo.

## Codex version

A Codex-compatible build of the plugin is distributed separately as release zip assets named `write-resume-plugin-codex-*.zip` (for example `write-resume-plugin-codex-1.17.13.zip`), not through this marketplace catalog.

## License

[MIT](./LICENSE) © 2026 Autopilot Careers
