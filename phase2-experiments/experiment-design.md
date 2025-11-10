# Phase 2: Experiment Design - Live Testing Suite

## Overview

This document defines 20 distinct experiments across 5 experimental sets to empirically measure Claude Code on the Web's capabilities, limitations, and optimal usage patterns.

**Estimated Resources:**
- Time: 2-4 hours
- Credits: 150-250
- Sessions: Up to 20 parallel

## Experiment Design Principles

### Scientific Rigor

1. **Reproducibility**: Each experiment has clear steps and expected outcomes
2. **Measurability**: Quantitative metrics defined for all experiments
3. **Control Variables**: Single factor changes per experiment when possible
4. **Documentation**: Real-time logging of observations

### Metrics Framework

**Performance Metrics:**
- Execution time (minutes)
- Token consumption (input/output)
- Quality score (1-10 subjective + objective criteria)
- Success rate (% of successful completions)

**Cost Metrics:**
- Credits consumed
- Cost per task
- Efficiency ratio (quality/cost)

**Failure Metrics:**
- Error types
- Recovery time
- Failure rate

## Experimental Set 1: Parallel Session Stress Test

**Objective:** Determine optimal parallel session count and identify scaling limits.

**Hypothesis:** Productivity increases linearly up to 5 sessions, then diminishing returns or degradation.

### Experiment 1.1: Single Session Baseline

**Setup:**
- 1 Claude Code session
- Task: Implement REST API with 5 endpoints (CRUD + search)
- Model: Sonnet 4.5
- Complexity: Medium

**Measurements:**
- Total time (start to completion)
- Token consumption
- Code quality (runs tests, follows conventions)
- Human wait time (blocking time)

**Expected Results:**
- Time: ~30 minutes
- Tokens: ~20K input, ~10K output
- Quality: 8/10
- Human wait: ~25 minutes (83%)

**Success Criteria:**
- ✅ All 5 endpoints functional
- ✅ Tests pass
- ✅ Code follows REST conventions

---

### Experiment 1.2: Five Parallel Sessions

**Setup:**
- 5 concurrent Claude Code sessions
- Tasks: 5 independent microservices (User, Product, Order, Payment, Notification)
- Model: Sonnet 4.5 for all
- Same complexity per service as E1.1

**Measurements:**
- Total time (slowest session)
- Aggregate token consumption
- Per-session quality
- Human wait time
- Context switching overhead

**Expected Results:**
- Time: ~35-40 minutes (vs. 150 min serial)
- Tokens: ~100K input, ~50K output (5x baseline)
- Quality: 7-8/10 per service
- Human wait: ~10 minutes (25%)
- Efficiency gain: ~4x (not perfect 5x due to orchestration)

**Success Criteria:**
- ✅ All 5 services functional
- ✅ Tests pass for each
- ✅ No cross-contamination between services
- ✅ Time < 50 minutes

**Risk Mitigation:**
- Use git worktrees for isolation
- Define clear API contracts upfront
- Monitor token consumption actively

---

### Experiment 1.3: Ten Parallel Sessions

**Setup:**
- 10 concurrent sessions
- Tasks: 10 independent components (5 backend APIs + 5 frontend React components)
- Model: Mix of Haiku (frontend) and Sonnet (backend)
- Medium complexity

**Measurements:**
- Total time
- Aggregate token consumption
- Quality degradation (if any)
- Human cognitive load (subjective 1-10)
- Infrastructure response time

**Expected Results:**
- Time: ~45-60 minutes
- Tokens: ~180K input, ~90K output
- Quality: 7/10 average (slight degradation)
- Cognitive load: 6-7/10
- Some slowdown possible (infrastructure limits)

**Success Criteria:**
- ✅ 8/10 components fully functional
- ✅ Quality remains acceptable (>6/10)
- ✅ Time < 90 minutes
- ❌ Expect 1-2 sessions may have issues

**Observations to Document:**
- Infrastructure response times
- Browser tab management challenges
- Git merge complexity
- Credit burn rate

---

### Experiment 1.4: Twenty Parallel Sessions (Stress Test)

