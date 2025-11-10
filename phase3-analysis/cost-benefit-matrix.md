# Cost-Benefit Matrix

## Model Selection Matrix

| Model | Input Cost | Output Cost | Quality | Speed | Value Score | Best Use Case |
|-------|-----------|-------------|---------|-------|-------------|---------------|
| Haiku 4.5 | $1/Mtok | $5/Mtok | 6.5/10 | 5/5 | **0.36** | Routine, <100 lines |
| Sonnet 4.5 | $3/Mtok | $15/Mtok | 9/10 | 3/5 | 0.13 | Complex, critical |
| 70/30 Hybrid | ~$1.6/Mtok | ~$8/Mtok | 8/10 | 4/5 | **0.24** ⭐ | General purpose |

## Parallel Session ROI

| Sessions | Setup Cost | Monthly Credits | Productivity | ROI | Recommendation |
|----------|-----------|-----------------|--------------|-----|----------------|
| 1 | $0 | ~$50-100 | 1.0x | Baseline | Default |
| 5 | $20 (worktrees) | ~$250-400 | 3.7x | **3.7x** ⭐ | Optimal |
| 10 | $30 (tools) | ~$500-800 | 2.7x | 2.7x | Advanced |
| 20 | $50 (orchestration) | ~$900-1500 | 1.9x | 1.9x | Not recommended |

## Subscription Plan Analysis

| Plan | Monthly Cost | Usage Limit | Effective Cost/Hour | Best For |
|------|-------------|-------------|---------------------|----------|
| Pro | $20 | ~15-20 hours | $1-1.30/hour | Light users |
| Max | $100-200 | Unlimited | $5-10/hour | Heavy users (>20hr/wk) |

**Break-even:** Max plan worth it at >20 hours/week

## Context Optimization ROI

| Optimization | One-time Cost | Monthly Savings | Payback Period | Priority |
|--------------|--------------|-----------------|----------------|----------|
| File exclusions | 5 min | $20-50 | Immediate | P0 |
| CLAUDE.md optimization | 30 min | $10-20 | 1-2 sessions | P0 |
| MCP pruning | 10 min | $5-15 | 1 month | P1 |
| Git worktrees | 1 hour (learning) | Enables parallel (3x) | 2-3 sessions | P0 |

## Workflow Pattern Efficiency

| Pattern | Implementation Time | Time Savings | Quality Improvement | Net Benefit |
|---------|-------------------|--------------|---------------------|-------------|
| TDD | +10 min upfront | -10 min total (22% faster) | +2 points | **High** ⭐ |
| Visual Iteration | +5 min setup | -15 min iterations | +1 point | **High** ⭐ |
| Async Parallel | +15 min orchestration | -85 min blocking | Neutral | **Very High** ⭐ |
| Git Handoff | +5 min per handoff | Enables specialization | +0.5 points | Medium |

---

**Highest ROI Actions:**
1. File exclusions (instant, massive)
2. 70/30 model split (2.5x value vs. Sonnet)
3. 5 parallel sessions (3.7x productivity)
4. TDD workflow (22% faster, 47% better coverage)
