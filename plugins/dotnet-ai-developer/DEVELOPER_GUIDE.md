# Developer Guide: Extending the .NET AI Developer Plugin

This guide is for developers who want to contribute to, maintain, or extend the .NET AI Developer plugin.

---

## Plugin Architecture

### Directory Structure

```
plugins/dotnet-ai-developer/
├── .github/plugin/
│   └── plugin.json                    # Plugin manifest (entry point)
├── agents/
│   ├── dotnet-ai-developer.agent.md   # Main development agent
│   └── dotnet-ai-reviewer.agent.md    # Code review agent
├── instructions/
│   └── dotnet-ai.instructions.md      # Coding standards & conventions
├── skills/
│   ├── extensions-ai-patterns/
│   │   └── SKILL.md
│   ├── agent-framework-workflows/
│   │   └── SKILL.md
│   ├── ai-observability-dotnet/
│   │   └── SKILL.md
│   ├── ai-safety-guardrails-dotnet/
│   │   └── SKILL.md
│   ├── ai-evals-dotnet/
│   │   └── SKILL.md
│   └── ai-provider-adapters-dotnet/
│       └── SKILL.md
├── README.md                          # Plugin overview
├── GETTING_STARTED.md                 # User documentation
├── DEVELOPER_GUIDE.md                 # This file
└── SKILLS-TODO.md                     # Implementation roadmap
```

### How It Works

1. **plugin.json** declares agents and skills paths to VS Code/Copilot
2. **Agents** (.agent.md files) provide interactive guidance; they can invoke skills and tools
3. **Skills** (.SKILL.md files) provide deep, reference-able patterns for specific topics
4. **Instructions** (.instructions.md files) define coding standards applied to agents/skills

---

## File Formats

### Agent File Format (.agent.md)

```yaml
---
name: agent-name
description: "What the agent does"
applyTo:
  - "**/*.cs"              # File patterns where agent is relevant
  - "**/*.csproj"
tools: [...]              # Optional: restrict to specific tools
---

# Agent Title

Operating instructions, decision heuristics, playbooks, etc.

## When To Use This Agent
(When should a user invoke this agent?)

## Key Workflows
(What can the agent help with?)

## Clarification Protocol
(How does the agent ask follow-up questions?)

## Interaction Style
(Tone, depth, communication preferences)

## Prompt Starters
- "..."
- "..."
```

**Example**: See [agents/dotnet-ai-developer.agent.md](agents/dotnet-ai-developer.agent.md)

---

### Skill File Format (.SKILL.md)

```yaml
---
name: skill-name
description: "What the skill covers; keywords for discovery"
---

# Skill Title

Brief intro.

## When To Use This Skill
(Use-case trigger keywords; helps agents decide when to recommend)

## Initial Workflow
(Step-by-step patterns; often includes code examples)

## Gotchas
(Common pitfalls; debugging tips)

## References
(Links to official docs, examples, related resources)
```

**Example**: See [skills/extensions-ai-patterns/SKILL.md](skills/extensions-ai-patterns/SKILL.md)

---

### Instructions File Format (.instructions.md)

```yaml
---
name: style-name
description: "What coding standards/conventions are defined"
applyTo:
  - "**/*.{cs,csproj}"
---

# Title

## Rule 1: Guideline
(Why this matters; when to apply)

## Rule 2: Guideline
(Examples; anti-patterns)
```

**Example**: See [instructions/dotnet-ai.instructions.md](instructions/dotnet-ai.instructions.md)

---

## Content Quality Standards

### Agent Quality Checklist

- [ ] Name is descriptive (e.g., `dotnet-ai-developer` not `helper`)
- [ ] Description is keyword-dense (includes: framework names, use cases, problem domain)
- [ ] `applyTo` patterns are correct (glob syntax, covers intended files)
- [ ] "When To Use" section is clear (no jargon; examples help)
- [ ] Workflow sections are concrete (step-by-step, not vague)
- [ ] Prompt starters are specific (not generic; user-friendly)
- [ ] No hardcoded secrets or personal info
- [ ] Spell-check and grammar pass

### Skill Quality Checklist

- [ ] Name follows pattern: `{domain}-{technology}` or `{technology}-{pattern}`
- [ ] Description is 1-2 sentences; includes keywords (use "Use when...")
- [ ] "When To Use" section is discovery-friendly (users understand relevance)
- [ ] Code examples compile and follow .NET 8.0+ best practices
- [ ] All external links are valid (tested 2026-05-02 or later)
- [ ] "Gotchas" section covers 3-6 real pitfalls (not obvious stuff)
- [ ] "References" point to authoritative sources (microsoft.com, github.com, official docs)
- [ ] Skill is self-contained (<500 lines recommended; use `references/` subdirectory if longer)

