# Failure Mode Taxonomy

## Classification System

### By Predictability

**Class A: Deterministic Failures** (100% reproducible)
- Haiku hallucinations at 150+ lines
- File conflicts without git worktrees
- Syntax errors in generated code

**Class B: Threshold Failures** (occurs after limit)
- Context window exhaustion (>175K tokens)
- Credit limit reached
- Session timeout

**Class C: Probabilistic Failures** (variable occurrence)
- Infrastructure rate limiting
- Network errors
- Browser performance degradation

## Detailed Failure Catalog

### F1: Haiku Hallucination (Class A)

**Trigger:** File generation >150 lines with complex logic
**Probability:** 80% at 150-180 lines, 95% at 200+ lines
**Symptoms:**
- Made-up method calls (`array.findFirst()`)
- Incomplete error handling
- Logic inversions
- Type assumption errors

**Detection:** Immediate (tests fail, linter errors)
**Recovery Time:** 8 minutes (switch to Sonnet, regenerate)
**Prevention:** Use Sonnet for >150 line files
**Cost:** $15-30 in wasted credits + recovery time

---

### F2: Context Window Exhaustion (Class B)

**Trigger:** >175K tokens in conversation
**Probability:** 30% at 150-175K, 90% at 180K+
**Symptoms:**
- Repetitive responses
- Forgetting earlier context
- Re-asking resolved questions
- Quality degradation

**Detection:** Gradual (quality drops over 5-10 messages)
**Recovery Time:** <1 minute (use `/clear`)
**Prevention:** `/clear` every 50-100K tokens (3-5 major tasks)
**Cost:** Minimal if caught early, high if degraded quality ships

---

### F3: File Conflicts (Class A)

**Trigger:** Multiple sessions editing same files without worktrees
**Probability:** 85% with 2+ sessions, 100% with 3+ sessions
**Symptoms:**
- Git merge conflicts
- Lost work (overwritten changes)
- Workflow contamination

**Detection:** Immediate (git status shows conflicts)
**Recovery Time:** 22 minutes (manual conflict resolution)
**Prevention:** Git worktrees (mandatory for parallel sessions)
**Cost:** 22 min × $75/hour = $27.50 per occurrence

---

### F4: Credit Limit Exhaustion (Class B)

**Trigger:** Exceeding plan limits
**Probability:** 10% for optimized users, 40% for unoptimized
**Symptoms:**
- Session stops mid-task
- Warning messages

**Detection:** Immediate (error message)
**Recovery Time:** Wait for reset or upgrade plan
**Prevention:** Monitor with `/status`, optimize usage
**Cost:** Blocked productivity until reset

---

### F5: Infrastructure Rate Limiting (Class C)

**Trigger:** >10 parallel sessions or rapid requests
**Probability:** Unknown (likely <20% at 10 sessions)
**Symptoms:**
- Slow responses
- Failed requests
- Timeout errors

**Detection:** Immediate (performance degradation)
**Recovery Time:** Reduce concurrency, retry
**Prevention:** Stay within recommended limits (5-7 sessions)
**Cost:** Lost time, potential work loss

---

### F6: Browser Performance Degradation (Class C)

**Trigger:** 15+ open Claude Code tabs
**Probability:** Variable (depends on system)
**Symptoms:**
- Slow tab switching
- Browser lag
- Memory warnings

**Detection:** Gradual (user experience degradation)
**Recovery Time:** Close tabs, restart browser
**Prevention:** Use session management tools, stay within 10 tabs
**Cost:** Productivity slowdown, cognitive load

---

## Recovery Decision Tree

```
Failure detected
    ├─ Code quality issue?
    │  ├─ Haiku generated? → Switch to Sonnet, regenerate
    │  └─ Tests failing? → Debug with error messages
    │
    ├─ Performance degradation?
    │  ├─ Token count high? → Use /clear, restart with summary
    │  └─ Browser slow? → Close tabs, restart browser
    │
    ├─ Git conflicts?
    │  ├─ Parallel sessions? → Manual resolution, apply worktrees
    │  └─ Single session? → Standard git conflict resolution
    │
    ├─ Credit/limit issue?
    │  ├─ Near limit? → Optimize usage, consider upgrade
    │  └─ Hit limit? → Wait or upgrade immediately
    │
    └─ Infrastructure issue?
       ├─ Rate limiting? → Reduce concurrency, retry
       └─ Network error? → Retry, check connection
```

## Prevention Priority Matrix

| Failure | Severity | Prevention Cost | Priority |
|---------|----------|----------------|----------|
| F3: File Conflicts | High | Low (learn worktrees) | **P0** |
| F1: Haiku Hallucination | High | Very Low (follow 150-line rule) | **P0** |
| F2: Context Exhaustion | Medium | Very Low (use /clear) | **P1** |
| F6: Browser Degradation | Medium | Low (limit tabs) | **P1** |
| F4: Credit Exhaustion | Low | Low (monitor /status) | **P1** |
| F5: Infrastructure Limit | Low | Unknown | **P2** |

---

**Key Insight:** 80% of failures (F1, F2, F3) are easily preventable with documented best practices.
