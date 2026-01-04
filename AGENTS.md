# Repository Guidelines

## Project Structure & Module Organization
`init.lua` bootstraps Lazy.nvim and wires every module under `lua/`. Core runtime tweaks live in `lua/base` (e.g., `1-options.lua`, `3-autocmds.lua`, `4-mappings.lua`), while shared helpers sit in `lua/base/utils`. Plugin specs are layer-ordered inside `lua/plugins`, so keep new integrations scoped to the file that matches their purpose (behavior, UI, dev tooling, etc.). Version locks live in `lua/lazy_snapshot.lua`, and lint rules in `selene.toml`.

## Build, Test, and Development Commands
- `NVIM_APPNAME=NormalNvim nvim --headless "+Lazy sync" +qa` ensures plugin specs compile cleanly after edits.
- `NVIM_APPNAME=NormalNvim nvim --headless "+Lazy check" +qa` validates missing dependencies or config errors.
- `selene --config selene.toml lua` runs the Lua linter used in CI; add `--display-style=quiet` locally for concise reports.
- For manual runs, start Neovim with `NVIM_APPNAME=NormalNvim nvim` so you load this distro instead of your personal config.

## Coding Style & Naming Conventions
Lua files use two-space indentation, no tabs, and return module tables instead of mutating globals. Keep function names `snake_case`, capitalize tables that behave like classes, and mirror Lazy’s naming (`M.opts`, `M.config`). When adding plugin specs, prefer `keys`, `opts`, and `init` fields over ad-hoc logic in `config`. All user-facing strings should remain English. Run Selene before pushing; suppressions belong near the offending line with `-- selene: allow(...)`.

## Testing Guidelines
Smoke-test every change with `NVIM_APPNAME=NormalNvim nvim --headless "+checkhealth base" +qa` to catch missing binaries. Exercise key workflows in an interactive session: open a project, trigger your feature, then rerun `:messages` for warnings. When touching dev tooling (`lua/plugins/3-dev-core.lua`), run the relevant Neotest suite via `:Neotest run file`. Document platform-specific caveats (Linux, macOS, Termux) in the PR so maintainers can reproduce them.

## Commit & Pull Request Guidelines
Follow the short, imperative style already in history (`Update README.md`, `comments`). Prefix the scope when possible (`plugins: add snacks toggles`). Each PR should include: purpose, affected modules, testing commands/output, and screenshots or gifs for UI changes. Link related issues or roadmap bullets, and note any follow-up work. Keep diffs focused—large refactors belong in separate commits with descriptive body text.

## Security & Configuration Tips
Never commit secrets (API keys, tokens); rely on environment variables such as `OPENAI_API_KEY`. If you add new tooling, mention required packages in the README “dependencies” wiki. When modifying `lazy_versions.lua`, explain why a pin changed so other agents know whether to stay on stable or nightly channels.