### Instructions Quality Checklist

- [ ] Rules are actionable (not just "be careful")
- [ ] Each rule has "why" + "when to apply" + examples
- [ ] Anti-patterns are called out explicitly
- [ ] Rules align with .NET best practices
- [ ] Rules integrate with agents/skills (cross-references where relevant)

---

## Adding a New Skill

### Step 1: Define the Skill

1. Create directory: `skills/{skill-name}/`
2. Create `SKILL.md` with frontmatter:

```yaml
---
name: my-new-skill
description: 'Concise description with keywords. Use when ...'
---

# My New Skill

(Content)
```

3. Fill in sections: When To Use, Initial Workflow, Gotchas, References

### Step 2: Validate Content

```bash
# Check markdown syntax
markdownlint skills/my-new-skill/SKILL.md

# Validate YAML frontmatter
cat skills/my-new-skill/SKILL.md | head -10  # Should have valid YAML

# Verify links are accessible (manual or script)
grep 'https://' skills/my-new-skill/SKILL.md
```

### Step 3: Link from Agents/Instructions

- Add reference in agent that recommends this skill
- Update `dotnet-ai.instructions.md` if the skill relates to coding standards
- Update [README.md](README.md) skills table

### Step 4: Update Roadmap

Add to [SKILLS-TODO.md](SKILLS-TODO.md):
- Link in Phase where skill was added
- Note key content sections included

### Step 5: Test Discovery

- Verify skill name and description in plugin.json
- Invoke relevant agent and confirm it recommends your skill
- Test manual search in Copilot skill browser

---

## Adding a New Agent

### Step 1: Create Agent File

Create `agents/{agent-name}.agent.md`:

```yaml
---
name: my-new-agent
description: 'What the agent does; keywords for discovery'
applyTo:
  - "**/*.cs"
  - "**/*.md"
---

# My New Agent

## When To Use This Agent
(Describe scenarios and use cases)

## Core Responsibilities
(What this agent will help with)

## Decision Heuristics
(How the agent decides between options)

## Prompt Starters
- "Help me..."
- "Review my..."
```

### Step 2: Reference Skills

In the agent body, mention which skills align:

> When you need deep guidance on X, I'll recommend the `{skill-name}` skill.

### Step 3: Update plugin.json

Add agent to `.github/plugin/plugin.json`:

```json
{
  "agents": [
    {
      "path": "./agents/my-new-agent.agent.md"
    }
  ]
}
```

### Step 4: Update README.md

Add row to "Agents" table in [README.md](README.md)

---

## Maintenance Tasks

### Monthly: Link Validation

```bash
# Check for broken/redirected links
for file in skills/*/SKILL.md agents/*.agent.md instructions/*.instructions.md; do
  grep -o 'https://[^)]*' "$file"
done | sort -u > /tmp/urls.txt

# Manually spot-check a sample of 5-10 URLs
```

**Action**: If link is broken, update to canonical URL or note deprecation in content.

---

### Quarterly: Skill Refresh

For each skill, verify:
- [ ] Latest API docs linked
- [ ] Code examples still compile on .NET 8.0+
- [ ] No deprecated patterns mentioned
- [ ] Gotchas still relevant (no "fixed in vX" without version bump note)

**Action**: If skill is stale, plan refresh for next phase.

---

### Biannually: Agent Competency Review

For each agent, verify it still:
- [ ] Covers intended use cases
- [ ] Recommends current skills
- [ ] Avoids contradicting instructions
- [ ] Handles edge cases (multi-tenant, compliance, etc.)

**Action**: If agent misses key scenario, plan enhancement.

---

## Contributing Guidelines

### Before You Submit

1. **Format Check**
   ```bash
   # Validate YAML frontmatter
   cat your-skill/SKILL.md | head -10
   
   # Check markdown syntax
   npx markdownlint-cli2 your-skill/SKILL.md
   ```

2. **Content Quality**
   - Read the file aloud; does it make sense?
   - Are all code examples runnable?
   - Are all links accessible?
   - Any typos or grammar issues?

3. **Cross-References**
   - Update [README.md](README.md) if needed
   - Add to [SKILLS-TODO.md](SKILLS-TODO.md) if new
   - Link from related agents/skills

4. **Testing**
   - If new skill: verify agents can discover and recommend it
   - If new agent: verify it recommends relevant skills
   - If new instructions: verify agents comply with them

### Submission Checklist

