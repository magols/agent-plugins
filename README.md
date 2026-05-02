# agent-plugins

Private VS Code Agent Plugins focused on **C# and Blazor** development.

## Available Plugins

| Plugin | Description |
|--------|-------------|
| [blazor-dev](./plugins/blazor-dev/) | Scaffolding and component generation for Blazor + C# projects |
| [csharp-code-review](./plugins/csharp-code-review/) | PR review and security review for C# and Blazor codebases |
| [fullstack-hybrid-developer](./plugins/fullstack-hybrid-developer/) | Full-stack hybrid developer agent with bundled Optimizely CMS and Radzen Blazor instructions |

## Installing in VS Code

1. Open VS Code Insiders
2. Open the Copilot Chat panel
3. Navigate to **Agent Plugins** and add this repository as a source: `magols/agent-plugins`
4. Install the desired plugins

## Structure

Each plugin lives under `plugins/<name>/` and follows the VS Code Agent Plugin convention:

```
plugins/<name>/
├── .github/plugin/plugin.json   # Plugin manifest
├── agents/                      # *.agent.md — Copilot Chat agents
├── skills/                      # Subdirs with SKILL.md — invokable skills
├── instructions/                # *.instructions.md — auto-applied guidelines
└── README.md
```
