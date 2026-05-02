# dotnet-ai-developer

- Instruction: `dotnet-architecture-good-practices.instructions.md`

## Implementation Roadmap

All phases (0-6) are **complete** ✅. See [SKILLS-TODO.md](SKILLS-TODO.md) for details on each phase.

### Getting Started

- **First time using this plugin?** → [GETTING_STARTED.md](GETTING_STARTED.md) (10 min read with 3 quick scenarios)
- **Want to extend this plugin?** → [DEVELOPER_GUIDE.md](DEVELOPER_GUIDE.md) (maintenance, contribution guidelines)
- **Need production deployment guidance?** → Invoke `.NET AI Developer` agent with your specific scenario
.NET AI developer plugin focused on building production-grade applications with **Microsoft.Extensions.AI** and **Microsoft Agent Framework**.


## 📖 Documentation

**New to this plugin?** Start here based on your role:

| Role | Documentation | Time |
|------|---------------|------|
| **Developer** building AI features | [GETTING_STARTED.md](GETTING_STARTED.md) | 10 min |
| **Architect** designing multi-provider systems | [GETTING_STARTED.md](GETTING_STARTED.md) + [Skill: ai-provider-adapters-dotnet](skills/ai-provider-adapters-dotnet/SKILL.md) | 30 min |
| **DevOps/SRE** setting up observability | [Skill: ai-observability-dotnet](skills/ai-observability-dotnet/SKILL.md) | 20 min |
| **Security engineer** adding guardrails | [Skill: ai-safety-guardrails-dotnet](skills/ai-safety-guardrails-dotnet/SKILL.md) | 25 min |
| **QA engineer** designing evals | [Skill: ai-evals-dotnet](skills/ai-evals-dotnet/SKILL.md) | 30 min |
| **Maintainer** extending this plugin | [DEVELOPER_GUIDE.md](DEVELOPER_GUIDE.md) | 20 min |

---

## Agents
| Agent | Description |
|-------|-------------|
| `PLUGIN .NET AI Developer` | Expert .NET AI developer for Microsoft.Extensions.AI and Microsoft Agent Framework. Helps design, build, refactor, and review agents/workflows with current Microsoft guidance. |
| `PLUGIN .NET AI Reviewer` | Review-focused agent for .NET AI codebases. Produces structured findings for correctness, reliability, security, observability, and test coverage. |

## Skills

| Skill | Description |
|-------|-------------|
| `extensions-ai-patterns` | Practical implementation patterns for Microsoft.Extensions.AI: IChatClient composition, tool calling, middleware pipelines, resiliency, telemetry, and testing patterns. |
| `agent-framework-workflows` | Patterns for Microsoft Agent Framework workflow design: routing, checkpoints, handoffs, and human-in-the-loop orchestration. |
| `ai-observability-dotnet` | OpenTelemetry and structured logging patterns for model calls, tool calls, and agent workflow spans. |
| `ai-safety-guardrails-dotnet` | Prompt-injection mitigation, tool permission boundaries, output validation, and safe execution defaults for .NET AI apps. |
| `ai-evals-dotnet` | Evaluation and regression testing patterns for prompts, tools, and end-to-end agent scenarios. |
| `ai-provider-adapters-dotnet` | Adapter templates to keep provider SDK details behind `IChatClient`-based boundaries with fallback and resiliency options. |

## Instructions

| File | Applies To | Description |
|------|-----------|-------------|
| `dotnet-ai.instructions.md` | `**/*.{cs,csproj,sln,slnx,json}` | Conventions for .NET AI projects using Microsoft.Extensions.AI and Microsoft Agent Framework, including architecture, resilience, observability, and testing expectations. |

## Recommended Companion Resources

This plugin pairs well with resources from `github/awesome-copilot`:

- Skill: `microsoft-agent-framework`
- Skill: `dotnet-best-practices`
- Skill: `microsoft-code-reference`
- Skill: `microsoft-docs`
- Instruction: `csharp.instructions.md`
- Instruction: `dotnet-architecture-good-practices.instructions.md`

## Implementation Roadmap

Use [SKILLS-TODO.md](SKILLS-TODO.md) to implement and harden these skills step by step.