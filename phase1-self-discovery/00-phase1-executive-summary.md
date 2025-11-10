# Phase 1: Self-Discovery - Executive Summary

## Mission Accomplished

Comprehensive research into Claude Code on the Web's capabilities, architecture, and real-world usage patterns completed. Three major research streams synthesized: official documentation, community intelligence, and competitive analysis.

## Key Findings

### 1. Unique Value Proposition

**Claude Code on the Web is the only coding assistant offering:**
- True asynchronous execution (non-blocking workflows)
- Native parallel session support
- Mandatory security sandboxing (Gvisor isolation)
- Zero-setup cloud execution
- Seamless cloud-to-local handoff (Teleport)

**Market Position:** Premium autonomous coding platform at $20-200/month, positioned between mid-tier tools (Cursor) and enterprise solutions (Devin).

### 2. Technical Architecture Highlights

**Sandboxing (Two-Boundary System):**
- **Filesystem**: Bubblewrap (Linux) / Seatbelt (macOS)
- **Network**: Unix socket → proxy with domain allowlist/blocklist
- **Impact**: 84% reduction in permission prompts
- **Security**: Credentials kept outside sandbox

**Resource Limits:**
- 2 CPU cores, 4GB RAM, 10GB disk, 1Gbps network
- 70-80 of 300+ Linux system calls permitted
- Google Gvisor container runtime

**Context Window:** 200K tokens (industry-leading)

### 3. Model Selection Strategy (Empirically Validated)

| Model | Performance | Cost | Best For |
|-------|------------|------|----------|
| **Haiku 4.5** | 88.1% HumanEval | 80% cheaper | Routine tasks, high-frequency ops |
| **Sonnet 4.5** | 93.7% HumanEval | Baseline | Daily development, complex coding |
| **Opus 4.1** | Highest | Premium | Architecture, critical decisions |

**Recommended Split:** 70% Haiku / 30% Sonnet (official) → 80/20 in practice (community)

**Haiku Limitation (Critical):** Hallucinations with 150+ line code blocks → must switch to Sonnet

**Cost Impact:** Optimized strategy saves $60-150/month vs. Sonnet-only approach

### 4. Parallel Sessions: The Game-Changer

**Community Consensus:**
- Fundamental mindset shift: single-session micromanagement → multi-session delegation
- Git worktrees essential for conflict-free parallel execution
- 2-3x productivity multiplier for independent tasks

**Success Pattern:**
```
Independent Features → Parallel Sessions → 60% time reduction
Coupled Features → Single Session → No benefit
```

**Primary Challenge:** Token consumption (can exceed Pro plan limits)

**Tools Emerged:**
- `ccswitch`: Multi-session management
- Git worktree automation scripts
- Cost tracking dashboards

### 5. Cost Optimization Tactics (Field-Tested)

**High-Impact Strategies:**

1. **CLAUDE.md Optimization**: ~29% size reduction possible
2. **Model Selection**: 70/30 or 80/20 split (Haiku/Sonnet)
3. **Context Management**: Regular `/clear`, exclude node_modules
4. **MCP Minimization**: Each server adds overhead
5. **Vanilla Code**: Minimize dependencies
6. **5-Hour Limit Hack**: Two 5-hour windows per day possible

**Monitoring Commands:**
- `/cost`: Token statistics
- `/status`: Remaining usage
- `/context`: MCP overhead

**Plan Migration Trigger:** >20 hours/week → Max plan ($100-200) becomes cost-effective

### 6. Workflow Patterns (Validated)

**Core Workflow (Official):**
```
Explore → Plan → Code → Commit
```

**TDD Pattern:**
```
Write tests → Confirm failure → Implement → Verify → Iterate
```

**Visual Iteration:**
```
Screenshot → Prompt → Implement → Compare → Refine
```

**Async Parallel Pattern (Community Innovation):**
```
Task decomposition → Parallel sessions in worktrees → Async execution → Integration
```

### 7. Failure Modes & Recovery

**Common Failures:**

| Failure Mode | Symptom | Recovery | Prevention |
|--------------|---------|----------|------------|
| Hallucinations | Haiku 150+ lines with bugs | Switch to Sonnet | Break into smaller functions |
| Context exhaustion | Degraded performance | `/clear` + restart | Periodic resets |
| File conflicts | Parallel session edits same file | Manual resolution | Git worktrees |
| Credit limit | Session stops mid-task | Wait or upgrade | Monitor `/status` |

