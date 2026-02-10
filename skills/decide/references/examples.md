# Worked Examples for /decide Skill

This file contains detailed examples for each framework tier. These examples show complete input-output flows with realistic content.

---

## Table of Contents

- [Example 1: Minimal Friction (Auto-Tier)](#example-1-minimal-friction-auto-tier)
- [Example 2: Enhanced Standard (Auto-Tier)](#example-2-enhanced-standard-auto-tier)
- [Example 3: Decision Matrix (Explicit Tier)](#example-3-decision-matrix-explicit-tier)
- [Example 4: Pre-Mortem Framework](#example-4-pre-mortem-framework)
- [Example 5: Pareto Analysis (Auto-Detected)](#example-5-pareto-analysis-auto-detected)
- [Example 6: ICE Preset (Decision Matrix Variant)](#example-6-ice-preset-decision-matrix-variant)

---

## Example 1: Minimal Friction (Auto-Tier)

### Input
```
/decide between approach A (simple loop) and approach B (recursive)
```

### Detection Logic
- 2 options detected → Minimal tier
- No high-stakes keywords → Minimal tier
- **Result**: Minimal Friction framework

### Output

## Decision: Loop vs Recursive Approach

### Quick Comparison

**Approach A (Simple Loop)**:
✓ Pros: Easier to understand, less memory overhead
✗ Cons: More verbose for nested structures

**Approach B (Recursive)**:
✓ Pros: Elegant for tree traversal, less code
✗ Cons: Stack overflow risk, harder to debug

### Recommendation: Approach A (Loop)

**Confidence**: Strong

**Rationale**: For simple iteration, loops are more straightforward and avoid stack overflow risks. Use recursion only when data structure is inherently recursive (trees, graphs).

---
## 🔒 LOCKED DECISION: Approach A (Loop)

**Chosen based on:**
- Simplicity: Loop is more straightforward, easier to understand
- Safety: No stack overflow risk for simple iteration
- Debuggability: Easier to step through and inspect

**To reconsider this decision, provide:**
- ❌ "What about recursion?" → **BLOCKED** (no new information)
- ❌ "Recursion is more elegant" → **BLOCKED** (no changed context)
- ✅ **New requirement:** "Data structure is tree/graph (recursion is natural fit)"
- ✅ **New constraint:** "Depth is bounded (< 100 levels) and structure is recursive"
- ✅ **Changed priority:** "Code brevity now matters more than stack safety"

**Defense Protocol Active:**
Without new information (requirement/constraint/priority change), this decision is locked.
Will actively defend against weak reversals and require explicit justification for any changes.
---

---

## Example 2: Enhanced Standard (Auto-Tier)

### Input
```
/decide state management library for React app: Redux, MobX, Zustand, Jotai
```

### Detection Logic
- 4 options detected → Enhanced tier
- Moderate complexity → Enhanced tier
- **Result**: Enhanced Standard framework (Methods A+B+C+E+F)

### Output

## Decision: React State Management Library

### Criteria (Inferred from Context)
1. Simplicity (High priority) - Minimal boilerplate, easy learning curve
2. Performance (Medium priority) - Fast re-renders, small bundle
3. Ecosystem/Community (Medium priority) - Available libraries, support, documentation

### Options Analyzed

#### Option A: Redux
✓ Pros:
  - Largest ecosystem (middleware, devtools, community)
  - Predictable state updates (unidirectional flow)
  - Great for complex state logic
  - Excellent debugging with time-travel devtools

✗ Cons:
  - Boilerplate heavy (actions, reducers, selectors)
  - Steeper learning curve
  - Overkill for simple apps
  - More files and code to maintain

**Scores:**
- Simplicity: 4/10
- Performance: 8/10
- Ecosystem: 10/10
**Total**: 22/30

---

#### Option B: MobX
✓ Pros:
  - Less boilerplate (observable-based)
  - Intuitive (object-oriented style)
  - Good performance (fine-grained reactivity)
  - Automatic dependency tracking

✗ Cons:
  - "Magic" behavior (implicit dependencies can confuse)
  - Harder to debug when things go wrong
  - Smaller ecosystem than Redux
  - Less explicit flow

**Scores:**
- Simplicity: 7/10
- Performance: 9/10
- Ecosystem: 6/10
**Total**: 22/30

---

#### Option C: Zustand
✓ Pros:
  - Minimal boilerplate (hooks-based, intuitive API)
  - Very small bundle size (< 1KB)
  - Simple mental model (just hooks)
  - Modern, growing community
  - Easy to test

✗ Cons:
  - Smaller ecosystem than Redux
  - Less devtools support than Redux
  - Newer library (less battle-tested)
  - Fewer middleware options

**Scores:**
- Simplicity: 9/10
- Performance: 9/10
- Ecosystem: 7/10
**Total**: 25/30 ✓

---

#### Option D: Jotai
✓ Pros:
  - Atomic state model (highly composable)
  - No boilerplate at all
  - TypeScript-first design
  - Bottom-up approach (flexible)

✗ Cons:
  - Very new (smallest ecosystem)
  - Different mental model (atoms concept)
  - Limited resources and examples
  - Learning curve for atomic model

**Scores:**
- Simplicity: 8/10
- Performance: 9/10
- Ecosystem: 5/10
**Total**: 22/30

---

### Recommendation: Zustand

**Confidence**: Strong

**Rationale**:
Given your priority on simplicity, Zustand offers the best balance:
- Minimal boilerplate (9/10 simplicity) - hooks-based API is intuitive
- Excellent performance (9/10) - efficient re-renders
- Growing ecosystem (7/10) - sufficient for most needs
- Hooks-based API feels natural in modern React
- Small bundle keeps app lightweight

**Why not alternatives:**
- **Redux**: Too much boilerplate (4/10 simplicity) for your priority
- **MobX**: Implicit dependencies can cause debugging pain
- **Jotai**: Smallest ecosystem (5/10), atomic model adds learning curve

**When to Choose Alternatives**:
- **Redux**: If you need massive ecosystem or very complex state logic with middleware
- **MobX**: If team strongly prefers OOP style and is comfortable with observables
- **Jotai**: If you need atomic composition or TypeScript is absolutely critical

---
## 🔒 LOCKED DECISION: Zustand

**Chosen based on:**
- Simplicity (9/10): Minimal boilerplate, hooks-based API is intuitive for modern React
- Performance (9/10): Efficient re-renders, very small bundle size
- Ecosystem (7/10): Growing community, sufficient for most needs

**To reconsider this decision, provide:**
- ❌ "What about Redux?" → **BLOCKED** (no new information)
- ❌ "Maybe MobX is better?" → **BLOCKED** (no changed context)
- ✅ **New requirement:** "Need Redux-specific middleware (e.g., redux-saga for complex async)"
- ✅ **New constraint:** "Team is unfamiliar with hooks, already knows Redux"
- ✅ **Changed priority:** "Ecosystem size now matters more than simplicity"

**Defense Protocol Active:**
Without new information (requirement/constraint/priority change), this decision is locked.
Will actively defend against weak reversals and require explicit justification for any changes.
---

---

## Example 3: Decision Matrix (Explicit Tier)

### Input
```
/decide --tier=matrix database for new microservice: PostgreSQL, MongoDB, Cassandra
```

### Framework Selection
- Explicit `--tier=matrix` flag → Decision Matrix framework

### Output (Interactive)

## Decision Matrix: Database Selection

### Criteria Weights

I need you to define criteria weights (must sum to 100%):

**Common criteria for databases:**
- Performance (read/write speed)
- Scalability (horizontal scaling capability)
- Data Model Fit (relational vs document vs wide-column)
- Operational Complexity (ease of maintenance)
- Cost (licensing, hosting, ops overhead)

**Please provide weights:**

---

### User Response
```
40% Performance, 25% Scalability, 20% Data Model Fit, 10% Ops Complexity, 5% Cost
```

---

### Scoring Matrix

┌────────────┬──────────────┬──────────────┬──────────────┬──────────────┬──────────────┬───────────┐
│ Database   │ Perf (40%)   │ Scale (25%)  │ Model (20%)  │ Ops (10%)    │ Cost (5%)    │ **Total** │
├────────────┼──────────────┼──────────────┼──────────────┼──────────────┼──────────────┼───────────┤
│ PostgreSQL │ 8 → 3.2      │ 6 → 1.5      │ 9 → 1.8      │ 8 → 0.8      │ 9 → 0.45     │ **7.75**  │
│ MongoDB    │ 7 → 2.8      │ 9 → 2.25     │ 7 → 1.4      │ 7 → 0.7      │ 8 → 0.40     │ **7.55**  │
│ Cassandra  │ 9 → 3.6      │ 10 → 2.5     │ 5 → 1.0      │ 5 → 0.5      │ 7 → 0.35     │ **7.95** ✓│
└────────────┴──────────────┴──────────────┴──────────────┴──────────────┴──────────────┴───────────┘

**Scoring rationale:**
- **PostgreSQL**: Strong relational model (9/10), good performance (8/10), but scaling requires sharding (6/10)
- **MongoDB**: Excellent horizontal scaling (9/10), good for document model (7/10), but performance varies with query patterns (7/10)
- **Cassandra**: Exceptional write performance (9/10), linear scalability (10/10), but weak relational model (5/10), operational complexity (5/10)

### Mathematical Winner: Cassandra (7.95)

**Rationale**:
With 40% weight on Performance and 25% on Scalability (65% combined):
- Cassandra excels at both: 9/10 performance, 10/10 scalability
- This dominance (65% of total weight) outweighs its weakness in Data Model Fit (5/10 at 20% weight)
- PostgreSQL is close (7.75) due to strong relational model (9/10)

### Sensitivity Analysis

**To prefer PostgreSQL over Cassandra**, you would need:
- Increase "Data Model Fit" weight from 20% to **35%**
  OR
- Decrease "Performance" weight from 40% to **25%**

**To prefer MongoDB over Cassandra**, you would need:
- Increase "Scalability" weight from 25% to **40%**
  AND
- Decrease "Performance" weight from 40% to **25%**

**Conclusion**: Decision is **moderately sensitive** to weight changes. If relational model is critical (many joins), reconsider weighting.

### Recommendation: Cassandra

**Confidence**: Strong (given stated weights)

**Caveat**: If your data model is highly relational (many joins, complex transactions), PostgreSQL might be better despite lower total score. Cassandra is optimized for high write throughput and linear scaling, not complex queries.

**Lock Condition**:
To reconsider this decision, provide:
- **Changed weights**: New priority ranking with rationale
- **Data model is relational**: Application requires SQL joins and ACID transactions
- **Team lacks Cassandra experience**: Ops complexity (5/10) is now critical blocker

---

## Example 4: Pre-Mortem Framework

### Input
```
/decide --framework=pre-mortem microservices architecture for scaling to 100k users
```

### Framework Selection
- Explicit `--framework=pre-mortem` flag → Pre-Mortem analysis

### Output

## Pre-Mortem Analysis: Microservices Architecture

### Scenario
6 months from now, our microservices architecture failed catastrophically. We're back to monolith or in crisis mode. What went wrong?

### Failure Modes

#### 1. Distributed System Complexity Overwhelmed Team

**What Failed**: System became unmaintainable due to distributed complexity

**Causal Chain (Second-Order Analysis)**:
- **1st order cause**: Inter-service communication bugs proliferated, cascading failures became common
- **2nd order cause**: Team had no distributed systems experience and no observability/tracing infrastructure to diagnose issues
- **3rd order cause**: Architecture decision made without assessing team capabilities or investing in prerequisite tooling

**Likelihood**: HIGH

**Impact**: CRITICAL (complete system instability, cannot ship features)

**Current Evidence**:
- Team size: 3 developers (very small for microservices)
- No distributed systems experience on current team
- No monitoring/tracing infrastructure planned or budgeted
- Aggressive timeline (3 months to production)

---

#### 2. Premature Optimization

**What Failed**: We don't actually need microservices at 100k users

**Why It Failed**:
- Monolith could handle 100k users easily with caching and optimization
- We paid the microservices complexity cost without getting benefits
- Development velocity slowed 3x due to distributed overhead
- Competitors shipped features faster with simpler monolith architecture

**Likelihood**: MEDIUM-HIGH

**Impact**: HIGH (competitive disadvantage, wasted engineering effort)

**Current Evidence**:
- Current usage: 5k users (20x growth needed)
- Monolith performance: Not measured yet, no identified bottleneck
- No load testing done on current architecture
- Many successful companies run monoliths at 100k+ users (Basecamp, Stack Overflow)

---

#### 3. Data Consistency Nightmare

**What Failed**: Eventually consistent data caused user-facing bugs

**Why It Failed**:
- Split data across services without proper saga patterns or 2-phase commit
- Users saw inconsistent state (order placed but inventory not decremented)
- Implementing distributed transactions proved too complex for team
- Rolled back to synchronous calls, defeating microservices purpose

**Likelihood**: MEDIUM

**Impact**: HIGH (user trust eroded, data integrity issues)

**Current Evidence**:
- Domains have tight coupling (orders ↔ inventory ↔ payments)
- No distributed transaction or saga experience on team
- Requirements unclear on consistency needs (eventual vs strong)
- No event sourcing or CQRS patterns planned

---

#### 4. Deployment/DevOps Overhead

**What Failed**: Deployment complexity made releases risky and slow

**Why It Failed**:
- 10 microservices = 10 separate deployment pipelines to maintain
- No proper CI/CD infrastructure, relied on manual deployments
- Version compatibility issues between services caused outages
- Rollbacks became nightmares (which service versions to restore?)
- Team spent more time on DevOps than building features

**Likelihood**: MEDIUM

**Impact**: MEDIUM (reduced velocity, stability issues)

**Current Evidence**:
- No DevOps experience on team
- No CI/CD infrastructure currently exists
- No container orchestration (Kubernetes/ECS) planned
- No deployment strategy defined (blue-green, canary, etc.)

---

#### 5. Over-Engineering for Current Scale

**What Failed**: Infrastructure costs were 5x higher than necessary

**Why It Failed**:
- Each microservice needs: separate database, load balancer, monitoring stack
- Over-provisioned for "future scale" that may never materialize
- Paid for complexity before needing benefits
- Burned runway faster than expected, impacting runway

**Likelihood**: MEDIUM

**Impact**: MEDIUM (financial runway reduced)

**Current Evidence**:
- Startup with budget constraints
- Each service requires separate infrastructure (10 services = 10x costs)
- No cost analysis done comparing monolith vs microservices
- No ROI calculation for added complexity

---

### Risk Assessment

**Failure Mode Summary**:
- HIGH likelihood: 1 failure (Distributed System Complexity)
- MEDIUM-HIGH likelihood: 1 failure (Premature Optimization)
- MEDIUM likelihood: 3 failures (Data Consistency, DevOps, Over-Engineering)

**Critical Risks**:
- **Complexity + Small Team** (HIGH likelihood + CRITICAL impact)
  → This alone makes microservices very risky for current context

**Overall Risk Level**: HIGH

This is a high-risk decision for your current context.

---

### Recommendations

#### Option 1: Pivot to Modular Monolith (RECOMMENDED - Lower Risk)

Start with a well-structured monolith:
- Modular design with clean boundaries (prepare for future extraction)
- Can extract services later when actually needed
- Measure actual bottlenecks first before splitting
- Significantly lower complexity and faster development

**Risk Reduction**:
- ✓ Eliminates distributed system complexity (Failure #1)
- ✓ Avoids premature optimization (Failure #2)
- ✓ No data consistency issues (Failure #3)
- ✓ Simple deployment (Failure #4)
- ✓ Lower infrastructure costs (Failure #5)

**When to Extract Services Later**:
- After measuring actual performance bottlenecks
- Team grows to 10+ developers (Conway's Law)
- Specific service needs independent scaling (e.g., background jobs)
- Domain boundaries are proven stable

---

#### Option 2: Proceed with Microservices + Mitigations (Accept Risk)

If microservices are truly necessary despite risks:

**For Failure #1 (Complexity)**:
- Hire distributed systems expert or consultant
- Start with 2-3 services MAX (not 10)
- Implement observability/tracing FIRST (OpenTelemetry, Jaeger)
- 3-month training period before production cutover

**For Failure #2 (Premature Optimization)**:
- Measure current monolith performance first
- Document specific bottleneck requiring microservices
- Accept 3x slower velocity in short term

**For Failure #3 (Data Consistency)**:
- Design saga patterns upfront for multi-service transactions
- Use event sourcing if applicable
- Document consistency requirements per domain (eventual vs strong)

**For Failure #4 (DevOps)**:
- Hire dedicated DevOps engineer
- Set up CI/CD infrastructure FIRST
- Use managed orchestration (AWS ECS or GKE, not raw Kubernetes)

**For Failure #5 (Cost)**:
- Do cost analysis: monolith vs microservices
- Share infrastructure where possible (combined databases for related services)
- Start small, scale incrementally

---

### Decision

**Recommendation**: PIVOT to Modular Monolith

**Rationale**:
- Risk level is HIGH (especially Failure #1 with CRITICAL impact)
- Team size (3 devs) is too small for microservices complexity
- No evidence that monolith cannot handle 100k users
- Microservices can be extracted later when scale actually demands it
- Cost and velocity advantages are significant for current stage

**If You Must Proceed with Microservices**:
- Reduce scope to 2-3 services maximum (not 10)
- Invest heavily in observability infrastructure FIRST
- Hire distributed systems expertise (consultant or full-time)
- Accept 3-6 month timeline extension for proper setup

**Lock Condition**:
To reconsider the modular monolith recommendation, provide:
- **Evidence monolith cannot scale**: Load testing shows bottleneck at < 100k users
- **Team growth**: Team grows to 10+ developers
- **Specific service needs**: Identified service requiring independent scaling
- **Distributed expertise**: Distributed systems expert hired

---

## Example 5: Pareto Analysis (Auto-Detected)

### Input
```
/decide we need to fix the checkout bug - full rewrite of checkout module vs refactor the broken functions vs quick hotfix patch
```

### Detection Logic
- 3 options with obvious effort differences (rewrite vs refactor vs patch) → Pareto Analysis
- Context suggests urgency ("need to fix") → efficiency keywords
- **Result**: Pareto Analysis framework

### Output

## Pareto Analysis (80/20 Efficiency): Checkout Bug Fix

### Value vs Effort Assessment

┌────────────────┬──────────────┬──────────────┬─────────────┬────────────────┐
│ Option         │ Value (0-10) │ Effort (0-10)│ Efficiency  │ Pareto Winner? │
├────────────────┼──────────────┼──────────────┼─────────────┼────────────────┤
│ Full Rewrite   │ 10 (perfect) │ 9 (weeks)    │ 1.11        │ ❌             │
│ Refactor       │ 8 (great)    │ 4 (days)     │ 2.00        │ ✓ WINNER       │
│ Quick Hotfix   │ 4 (minimal)  │ 1 (hours)    │ 4.00        │ ❌ Low value   │
└────────────────┴──────────────┴──────────────┴─────────────┴────────────────┘

**Efficiency = Value / Effort** (higher is better)

**Scoring rationale:**
- **Full Rewrite**: Value 10/10 - completely solves all checkout issues, clean architecture. Effort 9/10 - weeks of work, full module replacement, extensive testing needed.
- **Refactor**: Value 8/10 - fixes the bug plus improves surrounding code quality. Effort 4/10 - 2-3 days, targeted changes to broken functions.
- **Quick Hotfix**: Value 4/10 - patches the immediate symptom but underlying issues remain. Effort 1/10 - hours, minimal code change.

### Pareto Principle Applied

**Refactor** delivers 80% of maximum value with 44% of effort.

Key insight: Refactor achieves 80% of the Full Rewrite's outcome at less than half the effort. The remaining 20% (architectural cleanup) can be done later if needed.

### Threshold Check

**Critical Question**: Is 8/10 value sufficient for current needs?

- ✅ If yes → **Refactor** (optimal efficiency)
- ❌ If checkout has systemic issues beyond this bug → **Full Rewrite** (accept 9/10 effort for 10/10 value)

Note: Quick Hotfix has highest raw efficiency (4.0) but 4/10 value is likely insufficient - it only masks the symptom.

### Context Check

- **Time constraint**: Bug is blocking users, fix needed this week
- **Minimum acceptable value**: Must fully fix the bug (not just mask it) = 7/10 minimum
- **Risk tolerance for "good enough"**: Can iterate later - refactor now, rewrite in next quarter if needed

### Recommendation: Refactor

**Confidence**: Strong

**Rationale**: Refactor delivers 8/10 value at 4/10 effort (efficiency: 2.0). It fully fixes the bug, improves code quality, and takes days not weeks. Quick Hotfix is too shallow (4/10 value), Full Rewrite is overkill for one bug.

**When to choose Full Rewrite instead:**
- Checkout module has 5+ other known bugs (systemic issues)
- Architecture is fundamentally broken (not just these functions)
- Team has bandwidth for multi-week project

**Lock Condition**:
To reconsider this decision, provide:
- New requirement: "Checkout has 5+ other bugs that refactor won't fix"
- New constraint: "Timeline extended to next quarter, can afford full rewrite"
- Changed priority: "Long-term architecture quality matters more than shipping this week"

---

## Example 6: ICE Preset (Decision Matrix Variant)

### Input
```
/decide --preset=ice which feature to build next: dark mode, export to PDF, collaborative editing
```

### Framework Selection
- Explicit `--preset=ice` flag → Decision Matrix with Impact/Confidence/Ease (equal weights)

### Output

## Decision Matrix: Feature Prioritization (ICE Preset)

### Criteria Weights (ICE Preset - Equal Weights)
- **Impact** (33.33%): User value, business impact, competitive advantage
- **Confidence** (33.33%): Certainty of success, risk level, known patterns
- **Ease** (33.34%): Implementation effort, complexity, team familiarity

Total: 100% ✓

### Scoring Matrix

┌─────────────────────┬──────────────────┬──────────────────┬──────────────────┬───────────┐
│ Feature             │ Impact (33.33%)  │ Confidence (33.33%)│ Ease (33.34%)  │ **Total** │
├─────────────────────┼──────────────────┼──────────────────┼──────────────────┼───────────┤
│ Dark Mode           │ 6 → 2.00         │ 9 → 3.00         │ 8 → 2.67        │ **7.67**  │
│ Export to PDF       │ 8 → 2.67         │ 7 → 2.33         │ 6 → 2.00        │ **7.00**  │
│ Collaborative Edit  │ 10 → 3.33        │ 4 → 1.33         │ 2 → 0.67        │ **5.33**  │
└─────────────────────┴──────────────────┴──────────────────┴──────────────────┴───────────┘

### Mathematical Winner: Dark Mode (7.67)

**Rationale**:
Dark Mode wins despite lower Impact (6/10) because:
- Very high Confidence (9/10): Well-understood pattern, CSS-level changes, no backend risk
- High Ease (8/10): Straightforward implementation, team has done this before
- Collaborative Editing has highest Impact (10) but tanks on Confidence (4) and Ease (2)

**Sensitivity Analysis**:
To prefer Export to PDF over Dark Mode:
- Increase Impact weight from 33.33% to **50%** (shift from Confidence or Ease)

To prefer Collaborative Editing over Dark Mode:
- Increase Impact weight from 33.33% to **65%** (unrealistic with ICE equal-weight intent)

Decision is **moderately robust** - only shifts with significant Impact re-weighting.

### Recommendation: Dark Mode

**Confidence**: Strong (high certainty and ease compensate for moderate impact)

**Lock Condition**:
To reconsider this decision, provide:
- Changed weights: "Impact matters much more than Confidence/Ease" (breaks ICE equal-weight assumption)
- New information: "We just lost a major deal because we lack PDF export"
- New option: A higher-scoring feature emerges

---

*These examples demonstrate the full output format for each framework tier. Use them as templates when executing the /decide skill.*
