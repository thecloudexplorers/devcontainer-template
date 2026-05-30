# PowerShell & Azure Dev Container Template

A Dev Container Template published to the OCI registry at The Cloud Explorers GitHub Container Registry.

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

## Skills

**Skill**:
A named agent behavior module — a directory containing a `SKILL.md` (the instruction set) and optional supporting files — invokable as a slash command in Claude Code or a compatible agent runner.
_Avoid_: plugin, command, tool, macro

**Skill Library**:
The complete set of Skills shipped with this Template, stored at two locations: `.agents/skills/` (agent-runner agnostic) and `.claude/skills/` (Claude Code specific). Both locations mirror the same Skills.
_Avoid_: plugin library, command set

## Container Configuration

### engineering-tools Template

The `engineering-tools` Template is published to the OCI registry at `ghcr.io/thecloudexplorers/devcontainer-template/engineering-tools:latest`. It bundles PowerShell, Azure CLI, Bicep, GitHub CLI, Claude Code, draw.io, and a Skill Library into a ready-to-use development environment.

**Container Configuration**:
The `.devcontainer/` folder that defines the Template's runtime: `Dockerfile` (system-level installs — tools, CLI binaries, PowerShell modules), `devcontainer.json` (VS Code settings, extensions, lifecycle hooks), and `install.ps1` (post-create user-level terminal setup).
_Avoid_: devcontainer, container setup

**Template Source**:
The `src/engineering-tools/` directory — the publishable unit containing `devcontainer-template.json` and the Container Configuration. This is what gets packaged and pushed to the OCI registry.
_Avoid_: source folder, package directory

### Example dialogue

> "Should we add a Node version option?"
> "Not yet — the Toolset pins Node 22 because Claude Code requires it. A Template Option would let users override it, but there's no use case for that today."

> "Where do new Skills live?"
> "In the Skill Library — both under `.agents/skills/` and `.claude/skills/` inside the Template Source. They're mirrored so the same Skills work whether you're running Claude Code or another agent runner."

> "What's the difference between the Dockerfile and install.ps1?"
> "The Dockerfile owns all system-level installs — CLI tools, PowerShell modules, draw.io. The `install.ps1` runs post-create as the `vscode` user for personal terminal config. That split keeps system state in the image and user state out of it."