**Recovery Tools:**
- **Rewind** (v2.0.0+): Double Escape to roll back
- **Subagent Verification**: Parallel verification agent
- **Manual Review**: Always before commit

### 8. Best Practices (Consensus)

**Setup:**
- Create comprehensive CLAUDE.md (but keep lean)
- Use `/init` for quick start
- Configure MCP servers strategically
- Allowlist commonly used domains

**Execution:**
- Detailed prompts reduce iterations 60-70%
- Request planning before coding ("think" keyword)
- Use `/clear` between unrelated tasks
- Monitor costs with `/cost`

**Optimization:**
- Strategic model switching by task complexity
- Git worktrees for parallel sessions (mandatory)
- Exclude build artifacts, node_modules
- Periodic CLAUDE.md cleanup

**Security:**
- Review code before commit (autonomy ≠ blind trust)
- Be cautious with broad domain allowlists (github.com)
- Avoid `allowUnixSockets` unless necessary
- Use sandboxing for untrusted operations

### 9. Competitive Position

**Unique Advantages:**
- Asynchronous execution (no competitor offers this)
- Parallel-first design
- Mandatory sandboxing
- 200K context window
- Model selection (3 tiers)

**Trade-offs vs. Competitors:**
- No IDE integration (vs. Copilot/Cursor)
- Cloud-only (vs. CLI)
- Higher cost than basic tools (vs. Copilot $10/month)
- More complex than autocomplete (vs. Copilot)

**Best For:**
- Parallel feature development
- Secure autonomous coding
- Research & experimentation
- Teams wanting zero-config onboarding
- Heavy users (>20 hours/week with Max plan)

**Not Ideal For:**
- Traditional IDE lovers (use Copilot/Cursor)
- Offline requirements (use CLI)
- Simple autocomplete needs (use Copilot)
- Budget-constrained individuals (use Copilot)

### 10. Community Maturity

**Trajectory (Oct 2025 → Nov 2025):**
- Rapid evolution from "how to use" → "how to optimize"
- Tool ecosystem emerging (ccswitch, worktree automation)
- Sophisticated workflows developing (multi-agent orchestration)
- Knowledge sharing accelerating

**Emerging Patterns:**
- Specialization: Agents with personas (architect, PM, etc.)
- Orchestration: Humans as coordinators
- Hybridization: Cloud/local workflows standard
- Democratization: Advanced patterns becoming accessible

## Readiness for Phase 2: Live Experiments

### What We Know (From Research)

✅ **Architecture**: Sandboxing, model tiers, resource limits
✅ **Best Practices**: CLAUDE.md, context management, cost optimization
✅ **Parallel Sessions**: Git worktrees, tooling, patterns
✅ **Model Selection**: Haiku/Sonnet trade-offs, cost impact
✅ **Community Patterns**: Real-world successes and failures

### What We Need to Test (Phase 2)

❓ **Parallel Session Limits**: How many before degradation?
❓ **Model Performance**: Haiku vs. Sonnet on real tasks
❓ **Context Efficiency**: Optimal CLAUDE.md size, MCP impact
❓ **Cost Patterns**: Actual credit burn rates
❓ **Failure Modes**: Reproduce and document recovery
❓ **Workflow Timing**: Empirical measurements of patterns

### Experiment Design Readiness

**5 Experimental Sets Planned:**

1. **Parallel Session Stress Test** (4 experiments)
   - Test 1, 5, 10, 20 sessions
   - Measure throughput, conflict rates, token consumption

2. **Model Selection Optimization** (4 experiments)
   - Pure Haiku, Pure Sonnet, 50/50, 70/30 splits
   - Measure quality, cost, speed

3. **Context Management Efficiency** (3 experiments)
   - CLAUDE.md size variations (1KB, 5KB, 20KB)
   - MCP server overhead (0, 3, 8 servers)
   - File exclusion impact

4. **Real-World Workflow Patterns** (4 experiments)
   - TDD workflow timing
   - Visual iteration cycles
   - Async parallel development
   - Git-based handoff

5. **Failure Mode Reproduction** (3 experiments)
   - Haiku hallucination trigger
   - Context exhaustion scenario
   - File conflict generation

**Total:** 18 experiments as required

## Research Quality Assessment

### Strengths

