# Getting Started: .NET AI Developer Plugin

Welcome to the **.NET AI Developer** plugin for GitHub Copilot! This guide will help you get started building production-grade AI applications with Microsoft.Extensions.AI and Microsoft Agent Framework.

---

## What This Plugin Does

The .NET AI Developer plugin provides expert guidance, patterns, and best practices for building reliable, observable, and secure AI-powered features in .NET applications. It includes:

- **2 Specialized Agents**: Development and code review for AI systems
- **6 Reusable Skills**: Deep-dive guidance on extensions.ai, workflows, observability, safety, evals, and provider adapters
- **1 Coding Standards File**: Conventions for .NET AI code quality and reliability

**Who Should Use This?**
- .NET developers building AI-powered features
- Teams deploying to Azure AI Foundry or using Azure OpenAI/OpenAI APIs
- Projects requiring production observability, safety, and testing for AI workloads
- Architects designing multi-provider, multi-tenant AI systems

---

## Quick Start: 3 Common Scenarios

### Scenario 1: "I'm Building My First Chat Feature"

1. **Invoke**: `.NET AI Developer` agent
2. **Say**: "Help me build a chat feature using Extensions.AI. I need provider-agnostic patterns."
3. **Agent Will**: Ask about your provider (Azure OpenAI/OpenAI), then recommend:
   - IChatClient abstraction setup
   - Middleware composition patterns
   - Telemetry wiring
   - Example code for your provider

**Skills Used**: `extensions-ai-patterns`, `ai-provider-adapters-dotnet`

---

### Scenario 2: "I Need to Know If My AI Feature Is Production-Ready"

1. **Invoke**: `.NET AI Reviewer` agent
2. **Share**: Your PR changes or chat feature code
3. **Agent Will**: Check for:
   - ✅ Proper error handling and timeouts
   - ✅ Observable traces and metrics
   - ✅ Safety guardrails (input validation, output validation)
   - ✅ Test coverage
   - ✅ Vulnerability patterns

**Skills Used**: `ai-observability-dotnet`, `ai-safety-guardrails-dotnet`, `ai-evals-dotnet`

---

### Scenario 3: "I Need a Multi-Step Approval Workflow"

1. **Say**: "Help me design a workflow where AI classifies orders, then routes to approval if risky."
2. **Agent Will**: Provide:
   - Routing graph definition
   - Checkpoint/resume pattern for human handoffs
   - Error recovery logic
   - Observability integration

**Skills Used**: `agent-framework-workflows`, `ai-observability-dotnet`

---

## Meet the Agents

### 1. .NET AI Developer (`dotnet-ai-developer`)

**When to invoke:**
- Building or enhancing AI features
- Designing multi-step workflows
- Choosing provider architectures (multi-provider, fallback strategies)
- Integrating with existing .NET codebases

**How the agent works:**
- Asks clarifying questions (provider, use case, scale, compliance)
- Recommends architectural patterns
- Provides code examples and references
- Offers playbooks for common tasks (migrations, new features, scaling)

**Example prompts:**
- "Design a RAG pipeline using Extensions.AI and local models"
- "Help me migrate from Azure OpenAI to provider-agnostic code"
- "I need a multi-model evaluation strategy for my chatbot"

---

### 2. .NET AI Reviewer (`dotnet-ai-reviewer`)

**When to invoke:**
- After writing AI integration code
- Before submitting a pull request
- After adding new prompts or tools
- To catch reliability/security/observability gaps

**How the agent works:**
- Reviews code for correctness, reliability, security, observability, testing
- Reports issues by severity (Critical → Warning → Suggestion)
- Provides fixes with explanations
- Validates against coding standards

**Example prompts:**
- "Review my chat client setup for reliability patterns"
- "Check my tool definitions for least-privilege permissions"
- "Ensure my workflow has proper error handling"

---

## Meet the Skills

Each skill provides deep, actionable guidance on a specific domain. Use skills when you need step-by-step patterns, code templates, or troubleshooting help.

### Skill 1: `extensions-ai-patterns`
**What**: Foundational patterns for Microsoft.Extensions.AI

**Use when:**
- Setting up IChatClient abstractions
- Adding function/tool calling
- Composing middleware pipelines (retries, caching, telemetry)
- Testing AI integrations

**Key takeaways:**
- Keep app code dependent on IChatClient, not provider SDKs
- Compose behavior with middleware instead of ad-hoc logic
- Always propagate CancellationToken through async boundaries
- Use structured logging and correlation IDs

