# Upstream

- Source: https://github.com/dmmulroy/anti-slop
- Commit: `13909c9e07590f5a85d93c62712259fe14a883f7`
- Copied from: `skills/install-anti-slop/assets/anti-slop` at that commit (skill folder hash `97962dc9fad565ca7695bf57805d9d0138ce1def`, per `~/.agents/.skill-lock.json`)
- Installed: 2026-11-19 with `scripts/install.mjs`

## Installed plugins

- `tools/oxlint/anti-slop/index.ts`: generic rules, all enabled as `error` in `.oxlintrc.json`
- `tools/oxlint/anti-slop/effect/index.ts`: copied but not registered, because the project does not depend on `effect`

## Deviations

- None in the vendored files.
- `tools/oxlint/package.json` (`"type": "module"`) sits outside this directory so Node loads the plugin as ESM without a warning.
- The directory is excluded from Biome (`biome.json`), TypeScript (`tsconfig.json`), and Fallow (`.fallowrc.json`).
