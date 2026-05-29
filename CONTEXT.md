# PowerShell & Azure Dev Container Template

A single Dev Container Template published to the OCI registry at ghcr.io/thecloudexplorers/devcontainer/azure-engineering. It bundles PowerShell, Azure CLI, Bicep, GitHub CLI, Claude Code, and draw.io into a ready-to-use development environment.

## Language

**Template**:
The distributable unit — a folder containing `devcontainer-template.json` and a `.devcontainer/` configuration — published to an OCI registry and applied by supporting tools (VS Code, GitHub Codespaces).
_Avoid_: image, container, devcontainer (too broad)

**devcontainer-template.json**:
The metadata manifest required at the root of every Template. Declares `id`, `version`, `name`, `description`, and optional discovery metadata.
_Avoid_: config file, manifest (ambiguous with OCI manifest)

**Publisher**:
The GitHub organisation (`thecloudexplorers`) that owns, maintains, and publishes the Template to ghcr.io.
_Avoid_: author, owner, maintainer

**Toolset**:
The specific set of tools baked into this Template's image: PowerShell, Azure CLI, Bicep, GitHub CLI, Claude Code CLI, draw.io, Az PowerShell modules, PSRule.Rules.Azure.
_Avoid_: dependencies, tools, stack

**Template Option**:
A user-configurable parameter declared in `devcontainer-template.json` and substituted via `${templateOption:optionId}` placeholders at apply time. This Template currently defines no options.
_Avoid_: parameter, variable, argument

## Example dialogue

> "Should we add a Node version option?"
> "Not yet — the Toolset pins Node 22 because Claude Code requires it. A Template Option would let users override it, but there's no use case for that today."