**Setup:**
- 20 concurrent sessions
- Tasks: Complete feature across 20 touchpoints (APIs, components, tests, docs, configs)
- Model: Strategic mix (60% Haiku, 40% Sonnet)
- Varied complexity

**Measurements:**
- Total time
- Aggregate token consumption
- Success rate (% functional)
- Infrastructure failures
- Human orchestration overhead

**Expected Results:**
- Time: ~60-90 minutes
- Tokens: ~300K+ input, ~150K+ output
- Success rate: 70-80%
- Infrastructure: Possible rate limiting or slowdowns
- Cognitive load: 8-9/10 (high)

**Success Criteria:**
- ✅ >14/20 sessions complete successfully
- ✅ Document infrastructure limits discovered
- ✅ Time < 120 minutes
- ✅ No credit limit hit

**Failure Modes to Observe:**
- Rate limiting
- Browser performance degradation
- Git merge conflicts
- Context switching exhaustion

---

## Experimental Set 2: Model Selection Optimization

**Objective:** Empirically measure cost/quality trade-offs between models.

**Hypothesis:** 70/30 Haiku/Sonnet split provides optimal value; Haiku fails predictably at 150+ lines.

### Experiment 2.1: Pure Haiku Workflow

**Setup:**
- Single task: Build e-commerce product catalog with filters, pagination, cart
- Model: 100% Haiku 4.5
- Complexity: High (multi-file, >500 total lines)

**Measurements:**
- Total cost (credits)
- Quality score (functionality, bugs, edge cases)
- Hallucination incidents
- Line count where quality degrades

**Expected Results:**
- Cost: Low (~15-20 credits)
- Quality: 6-7/10 (functional but bugs in complex logic)
- Hallucinations: 2-3 instances (150+ line files)
- Speed: Fast

**Success Criteria:**
- ✅ Quantify Haiku's failure point (line count)
- ✅ Document specific hallucination types
- ✅ Measure cost savings vs. Sonnet

---

### Experiment 2.2: Pure Sonnet Workflow

**Setup:**
- Same task as E2.1
- Model: 100% Sonnet 4.5

**Measurements:**
- Total cost (credits)
- Quality score
- Time to completion
- Bug count

**Expected Results:**
- Cost: Medium-High (~60-80 credits)
- Quality: 9/10 (robust, handles complexity)
- Hallucinations: 0
- Speed: Moderate

**Success Criteria:**
- ✅ Baseline for cost comparison
- ✅ Quality benchmark (should be highest)
- ✅ Time measured accurately

---

### Experiment 2.3: 50/50 Hybrid Workflow

**Setup:**
- Same task
- Model: 50% Haiku (simple files), 50% Sonnet (complex files)
- Strategic switching based on file complexity

**Measurements:**
- Total cost
- Quality score
- Switching overhead
- Decision accuracy (was switch appropriate?)

**Expected Results:**
- Cost: Medium (~40-50 credits)
- Quality: 8/10
- Decision accuracy: 80% (some mis-assessments)

**Success Criteria:**
- ✅ Cost between E2.1 and E2.2
- ✅ Quality closer to Sonnet than Haiku
- ✅ Document switching decision framework

---

### Experiment 2.4: 70/30 Haiku/Sonnet (Recommended Split)

**Setup:**
- Same task
- Model: 70% Haiku (routine tasks), 30% Sonnet (critical/complex)
- Follow community best practices for selection

**Measurements:**
- Total cost
- Quality score
- Value ratio (quality/cost)
- Comparison to all previous experiments

**Expected Results:**
- Cost: Low-Medium (~30-40 credits)
- Quality: 8/10
- Best value ratio

**Success Criteria:**
- ✅ Validate community recommendation
- ✅ Highest value ratio among all experiments
- ✅ Quality acceptable for production

---

## Experimental Set 3: Context Management Efficiency

**Objective:** Measure impact of CLAUDE.md size, MCP servers, and file exclusions on performance and cost.

**Hypothesis:** Context bloat significantly impacts token consumption; optimal CLAUDE.md < 5KB.

### Experiment 3.1: CLAUDE.md Size Impact