- [ ] All files in correct directories (`agents/`, `skills/`, `instructions/`)
- [ ] YAML frontmatter is valid and keyword-dense
- [ ] Markdown is well-formatted (headers, code blocks, links)
- [ ] No hardcoded secrets, API keys, or personal info
- [ ] All links tested and working
- [ ] Code examples compile and follow .NET 8.0+ patterns
- [ ] Spelling and grammar checked
- [ ] Files committed with clear commit message

---

## Common Patterns

### Pattern: Recommending a Skill from an Agent

```markdown
For patterns on X, see the `skill-name` skill in this plugin.
```

### Pattern: Providing Code Example in a Skill

```markdown
### Example: Setting Up IChatClient

\`\`\`csharp
// Include just enough context (2-3 lines of setup)
var client = new ChatClientBuilder(...)
    .UseInstrumentation()
    .Build();

// Show the key concept
var response = await client.CompleteAsync(messages);
\`\`\`
```

### Pattern: Linking Between Skills

```markdown
For observability integration, see the telemetry schema in `ai-observability-dotnet`.
```

### Pattern: Documenting a Gotcha

```markdown
## Gotchas

- **Never Leak Provider Types**: Don't return `Azure.AI.OpenAI.ChatCompletionOptions` 
  from adapter methods. Always normalize to generic `ChatClientOptions`.
  - Why: Breaks portability; makes switching providers painful
  - Debug: If other code gets compilation errors after adding a new adapter, check for leaked types
  - Fix: Wrap provider type in adapter; return only `ChatClientOptions` or similar abstractions
```

---

## CI/CD Integration (If Publishing)

### GitHub Actions Workflow Example

```yaml
name: Validate Plugin

on: [pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Validate YAML frontmatter
        run: |
          for file in skills/*/SKILL.md agents/*.agent.md instructions/*.instructions.md; do
            python3 -c "import yaml; yaml.safe_load(open('$file'))"
          done
      
      - name: Markdown lint
        run: npx markdownlint-cli2 'skills/*/*.md' 'agents/*.md' 'instructions/*.md'
      
      - name: Check for broken links
        run: |
          # Tool: https://github.com/gaurav-nelson/github-action-markdown-link-check
          npx markdown-link-check --config .github/workflows/mlc-config.json 'skills/**/*.md'
```

---

## Versioning

This plugin uses **semantic versioning** for releases:

- **Major** (X.0.0): Breaking changes (renamed agent/skill, removed functionality)
- **Minor** (1.X.0): New skills or agents added
- **Patch** (1.0.X): Bug fixes, link updates, typo corrections

**Example Changelog Entry:**

```markdown
## [1.2.0] - 2026-06-15

### Added
- New skill: `ai-cost-optimization-dotnet` for tracking and optimizing AI spending

### Fixed
- Corrected broken link in ai-observability-dotnet to Prometheus docs
- Updated OpenAI adapter example to use latest SDK (v2.0+)

### Changed
- Enhanced description of extensions-ai-patterns with more keywords
```

---

## FAQ for Maintainers

**Q: How do I keep skills from becoming too long?**

A: If a skill exceeds ~500 lines, move deep content to a `references/` subdirectory:

```
skills/my-skill/
├── SKILL.md              (compact: 200 lines max, points to references/)
├── references/
│   ├── advanced-patterns.md    (300 lines)
│   └── troubleshooting.md      (200 lines)
```

Then in SKILL.md:

```markdown
## Advanced Patterns

For detailed patterns on X, see [Advanced Patterns](references/advanced-patterns.md).
```

---

**Q: How do I handle contradictions between agents?**

A: Use `dotnet-ai.instructions.md` as source-of-truth. If agents conflict:

1. Update instructions to clarify the rule
2. Update both agents to reference the rule
3. Add a "Clarification Protocol" to agents if there's ambiguity

---

**Q: Should I include Microsoft Agent Framework vs Semantic Kernel content?**

A: This plugin focuses exclusively on **Microsoft Agent Framework** (public preview; successor to Semantic Kernel/AutoGen). If user asks about Semantic Kernel, redirect to official Microsoft docs.

---

**Q: How often should skills be updated?**

A: Quarterly minimum. Triggers for updates:
- New SDK version released (e.g., Extensions.AI 2.0)
- New best practice emerges (e.g., new safety threat)
- Gotcha is no longer relevant (e.g., "fixed in SDK v2")
- External link is broken

---

## Resources

- [Awesome Copilot Repository](https://github.com/github/awesome-copilot)
- [Microsoft Agent Framework Docs](https://learn.microsoft.com/agent-framework/)
- [Microsoft.Extensions.AI Docs](https://learn.microsoft.com/dotnet/ai/microsoft-extensions-ai)
- [VS Code Extension Development](https://code.visualstudio.com/api)

---

**Happy maintaining! 🛠️**