---

### Skill 2: `agent-framework-workflows`
**What**: Designing deterministic multi-step orchestration with Agent Framework

**Use when:**
- Building approval workflows or multi-step business processes
- Implementing agent routing or handoffs
- Adding pause/resume checkpoints (human-in-the-loop)
- Defining clear success/failure paths

**Key takeaways:**
- Define clear input/output contracts for each workflow step
- Use explicit routing conditions (avoid hiding logic in prompts)
- Save checkpoints before human handoffs
- Handle every branch with explicit error handling

**Example workflow**: Order Classification → Risk Assessment → [Auto-Approve] or [Request Approval] → Execute

---

### Skill 3: `ai-observability-dotnet`
**What**: Production observability using OpenTelemetry and structured logging

**Use when:**
- Need visibility into model latency, failures, token usage, cost
- Debugging AI system behavior in production
- Setting up dashboards and alerts
- Tracking correlation IDs across async boundaries

**Key takeaways:**
- Define telemetry schema (trace names, tags, metrics) upfront
- Instrument model and tool call boundaries
- Use safe logging (redact prompts/responses by default)
- Set alert thresholds for latency, errors, cost anomalies

**Example metrics**: `ai.model.latency_ms`, `ai.model.cost_usd`, `ai.safety.events.total`

---

### Skill 4: `ai-safety-guardrails-dotnet`
**What**: Defense-in-depth safety patterns (threat modeling, input sanitization, output validation)

**Use when:**
- User input can influence prompts or tool parameters
- Tools execute side effects (database writes, API calls)
- Sensitive data (PII, financial, healthcare) is involved
- Regulatory compliance (GDPR, industry standards) applies

**Key takeaways:**
- Model threats upfront: prompt injection, hallucination, lateral movement
- Sanitize user input (escape, length limits, pattern detection)
- Validate model output (guardrails, policy checks) before action
- Enforce least-privilege tool execution (field/scope restrictions)
- Log all guardrail events for audit trails

**Defense layers**: Input → Model → Output → Tool Execution

---

### Skill 5: `ai-evals-dotnet`
**What**: Systematic evaluation and regression testing with repeatable scorecards

**Use when:**
- Validating prompt quality before deployment
- Detecting performance regressions in CI
- Comparing across models/providers (e.g., GPT-4 vs Claude)
- Gating release readiness on objective thresholds

**Key takeaways:**
- Define quality dimensions (accuracy, safety, latency, cost, robustness)
- Version datasets for reproducibility
- Automate evals in CI/CD (run on every PR)
- Compare to baseline to detect regressions
- Report results in PR comments for transparency

**Example scorecard**: Accuracy 95% ✅, Safety 99.5% ✅, Latency p95 1.2s ✅

---

### Skill 6: `ai-provider-adapters-dotnet`
**What**: Multi-provider adapter patterns (Azure OpenAI, OpenAI, Anthropic, Ollama, local)

**Use when:**
- Switching providers without changing app code
- Implementing failover/fallback strategies
- Supporting multi-tenant setups with provider variance per tenant
- Normalizing error handling across providers

**Key takeaways:**
- Define common `IChatProviderAdapter` interface
- Implement adapter for each provider (wraps SDK)
- Don't leak provider-specific types outside adapter boundary
- Use MultiProviderAdapter for failover (try primary, fall back to secondary)
- Test adapter contract conformance

**Benefit**: Swap `AzureOpenAiAdapter` for `OpenAiAdapter` with zero app-code changes

---

## Integration Guide: How the Skills Work Together

```
┌─────────────────────────────────────────────────────────────────┐
│                    Your .NET AI Application                     │
└────────┬────────────────────────────────────────────────────────┘
         │
         ├─→ [extensions-ai-patterns]
         │   ├─ IChatClient abstraction
         │   ├─ Tool/function calling
         │   └─ Middleware composition
         │
         ├─→ [ai-provider-adapters-dotnet]
         │   ├─ Azure OpenAI adapter
         │   ├─ OpenAI adapter
         │   ├─ Fallback logic
         │   └─ Error normalization
         │
         ├─→ [ai-observability-dotnet]
         │   ├─ OpenTelemetry traces
         │   ├─ Structured logs
         │   ├─ Metrics (latency, cost)
         │   └─ Alerts + runbooks
         │
         ├─→ [ai-safety-guardrails-dotnet]
         │   ├─ Input sanitization
         │   ├─ Output validation
         │   ├─ Least-privilege tools
         │   └─ Audit logging
         │
         ├─→ [agent-framework-workflows]
         │   ├─ Routing logic
         │   ├─ Human handoffs
         │   ├─ Checkpoint recovery
         │   └─ Error handling
         │
         └─→ [ai-evals-dotnet]
             ├─ Regression testing
             ├─ Scorecard generation
             ├─ CI automation
             └─ PR reporting
```