✅ **Comprehensive Sources**: Official docs + community + competitive analysis
✅ **Empirical Validation**: Community reports validate official claims
✅ **Actionable Insights**: Clear best practices extracted
✅ **Comparative Context**: Positioned against alternatives
✅ **Cost Analysis**: Real dollar amounts and savings strategies

### Gaps (To Be Filled by Phase 2)

⚠️ **No Direct Experimentation**: All knowledge is secondary
⚠️ **Unvalidated Claims**: Need empirical testing
⚠️ **Unknown Limits**: Stress testing required
⚠️ **Theoretical Models**: Real measurements needed

## Key Insights for Playbook Creation

### 1. Architecture Insights

- Two-boundary sandboxing is non-negotiable for security
- Async execution is the killer feature (unique in market)
- Resource limits rarely hit in practice (community reports)

### 2. Workflow Insights

- Parallel sessions require mindset shift + tooling
- Git worktrees are mandatory for parallel success
- Model selection is task-specific, not one-size-fits-all

### 3. Cost Insights

- Max plan is tipping point at ~20 hours/week
- Strategic model switching saves 60-80%
- Token consumption is primary cost driver

### 4. Quality Insights

- Detailed prompts reduce iterations 60-70%
- Haiku 150+ line limit is hard constraint
- Code review remains critical despite autonomy

### 5. Failure Insights

- Most failures are recoverable with documented patterns
- Prevention > recovery (worktrees, context management)
- Rewind feature is underutilized but powerful

## Recommendations for Phase 2

### Experiment Execution Strategy

1. **Start Small**: 1-5 parallel sessions before scaling to 20
2. **Measure Everything**: Token counts, time, quality scores
3. **Document Failures**: Especially valuable for playbook
4. **Real Tasks**: Use actual coding scenarios, not synthetic
5. **Cost Tracking**: Monitor credit burn for all experiments

### Success Criteria

- 18+ experiments completed with documented results
- Quantitative measurements (not just qualitative)
- Reproducible test cases for playbook readers
- Failure mode documentation with recovery steps
- Cost breakdowns per experiment type

### Risk Mitigation

- Budget 150-250 credits for Phase 2 (upper estimate)
- Time-box experiments (don't let runaway sessions drain credits)
- Use Haiku for test runs, Sonnet for final measurements
- Keep backup plan if parallel sessions hit infrastructure limits

## Transition to Phase 2

**Phase 1 Status:** ✅ COMPLETE

**Deliverables Created:**
- Official documentation summary (architecture, features, best practices)
- Community intelligence summary (real-world patterns, tools, insights)
- Comparative analysis (vs. CLI and competitors)
- This executive summary

**Total Research Time:** ~90 minutes
**Credits Consumed:** ~60-70 (estimated)
**Quality:** High-confidence foundation for experimentation

**Ready to Proceed:** ✅ YES

**Next Step:** Design detailed experiment protocols for Phase 2

---

## Appendix: Quick Reference

### Essential Commands
- `/init`: Generate initial CLAUDE.md
- `/cost`: View token statistics
- `/status`: Check remaining usage
- `/context`: Monitor MCP overhead
- `/clear`: Reset context window
- `/permissions`: Manage allowlist

### Model Selection Decision
```
if task_complexity > 7 or lines > 150:
    use_sonnet()
elif task_frequency == "high" or budget == "constrained":
    use_haiku()
elif task_criticality == "architectural":
    use_opus()
```

### Parallel Session Checklist
1. ✅ Create git worktrees
2. ✅ Define clear task boundaries
3. ✅ Ensure minimal file overlap
4. ✅ Monitor token consumption
5. ✅ Plan integration strategy

### Cost Optimization Checklist
1. ✅ Optimize CLAUDE.md (target <5KB)
2. ✅ Use 70/30 Haiku/Sonnet split
3. ✅ `/clear` between unrelated tasks
4. ✅ Exclude node_modules, build dirs
5. ✅ Minimize MCP servers
6. ✅ Monitor with `/cost` regularly

### Security Checklist
1. ✅ Review all generated code
2. ✅ Use allowlist for domains (not "*")
3. ✅ Avoid `allowUnixSockets` unless required
4. ✅ Keep credentials outside sandbox
5. ✅ Test in sandbox before production

---

**Phase 1 Complete | Ready for Phase 2: Live Experiments**