**Setup:**
- 3 sub-experiments with identical task (implement authentication system)
- Variables:
  - A: Minimal CLAUDE.md (~1KB, essential only)
  - B: Moderate CLAUDE.md (~5KB, comprehensive)
  - C: Bloated CLAUDE.md (~20KB, excessive detail)

**Measurements:**
- Token consumption per sub-experiment
- Quality consistency
- Prompt iteration count
- Cost difference

**Expected Results:**
- A: Lowest tokens, but may require more iterations
- B: Optimal balance (hypothesis)
- C: High tokens, diminishing returns

**Success Criteria:**
- ✅ Quantify token increase per KB of CLAUDE.md
- ✅ Identify optimal size (expected: 3-7KB)
- ✅ Document ROI of CLAUDE.md investment

---

### Experiment 3.2: MCP Server Overhead

**Setup:**
- Same task repeated with varying MCP configurations
- Variables:
  - A: 0 MCP servers
  - B: 3 MCP servers (commonly useful)
  - C: 8 MCP servers (excessive)

**Measurements:**
- Token overhead per server (using `/context`)
- Functionality benefit (did servers help?)
- Cost increase
- Performance impact

**Expected Results:**
- Token overhead: ~500-1000 tokens per server
- Benefit: Diminishing after 3-4 servers
- Cost: 10-30% increase with 8 servers

**Success Criteria:**
- ✅ Quantify per-server token cost
- ✅ Recommend optimal server count (expected: 2-4)
- ✅ Cost-benefit analysis

---

### Experiment 3.3: File Exclusion Impact

**Setup:**
- Task: Refactor codebase with and without proper exclusions
- Variables:
  - A: No exclusions (reads node_modules, build)
  - B: Proper exclusions (exclude node_modules, build, .git)

**Measurements:**
- Token consumption difference
- Performance impact
- Irrelevant file reads
- Cost savings

**Expected Results:**
- Scenario A: 2-5x token waste
- Scenario B: Optimal
- Savings: 50-80% token reduction

**Success Criteria:**
- ✅ Quantify waste from missing exclusions
- ✅ Demonstrate cost savings
- ✅ Create recommended exclusion list

---

## Experimental Set 4: Real-World Workflow Patterns

**Objective:** Time and measure effectiveness of documented workflow patterns.

**Hypothesis:** Structured workflows (TDD, visual iteration) improve quality and reduce iterations.

### Experiment 4.1: TDD Workflow (Test-Driven Development)

**Setup:**
- Task: Implement shopping cart with edge cases
- Process:
  1. Write comprehensive tests first
  2. Confirm all tests fail
  3. Implement to pass tests
  4. Verify no overfitting

**Measurements:**
- Total time
- Iteration count
- Final bug count
- Test coverage
- Quality score

**Expected Results:**
- Time: Slightly longer upfront, faster overall
- Iterations: Fewer (tests guide implementation)
- Bugs: Minimal (caught by tests)
- Coverage: >90%
- Quality: 9/10

**Success Criteria:**
- ✅ All tests pass
- ✅ No bugs in manual testing
- ✅ Compare favorably to non-TDD approach

---

### Experiment 4.2: Visual Iteration Workflow

**Setup:**
- Task: Implement landing page from Figma mockup
- Process:
  1. Provide mockup screenshot
  2. Implement
  3. Screenshot result
  4. Compare and iterate
  5. Repeat until match

**Measurements:**
- Iteration count
- Visual accuracy score (1-10)
- Time per iteration
- Total time
- Token consumption

**Expected Results:**
- Iterations: 3-5
- Accuracy: 8-9/10
- Time per iteration: 5-10 minutes
- Total: 30-40 minutes

**Success Criteria:**
- ✅ Final visual matches mockup (>8/10)
- ✅ Responsive design works
- ✅ Clean, maintainable code

---

### Experiment 4.3: Async Parallel Development Workflow

**Setup:**
- Task: Implement full-stack feature (API + UI + tests + docs)
- Process:
  1. Queue all tasks at once
  2. Let Claude work asynchronously
  3. Minimal intervention during execution
  4. Integrate results