### Example Integration: Order Processing Workflow

```csharp
// 1. Set up provider-agnostic client (extensions-ai-patterns)
var client = new ChatClientBuilder(new OpenAiAdapter(...))
    .UseInstrumentation()           // ai-observability-dotnet
    .UseGuardrails(...)             // ai-safety-guardrails-dotnet
    .Build();

// 2. Define workflow with routing (agent-framework-workflows)
var workflow = new OrderProcessingWorkflow(client, toolProvider, logger);

// 3. Execute with checkpoints and observability
var result = await workflow.ExecuteAsync(order);

// 4. Evaluate quality (ai-evals-dotnet)
var scorecard = await evalEngine.EvaluateAsync(
    dataset,
    model: "gpt-4",
    provider: "openai",
    baseline: previousScorecard);

// 5. Report to PR (ai-evals-dotnet)
var report = reporter.GenerateReport(evalResult);
```

---

## Coding Standards

All code in this plugin follows these conventions (defined in `dotnet-ai.instructions.md`):

1. **Abstraction First**: IChatClient as application boundary; provider SDKs hidden
2. **Async/Await Throughout**: All I/O operations must be async
3. **CancellationToken Propagation**: Pass through all async call chains
4. **Timeout Handling**: Set explicit timeouts on all external calls
5. **Structured Logging**: Use ILogger with correlation IDs
6. **No Hardcoded Secrets**: Use configuration/keyvault
7. **Test at Abstraction Boundary**: Mock IChatClient for unit tests

---

## Common Workflows

### Workflow A: Add Chat Feature to Existing ASP.NET Core App

1. **Agent**: Invoke `.NET AI Developer` → "Add a chat endpoint using Azure OpenAI"
2. **Skill**: Follow `extensions-ai-patterns` for middleware setup
3. **Skill**: Follow `ai-observability-dotnet` to wire traces/metrics
4. **Skill**: Follow `ai-safety-guardrails-dotnet` to add input validation
5. **Review**: Invoke `.NET AI Reviewer` on your PR

**Timeline**: 30 mins (with copilot assistance)

---

### Workflow B: Migrate to Provider-Neutral Code

1. **Agent**: "Help me refactor my Azure OpenAI code to be provider-agnostic"
2. **Skill**: Study `ai-provider-adapters-dotnet` for adapter pattern
3. **Skill**: Review `extensions-ai-patterns` for middleware composition
4. **Implement**: Create adapters for Azure, OpenAI, other providers
5. **Test**: Use `ai-evals-dotnet` to verify behavior across providers

**Timeline**: 2-3 days (depends on codebase size)

---

### Workflow C: Design Approval Workflow

1. **Agent**: "Design a workflow where AI assesses risk, then routes to human approval if needed"
2. **Skill**: Study `agent-framework-workflows` for routing/checkpoint patterns
3. **Skill**: Integrate `ai-observability-dotnet` for workflow tracing
4. **Skill**: Add `ai-safety-guardrails-dotnet` for suspicious activity detection
5. **Review**: Use `.NET AI Reviewer` to validate error handling

**Timeline**: 1-2 days (design + implementation)

---

### Workflow D: Set Up Regression Testing

1. **Agent**: "How do I gate releases on AI quality?"
2. **Skill**: Follow `ai-evals-dotnet` for scorecard and dataset setup
3. **Skill**: Create evaluation suite (happy path, edge cases, adversarial)
4. **Skill**: Integrate CI (GitHub Actions) to run evals on every PR
5. **Skill**: Report results in PR comments

**Timeline**: 3-5 days (initial setup + tuning)

---

## Troubleshooting

### Issue: "How do I choose between Azure OpenAI and OpenAI?"

**Answer**: Use `ai-provider-adapters-dotnet` to stay provider-neutral. See:
- Use Azure if: Enterprise contracts, HIPAA compliance, regional requirements
- Use OpenAI if: Cost-sensitive, need latest models first, simpler billing
- Use `MultiProviderAdapter` if: Need fallback/failover for HA

