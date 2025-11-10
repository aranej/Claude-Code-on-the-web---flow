# Official Documentation Summary - Phase 1

## Executive Summary

Claude Code on the Web is a cloud-based AI coding assistant launched October 2025 as a research preview, enabling developers to run autonomous coding sessions directly from claude.ai without terminal access. Key differentiator: asynchronous workflow with secure sandboxing.

## Core Architecture

### Technical Foundation

**Execution Environment:**
- Google's Gvisor container runtime for isolation
- System call filtering: permits only 70-80 of Linux's 300+ system calls
- Resource limits: 2 CPU cores, 4GB RAM, 10GB disk, 1Gbps network

**Sandboxing Model (Two-Boundary System):**

1. **Filesystem Isolation**
   - Read/write access to working directory only
   - Blocks modification outside designated areas
   - OS-level enforcement via:
     - Linux: Bubblewrap
     - macOS: Seatbelt

2. **Network Isolation**
   - Unix domain socket → proxy server architecture
   - Domain allowlist/blocklist support
   - User confirmation for new domains
   - No traffic inspection (user responsibility)

**Security Impact:**
- 84% reduction in permission prompts (internal testing)
- Credentials (git, signing keys) kept outside sandbox
- Enables autonomous operation while maintaining boundaries

## Key Features

### 1. GitHub Integration
- Direct repository connection
- Automatic branch creation
- Pull request generation
- Issue reading & implementation

### 2. Asynchronous Workflow
- Queue multiple prompts while session runs
- Non-blocking task submission
- Background execution on Anthropic infrastructure

### 3. Teleport Feature
- Copy chat transcript to local CLI
- Transfer edited files for local continuation
- Seamless cloud-to-local handoff

### 4. Environment Configuration
- Fully locked down (no network)
- Restricted (allowlist domains)
- Custom domains (including "*" wildcard)

## Model Selection Strategy

### Available Models

| Model | Performance | Cost | Speed | Use Case |
|-------|------------|------|-------|----------|
| **Sonnet 4.5** | 93.7% HumanEval | Baseline | 0.64s TTFT | Complex coding, architecture |
| **Haiku 4.5** | 88.1% HumanEval | 80% cheaper | 0.36s TTFT | Routine tasks, high-frequency |
| **Opus 4.1** | Highest | Premium | Slower | Critical decisions, deep reasoning |

**Recommended Split:** 70% Haiku / 30% Sonnet for optimal cost/performance

### Performance Notes
- Haiku 4.5: 90% of Sonnet performance at 3x cost savings
- Haiku struggles with 150+ line code generation (hallucinations)
- Sonnet handles complex problems more reliably
- 4-5x speed advantage: Haiku over Sonnet

## Best Practices from Official Docs

### Setup & Configuration

**CLAUDE.md Files**
- Special file automatically pulled into context
- Locations: repo root, parent/child dirs, `~/.claude/CLAUDE.md`
- Content: bash commands, code style, testing instructions, project etiquette
- Generate via `/init` command
- Refine iteratively like prompts

**Tool Allowlisting**
- Default: request permission for system modifications
- Management options:
  - "Always allow" selections
  - `/permissions` command
  - `.claude/settings.json`
  - `--allowedTools` CLI flag
- Install `gh` CLI for enhanced GitHub integration

**Custom Tools & MCP**
- Document custom tools in CLAUDE.md with examples
- Configure MCP servers via:
  - Project config
  - Global config
  - `.mcp.json` (checked-in)

### Core Workflows

**1. Explore → Plan → Code → Commit**
- Request reading without coding first
- Use "think" keyword for extended reasoning
- Implement with verification
- Commit with updated docs

**2. Test-Driven Development**
- Write tests from expected I/O first
- Confirm tests fail
- Implement until tests pass
- Use subagents to verify no overfitting

**3. Visual Iteration**
- Provide screenshots (Puppeteer MCP, simulators, paste)
- Supply design mocks
- Iterate until visual match

**4. Safe Autonomous Mode**
- `claude --dangerously-skip-permissions` for low-risk tasks
- Minimize risks: use in container without internet

**5. Git & GitHub Integration**
- Search commit history for context
- Auto-generate commit messages
- Handle rebasing, conflicts, patches
- Manage PRs and code reviews

### Optimization Techniques

**Clarity & Specificity**
- Explicit instructions reduce course corrections
- Include context: file references, images, URLs
- Tab-complete specific files/folders
- Pass domain URLs to allowlist

**Course Correction**
- Request planning before coding
- Escape to interrupt (preserves context)
- Double-tap Escape to edit previous prompts
- Request undo for alternative approaches

**Context Management**
- `/clear` between tasks to reset window
- Markdown checklists for complex workflows
- Data input methods:
  - Copy-paste
  - Pipes
  - Bash commands
  - MCP tools
  - File reads

**Cost Management**
- Use `/cost` to view token statistics
- Use `/status` to check remaining usage
- Use `/context` to monitor MCP overhead
- Exclude node_modules, build artifacts, test fixtures
- Strategic CLAUDE.md for file selection
- Stateless design: full history reprocessed each message

### Advanced Patterns

**Headless Automation**
- `-p` flag for non-interactive CI/pre-commit
- `stream-json` output for processing pipelines
- Issue triage automation
- Subjective code reviews

**Multi-Claude Workflows**
- Parallel instances for writing + verification
- Separate git checkouts or worktrees
- "Fanning out" for migrations/analyses
- Pipeline into existing data workflows

**Jupyter Notebooks**
- Interprets outputs and images
- Data exploration assistance
- Aesthetic improvements for presentations

## Security Considerations

### Known Limitations

**Network Filtering**
- No traffic inspection (domain-level only)
- Broad domains (e.g., github.com) risk data exfiltration
- Domain fronting bypass potential

**Unix Socket Risks**
- `allowUnixSockets` can grant system service access
- Example: `/var/run/docker.sock` → host system access

**Autonomy Trade-offs**
- Reduced prompts → potential decrease in vigilance
- Autonomous operation requires thorough code review
- Lost "built-in review" from constant approval

## Availability & Pricing

**Access Tiers:**
- Pro Plan: $20/month
- Max Plan: $100/month or $200/month
- Available via claude.ai "Code" tab or iOS app

**Growth Metrics:**
- 10x user growth since May launch
- $500M+ annualized revenue

## Key Takeaways

1. **Security Model**: Two-boundary isolation (filesystem + network) is foundational
2. **Cost Optimization**: 70/30 Haiku/Sonnet split recommended
3. **Async Advantage**: Non-blocking execution enables parallel workflows
4. **Context is King**: CLAUDE.md + careful file selection = efficiency
5. **Trade-offs**: Autonomy requires increased vigilance in code review

---

**Sources:**
- docs.claude.com/en/docs/claude-code/
- anthropic.com/engineering/claude-code-best-practices
- anthropic.com/engineering/claude-code-sandboxing
- anthropic.com/news/claude-code-on-the-web