**Measurements:**
- Human active time vs. total time
- Quality of autonomous work
- Integration effort required
- Productivity multiplier

**Expected Results:**
- Human active time: 20% of total
- Quality: 7-8/10 (some integration fixes needed)
- Integration: 15-30 minutes
- Multiplier: 3-4x

**Success Criteria:**
- ✅ Feature fully functional after integration
- ✅ Human time < 30% of total
- ✅ Demonstrate async advantage

---

### Experiment 4.4: Git-Based Handoff Workflow

**Setup:**
- Task: Multi-stage feature (research → prototype → refine)
- Process:
  1. Session 1: Research and prototype (Haiku)
  2. Commit to branch
  3. Session 2: Refine and optimize (Sonnet)
  4. Commit to branch
  5. Session 3: Test and document (Haiku)

**Measurements:**
- Handoff efficiency (how well does context transfer?)
- Commit message quality
- Continuity of work
- Total time vs. single session

**Expected Results:**
- Handoff efficiency: 80% (some context loss)
- Commit messages: Clear and informative
- Continuity: Good with proper commits
- Time: Similar to single session (but better specialization)

**Success Criteria:**
- ✅ Each session builds on previous successfully
- ✅ No major context loss
- ✅ Clear commit history tells story

---

## Experimental Set 5: Failure Mode Reproduction & Recovery

**Objective:** Deliberately trigger known failure modes and validate recovery strategies.

**Hypothesis:** Documented failure patterns are reproducible and recoverable with specific techniques.

### Experiment 5.1: Haiku Hallucination Trigger

**Setup:**
- Task: Have Haiku generate single file with 200+ lines of complex logic
- Intentionally trigger hallucination failure

**Measurements:**
- Line count where hallucinations begin
- Types of hallucinations (syntax errors, logic bugs, made-up APIs)
- Detection time
- Recovery strategy effectiveness

**Expected Results:**
- Hallucinations begin: ~150-180 lines
- Types: Logic bugs, incomplete implementations
- Detection: Immediately on review or test run

**Recovery Strategy:**
- Switch to Sonnet
- Request verification subagent
- Break into smaller files

**Success Criteria:**
- ✅ Reproduce hallucination predictably
- ✅ Document specific failure patterns
- ✅ Validate recovery strategy

---

### Experiment 5.2: Context Window Exhaustion

**Setup:**
- Long session with continuous task additions
- No `/clear` usage
- Accumulate context until degradation

**Measurements:**
- Token count at degradation point
- Degradation symptoms (repetition, confusion, quality drop)
- Recovery time with `/clear`

**Expected Results:**
- Degradation: ~180K+ tokens
- Symptoms: Repetitive responses, forgetting earlier context
- Recovery: Immediate with `/clear`

**Recovery Strategy:**
- `/clear` command
- Restart with essential context only
- Summarize previous work in new prompt

**Success Criteria:**
- ✅ Observe clear degradation point
- ✅ Confirm `/clear` effectiveness
- ✅ Recommend context reset frequency

---

### Experiment 5.3: Parallel Session File Conflicts

**Setup:**
- 2 parallel sessions intentionally editing same files
- No git worktree isolation (deliberately problematic)

**Measurements:**
- Conflict occurrence rate
- Conflict complexity
- Resolution time
- Workflow contamination

**Expected Results:**
- Conflicts: High (>80% of shared files)
- Complexity: Moderate to high
- Resolution: 10-30 minutes
- Contamination: Significant

**Recovery Strategy:**
- Manual conflict resolution
- Session restart
- Retrospective application of git worktrees

**Success Criteria:**
- ✅ Demonstrate why worktrees are mandatory
- ✅ Quantify cost of improper isolation
- ✅ Document conflict resolution best practices

---

## Experiment Execution Plan

### Phase 2A: Foundation (40-60 minutes, 40-60 credits)

**Order:**
1. E1.1: Single session baseline
2. E2.2: Pure Sonnet baseline
3. E3.1: CLAUDE.md size impact
4. E4.1: TDD workflow

**Rationale:** Establish baselines before comparative experiments.