---

### Issue: "My model responses are hallucinating—how do I validate output?"

**Answer**: See `ai-safety-guardrails-dotnet`:
1. Use `OutputValidator` to check for invalid tool calls
2. Implement `DataClassifier` to detect sensitive data
3. Add explicit parsing/validation before downstream action
4. Log all rejections for audit

---

### Issue: "How do I debug latency spikes in production?"

**Answer**: Use `ai-observability-dotnet`:
1. Check OpenTelemetry traces for bottleneck (model? network? tool?)
2. Review structured logs with correlation ID
3. Check metrics: `ai.model.latency_ms` histogram
4. Alert on p95 latency exceeding threshold
5. Use runbook to investigate (model availability? queue? timeout?)

---

### Issue: "How do I ensure my workflow doesn't get stuck?"

**Answer**: Use `agent-framework-workflows`:
1. Add explicit timeout to all waits (e.g., `WaitForApprovalAsync(..., timeout: TimeSpan.FromHours(4))`)
2. Save checkpoints before handoffs so workflow can resume
3. Monitor workflow duration; alert on p95 exceeding SLA
4. Log every step transition for audit trail
5. Test recovery by simulating crashes at each checkpoint

---

### Issue: "My evals take too long to run on every PR—how do I optimize?"

**Answer**: Use `ai-evals-dotnet`:
1. Run fast evals on PR (subset of dataset, cheaper model)
2. Run full evals on main branch (comprehensive suite, production model)
3. Use cached results from baseline when possible
4. Parallelize eval execution (run multiple test cases concurrently)
5. Consider 30-min timeout for PR evals, fail fast if threshold breached

---

## Recommended Reading Order

Start here:
1. **[README.md](README.md)** — Overview of agents, skills, and references
2. **[SKILLS-TODO.md](SKILLS-TODO.md)** — Implementation roadmap (shows what each phase covers)

Then dive into skills based on your use case:

- **Chat Features?** → `extensions-ai-patterns` → `ai-provider-adapters-dotnet` → `ai-observability-dotnet`
- **Workflows?** → `agent-framework-workflows` → `ai-safety-guardrails-dotnet` → `ai-observability-dotnet`
- **Quality Gates?** → `ai-evals-dotnet` → `ai-safety-guardrails-dotnet`
- **Production Hardening?** → `ai-observability-dotnet` → `ai-safety-guardrails-dotnet` → `ai-evals-dotnet`

---

## Next Steps

1. **Pick a Scenario** from "Quick Start: 3 Common Scenarios" above
2. **Invoke an Agent** (`.NET AI Developer` or `.NET AI Reviewer`)
3. **Ask a Question** specific to your use case
4. **Follow the Recommendations** and use skills for deep dives
5. **Review Your PR** with `.NET AI Reviewer` before shipping

---

## FAQ

**Q: Do I have to use Azure OpenAI?**
A: No. This plugin supports Azure OpenAI, OpenAI, Anthropic, Ollama (local), and any openai-compatible API. See `ai-provider-adapters-dotnet`.

**Q: Can I use this for non-Azure environments?**
A: Yes. The plugin is provider-agnostic by design. All patterns work with any provider. See `ai-provider-adapters-dotnet` and `extensions-ai-patterns`.

**Q: Does this cover Semantic Kernel or AutoGen?**
A: No. This plugin focuses exclusively on **Microsoft Agent Framework** (successor to Semantic Kernel) and **Microsoft.Extensions.AI** abstractions. For Semantic Kernel, refer to official Microsoft docs.

**Q: What .NET versions are supported?**
A: .NET 8.0 LTS and later. All code examples use modern C# 12+ features (async/await, records, pattern matching, nullable reference types).

**Q: How do I contribute improvements to this plugin?**
A: This is a public plugin. Report issues, request skills, or submit PRs to the [awesome-copilot repository](https://github.com/github/awesome-copilot).

---

## Need Help?

- **Agent Questions?** Ask `.NET AI Developer` or `.NET AI Reviewer` directly
- **Skill Deep Dives?** Open each `SKILL.md` file under `skills/`
- **Code Examples?** Each skill includes runnable C# snippets
- **Microsoft Docs?** See "References" section in each skill for official guidance
- **This Plugin Issues?** Check [Awesome Copilot GitHub](https://github.com/github/awesome-copilot)

---

**Happy building! 🚀**