### Phase 2B: Parallelization (60-90 minutes, 60-100 credits)

**Order:**
5. E1.2: Five parallel sessions
6. E1.3: Ten parallel sessions
7. E1.4: Twenty parallel sessions (stress test)

**Rationale:** Progressive scaling to identify limits safely.

### Phase 2C: Optimization (40-60 minutes, 30-50 credits)

**Order:**
8. E2.1: Pure Haiku
9. E2.3: 50/50 hybrid
10. E2.4: 70/30 recommended
11. E3.2: MCP overhead
12. E3.3: File exclusion impact

**Rationale:** Cost optimization experiments with baselines established.

### Phase 2D: Advanced Workflows (40-60 minutes, 30-50 credits)

**Order:**
13. E4.2: Visual iteration
14. E4.3: Async parallel development
15. E4.4: Git-based handoff

**Rationale:** Real-world pattern validation.

### Phase 2E: Failure Modes (30-45 minutes, 20-30 credits)

**Order:**
16. E5.1: Haiku hallucination
17. E5.2: Context exhaustion
18. E5.3: File conflicts

**Rationale:** Final experiments; acceptable to end on failures for learning.

---

## Data Collection Framework

### Standardized Data Sheet Per Experiment

```markdown
## Experiment [ID]: [Name]

**Start Time:** [HH:MM]
**End Time:** [HH:MM]
**Duration:** [MM minutes]

### Setup
- Model(s): [...]
- Task: [...]
- Variables: [...]

### Measurements
- Tokens In: [XXXXX]
- Tokens Out: [XXXXX]
- Credits: [XX.XX]
- Quality Score: [X/10]
- Success Rate: [XX%]

### Observations
[Real-time notes]

### Results
[Quantitative data]

### Analysis
[Interpretation]

### Recommendations
[Actionable insights]

### Artifacts
- Code: [link]
- Screenshots: [links]
- Logs: [links]
```

### Aggregation Spreadsheet

| Exp ID | Model | Duration | Tokens In | Tokens Out | Credits | Quality | Success |
|--------|-------|----------|-----------|----------|---------|---------|---------|
| 1.1 | Sonnet | 30 | 20000 | 10000 | 25 | 8/10 | 100% |
| ... | ... | ... | ... | ... | ... | ... | ... |

---

## Success Criteria for Phase 2

### Quantitative
- ✅ 18+ experiments completed
- ✅ All experiments have numerical measurements
- ✅ Total credits < 300
- ✅ Total time < 5 hours

### Qualitative
- ✅ Clear patterns emerge from data
- ✅ Failure modes documented with recovery strategies
- ✅ Reproducible test cases for playbook readers
- ✅ Unexpected insights discovered (bonus)

### Deliverables
- ✅ Experiment results document (detailed)
- ✅ Data aggregation spreadsheet
- ✅ Artifacts repository (code, screenshots, logs)
- ✅ Preliminary analysis (input for Phase 3)

---

## Risk Management

### Budget Overrun
- **Trigger:** >200 credits consumed before Phase 2E
- **Response:** Reduce parallel session count in E1.4, skip E1.4 if necessary

### Time Overrun
- **Trigger:** >3 hours elapsed before Phase 2D
- **Response:** Reduce E1.4 from 20 to 10 sessions, combine experiments

### Infrastructure Limits
- **Trigger:** Rate limiting or failures in parallel sessions
- **Response:** Document thoroughly (valuable data), reduce concurrency

### Quality Issues
- **Trigger:** Experiments not producing measurable results
- **Response:** Adjust methodology mid-phase, document learnings

---

## Transition to Phase 3

**Prerequisites for Phase 3:**
- All experimental data collected
- Artifacts organized and accessible
- Preliminary patterns observed
- Data validated for consistency

**Readiness Checklist:**
- [ ] 18+ experiments completed
- [ ] Data sheet filled for each
- [ ] Aggregation spreadsheet complete
- [ ] Artifacts repository organized
- [ ] Initial observations documented

**Next Step:** Data Analysis & Pattern Extraction (Phase 3)

---

**Phase 2 Design Complete | Ready for Execution**
