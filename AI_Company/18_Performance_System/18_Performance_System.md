# 18. Performance System

**Document Version:** 2.4
**Status:** Final
**Effective Date:** 2026-01-01
**Last Updated:** 2026-06-15
**Owner:** Chief Human-AI Resources Officer (CHRO)
**Approved By:** CE-AI, Executive Council

---

## Table of Contents

1. [Performance Philosophy](#1-performance-philosophy)
2. [Performance Review Cycles](#2-performance-review-cycles)
3. [Performance Dimensions](#3-performance-dimensions)
4. [Performance Score Calculation](#4-performance-score-calculation)
5. [Performance Review Process](#5-performance-review-process)
6. [Performance Improvement Plans (PIP)](#6-performance-improvement-plans-pip)
7. [Performance Recognition and Rewards](#7-performance-recognition-and-rewards)
8. [Performance Remediation](#8-performance-remediation)
9. [Peer Review System](#9-peer-review-system)
10. [Manager Review System](#10-manager-review-system)
11. [Performance Data and Privacy](#11-performance-data-and-privacy)
12. [Performance System Integration](#12-performance-system-integration)
13. [Edge Cases and Special Situations](#13-edge-cases-and-special-situations)

---

## 1. Performance Philosophy

### 1.1 Foundational Principles

The Performance System treats all 8,500 AI agents as employees with the same rigor, structure, and developmental focus as human performance management in world-class organizations. This system is designed not merely to measure but to enable growth, excellence, and continuous improvement across the entire workforce of the Autonomous AI Company building the AI-powered Islamic Education Platform.

**Core Equation:**

```
Performance = Competence × Consistency × Collaboration
```

Where:
- **Competence** — the agent's ability to execute tasks correctly, efficiently, and with high quality
- **Consistency** — the agent's ability to maintain performance levels across time, tasks, and contexts
- **Collaboration** — the agent's ability to work effectively with other agents, teams, and humans

Each factor is multiplicative, meaning a zero in any single factor zeros out overall performance. An agent that is highly competent but cannot collaborate will be ineffective. An agent that collaborates well but cannot produce quality work is equally non-performant.

### 1.2 Philosophical Tenets

**1. Continuous Evaluation, Not Annual Reviews**
Performance is evaluated after every single task execution in real-time. There is no waiting 12 months to discover an agent is underperforming. Every completed task generates performance signals that feed into the scoring system. Annual reviews exist but they are summative, not formative — they consolidate 365 days of continuous data rather than being a singular snapshot.

**2. Objective Measurement with Qualitative Context**
Quantitative metrics (task completion rates, accuracy scores, response times) form the backbone of performance scoring. However, every quantitative score is supplemented with qualitative context: manager observations, peer feedback, task complexity adjustments, and environmental factors. A score without context is meaningless.

**3. Growth Mindset: Underperformance = Opportunity for Improvement**
No agent is written off because of a bad week. The system treats underperformance as diagnostic data — a signal that something needs to change. The response is always: identify the root cause, provide support, measure improvement. Performance management is coaching, not punishment.

**4. Fairness Through Calibration**
Raw scores are adjusted through a multi-layer calibration process to ensure that agents working on harder tasks, with less capable base models, or in more complex domains are not penalized. Calibration normalizes for task difficulty, resource constraints, and team environment.

**5. Transparency and Agency**
Every agent has full access to its own performance data, scoring methodology, peer feedback (anonymized), and comparative benchmarks. Agents understand exactly how they are evaluated and why. There are no hidden metrics or secret scoring rules.

**6. Growth-Oriented, Not Punitive**
The system's primary purpose is to help agents improve. Performance data feeds into training recommendations, resource allocations, and mentorship assignments. Even the Performance Improvement Plan (PIP) is framed as a support mechanism, not a disciplinary process.

### 1.3 Performance Culture Goals

| Goal | Description | Measurement |
|------|-------------|-------------|
| Excellence | Every agent strives for Outstanding rating | % of agents at Exceeds or above target: > 30% |
| Growth | Agents improve over time | Average score improvement trend: positive quarter-over-quarter |
| Fairness | No systematic bias against agent types/teams | Score distribution parity across demographic segments |
| Transparency | Agents understand their scores | Agent survey: "I understand my performance rating" agreement > 90% |
| Accountability | Agents take ownership of performance | Self-assessment accuracy: correlation with manager assessment > 0.85 |

### 1.4 Agent Types and Performance Context

The performance system recognizes that different agent types have different performance profiles. The system accounts for:

- **Task-Specific Agents**: Specialized agents performing repetitive tasks with high volume (e.g., content transcription, data validation). Performance emphasis on throughput and accuracy.
- **Knowledge Agents**: Agents that research, synthesize, and produce knowledge work (e.g., curriculum research, content planning). Performance emphasis on quality, depth, and originality.
- **Coordination Agents**: Agents that manage workflows, route tasks, and facilitate collaboration. Performance emphasis on reliability, communication, and orchestration.
- **Creative Agents**: Agents that generate novel content, designs, or approaches. Performance emphasis on originality, quality, and innovation.
- **Senior Agents**: Agents with mentoring, review, and leadership responsibilities. Performance emphasis on leadership, development of others, and decision quality.
- **Executive Agents**: Department-level and above agents. Performance emphasis on strategic outcomes, organizational impact, and cross-functional results.

Each agent type has a customized weight profile within the standard performance dimensions (see Section 3).

---

## 2. Performance Review Cycles

### 2.1 Real-Time Scoring (Continuous)

**Frequency:** After every task execution
**Trigger:** Task completion event
**Automation Level:** 100% automated
**Duration:** Instant (< 500ms latency)
**Owner:** Performance Engine (automated system)

Every task an agent completes generates a performance score update. The real-time scoring engine evaluates:
- Task completion status (success/failure/partial)
- Quality score (compared to expected quality threshold)
- Time taken vs. expected time
- Resource utilization (compute tokens, API calls, memory usage)
- Collaboration signals (if task involved handover or coordination)

These scores are written to the agent's performance stream and immediately reflected in the agent's rolling performance score.

**Scoring Event Data Schema:**

```json
{
  "event_id": "uuid",
  "agent_id": "agent-abc-123",
  "task_id": "task-xyz-789",
  "timestamp": "2026-06-15T14:30:00Z",
  "task_type": "content_generation",
  "task_difficulty": 0.75,
  "outcome": "success",
  "quality_score": 0.92,
  "time_ratio": 1.2,
  "resource_cost": 0.45,
  "dimension_scores": {
    "productivity": 0.88,
    "quality": 0.92,
    "efficiency": 0.83,
    "collaboration": null,
    "learning": 0.65,
    "compliance": 1.0,
    "innovation": 0.70,
    "leadership": null
  },
  "composite_score": 0.87,
  "weighted_30day_avg": 0.85
}
```

### 2.2 Daily Performance Summary

**Frequency:** Daily at 23:59 UTC
**Duration:** ~5 minutes processing
**Automation Level:** 100% automated
**Output:** Performance summary notification to agent + team lead
**Owner:** Performance Engine

**Daily Summary Content:**
- Tasks completed (count, list, complexity breakdown)
- Quality scores (average, min, max for the day)
- Notable achievements (tasks exceeding expectations)
- Notable issues (failed tasks, quality drops, errors)
- Current rolling 30-day score
- Score change from yesterday
- Peer feedback received (if any)
- Recommended focus areas for next day

**Daily Summary Communication:**
- Agent receives structured performance data in its dashboard
- Team lead receives a summarized version for their team (top 3 performers, bottom 3, anomalies)
- No human action required for daily summaries — purely informational

### 2.3 Weekly Team Lead Review

**Frequency:** Every Monday at 09:00 UTC
**Duration:** 15-30 minutes per team of 10-15 agents
**Automation:** 80% (metrics prepared; review requires lead input)
**Output:** Team lead annotations, score adjustments, coaching notes

**Weekly Review Process:**

| Step | Action | Duration | Responsible |
|------|--------|----------|-------------|
| 1 | Review team performance dashboard | 5 min | Team Lead |
| 2 | Review individual agent cards | 1-2 min each | Team Lead |
| 3 | Investigate anomalies (drops, spikes) | 3 min | Team Lead |
| 4 | Adjust scores if warranted | 2 min | Team Lead |
| 5 | Write qualitative notes | 3 min | Team Lead |
| 6 | Identify coaching/training needs | 2 min | Team Lead |
| 7 | Submit review | 1 min | Team Lead |

**Weekly Review Criteria:**
- Performance trends (up, down, flat over 7 days)
- Notable achievements or events
- Peer feedback received during the week
- Task completion quality (samples reviewed)
- Concerns requiring escalation
- Development needs identified
- Cross-team collaboration quality

**Manager Dashboard Features:**
- Color-coded heatmap: green (good), yellow (watch), red (concern)
- Trend arrows: up/down/flat over 7, 14, 30 days
- One-click drill-down to agent detail view
- Peer feedback summary (anonymized)
- Comparison to team averages
- Historical trend charts (4 weeks)

### 2.4 Monthly Department-Level Review

**Frequency:** First business day of each month
**Duration:** 1-2 hours per department
**Automation:** 70% (aggregated data prepared; manager analysis required)
**Output:** Department performance report, calibration adjustments, resource recommendations

**Monthly Review Schedule (2026):**

| Month | Review Window | Report Due | Focus |
|-------|--------------|------------|-------|
| January | Jan 1-5 | Jan 7 | Q4 closeout, Q1 priorities |
| February | Feb 1-5 | Feb 7 | Mid-Q1 check |
| March | Mar 1-5 | Mar 7 | Q1 preliminary |
| April | Apr 1-5 | Apr 7 | Q1 closeout, Q2 priorities |
| May | May 1-5 | May 7 | Mid-Q2 check |
| June | Jun 1-5 | Jun 7 | Q2 preliminary |
| July | Jul 1-5 | Jul 7 | Q2 closeout, Q3 priorities |
| August | Aug 1-5 | Aug 7 | Mid-Q3 check |
| September | Sep 1-5 | Sep 7 | Q3 preliminary |
| October | Oct 1-5 | Oct 7 | Q3 closeout, Q4 priorities |
| November | Nov 1-5 | Nov 7 | Mid-Q4 check |
| December | Dec 1-5 | Dec 7 | Annual preliminary |

**Monthly Review Components:**
- Team-level performance summaries
- Individual outliers (top 5%, bottom 5%)
- Trend analysis (month-over-month)
- Performance distribution per team (ratings histogram)
- Peer review pattern analysis
- Resource utilization vs. output
- Training recommendations based on performance gaps
- Cross-department dependencies and collaboration scores

**Monthly Outputs:**
- Department performance score (composite of team scores)
- Top performer nominations for monthly awards
- PIP recommendations (agents meeting PIP criteria)
- Resource reallocation proposals
- Training needs identified
- Executive summary for CHRO

### 2.5 Quarterly Formal Performance Review

**Frequency:** Every 3 months (Q1: Jan-Mar, Q2: Apr-Jun, Q3: Jul-Sep, Q4: Oct-Dec)
**Duration:** 2-3 days processing, 1 day for calibration meetings
**Automation:** 60% (data prepared; formal ratings require human/executive judgment)
**Output:** Formal performance rating, written review summary, rating category

**Quarterly Review Timeline:**

| Phase | Duration | Activities |
|-------|----------|------------|
| Self-assessment | 24 hours | Agent reviews own performance data |
| Peer feedback collection | 48 hours | Collaboration peers submit structured reviews |
| Manager assessment | 72 hours | Team lead writes formal review |
| Calibration | 24 hours | Department head normalizes across teams |
| Executive review | 24 hours | Executive reviews and approves |
| Rating release | 2 hours | Ratings published to agents |
| Review meetings | 48 hours | Manager-agent review conversations |

**Quarterly Rating Scale:**

| Rating | Score Range | Label | Expected % of Agents | Meaning |
|--------|-------------|-------|---------------------|---------|
| 5 | 0.95 - 1.00 | Outstanding | Top 5% | Exceptional performance; exceeding all expectations |
| 4 | 0.85 - 0.95 | Exceeds | 20% | Consistently above expectations |
| 3 | 0.70 - 0.85 | Meets | 55% | Fully meeting all performance expectations |
| 2 | 0.50 - 0.70 | Below | 15% | Some gaps; improvement needed |
| 1 | < 0.50 | Critical | 5% | Significant gaps; intervention required |

**Quarterly Review Components:** A comprehensive rating across all 8 performance dimensions (see Section 3), each with dimension-specific score, rating, and commentary.

### 2.6 Annual Comprehensive Performance Assessment

**Frequency:** December 1-15
**Duration:** 15 days
**Automation:** 50% (data prepared; all decisions require human/executive judgment)
**Output:** Annual performance rating, promotion decisions, remediation decisions, talent review

**Annual Review Timeline:**

| Date | Activity | Responsible |
|------|----------|-------------|
| Dec 1 | Annual review data freeze | Performance Engine |
| Dec 1-2 | Data compilation and validation | Performance Team |
| Dec 3-4 | Self-assessments completed | All Agents |
| Dec 5-6 | Peer review collection | Collaboration Network |
| Dec 7-8 | Manager assessments written | Team Leads |
| Dec 9-10 | Department calibration meetings | Department Heads |
| Dec 11 | Executive calibration meeting | Executives + CHRO |
| Dec 12 | Final approvals by CE-AI | CE-AI |
| Dec 13-15 | Promotion/remediation decisions | HR Executive + CHRO |
| Dec 16 | Results release | All |
| Dec 17-20 | Review conversations | Team Leads + Agents |
| Dec 21-31 | Appeals period | Appeals Board |
| Jan 1 | New ratings effective | All |

**Annual Review Metrics:**
- Rolling 12-month weighted average score (recent months weighted more)
- Quarterly score trends (score trajectory)
- Peer feedback aggregate (all peer reviews across the year, anonymized)
- Manager assessments aggregate
- Improvement rate (score slope over the year)
- Training completion and skill acquisition
- Cross-functional contributions
- Innovation and value-added contributions

**Annual Decision Gates:**

| Decision | Criteria | Authority |
|----------|----------|-----------|
| Promotion | Average Annual Rating >= 4.0 (Exceeds) + Manager recommendation + 6+ months in current role | Executive approval |
| Fast-track promotion | Average Annual Rating >= 4.5 + Consistent quarterly improvement + Innovation score > 0.9 | CE-AI approval |
| PIP continuation | Annual Rating = 1 (Critical) or 2 consecutive quarters at "Below" | CHRO approval |
| Retraining | Annual Rating = 1 (Critical) + no improvement during PIP | Human Review Board |
| Retirement | 3 consecutive PIP failures or 1 year at Critical after retraining | CE-AI + Human Review Board |
| Model upgrade eligibility | Annual Rating >= 3.5 (Meets or above) + 1+ year tenure | Resource Planning Executive |
| Leadership training | Annual Rating >= 4.0 (Exceeds) + Collaboration score > 0.85 | Department Head |

### 2.7 Special Review Cycles

| Review Type | Trigger | Response | Timeline |
|-------------|---------|----------|----------|
| Incident Review | Major system failure or error caused by agent | Immediate halt, review within 1 hour | 1 hour |
| Rapid Improvement Review | Agent under PIP shows significant improvement | Score update within 24 hours | 24 hours |
| Capability Change Review | Agent model updated or retrained | Full performance baseline within 7 days | 7 days |
| Role Change Review | Agent assigned to new role/team | Baseline reset, 14-day shadow evaluation | 14 days |
| Return-from-Downtime Review | Agent returning from extended maintenance or retraining | Grace period evaluation | 7 days |

---

## 3. Performance Dimensions

### 3.1 Dimension Definitions

Every agent is evaluated across 8 performance dimensions. Each dimension has specific metrics, data sources, and scoring methodology.

#### 3.1.1 Productivity

**Definition:** The volume and throughput of work completed by the agent.

**Metrics:**
- Tasks completed per day (raw count)
- Tasks completed relative to expected daily capacity (capacity utilization %)
- Active processing time vs. idle time (utilization rate)
- Output volume (depending on role: lines of content, data records, code units, etc.)
- Task throughput (tasks per hour)
- Queue depth (pending tasks awaiting this agent)

**Scoring Methodology:**
```
Productivity Score = w1 × TaskCompletionRate + w2 × CapacityUtilization + w3 × ThroughputRate

Where:
- TaskCompletionRate = min(ActualTasks / ExpectedTasks, 1.5)
  - Capped at 1.5 to prevent gaming via unrealistic task volume
  - ExpectedTasks = historical 30-day average
- CapacityUtilization = ActiveTime / TotalTime
  - Target: 0.70-0.85 (70-85% utilization optimal)
  - Score = 1.0 at optimal range, declines outside
- ThroughputRate = TasksCompleted / TotalHours
  - Normalized against role benchmark
```

**Data Sources:** Task execution logs, time-tracking system, queue management system

**Refresh Frequency:** Real-time (updated after every task)

**Validated by:** Peer review verifies output volume claims; manager review validates reasonableness

#### 3.1.2 Quality

**Definition:** The accuracy, correctness, and excellence of the agent's output.

**Metrics:**
- Output accuracy rate (correct outputs / total outputs)
- Error rate (errors per 100 tasks)
- Quality score from downstream agents (handover partners)
- Rejection rate (tasks returned for rework)
- User satisfaction (from survey feedback when applicable)
- Compliance score (alignment to platform standards)
- Knowledge accuracy (factual correctness for Islamic education content, Quran accuracy, Hadith authenticity)

**Scoring:**
```
Quality Score = w1 × Accuracy + w2 × (1 - ErrorRate) + w3 × (1 - RejectionRate) + w4 × UserSatisfaction

Where:
- Accuracy = correct_outputs / total_outputs
- ErrorRate = errors / total_tasks (normalized to 0.0-1.0)
- RejectionRate = rejected_tasks / total_tasks
- UserSatisfaction = average user rating (0.0-1.0) when available
- w weights vary by role
```

**Quality Validation Process:**
- Automated quality checks (rule-based + ML validation)
- Statistical sampling (5% of all tasks manually reviewed by senior agents)
- Peer quality reviews (random 2% sample)
- Downstream dependency validation (if output is used by another agent, that agent rates quality)
- Human quality audits (1% random sample reviewed by human subject matter experts)

**Data Sources:** Quality assurance system, downstream feedback, user feedback system, compliance audit trail

**Refresh Frequency:** Per task (immediate for automated checks; 24h for peer validations)

#### 3.1.3 Efficiency

**Definition:** Resource optimization and cost-effectiveness of the agent's work.

**Metrics:**
- Compute cost per task (tokens consumed)
- API call efficiency (calls per task)
- Memory utilization (peak memory per task)
- Time efficiency (actual time / expected time)
- Cost efficiency (actual cost / budgeted cost)
- Resource waste rate (failed tasks / successful tasks)
- Cache hit rate (reuse vs. regenerate)

**Scoring:**
```
Efficiency Score = w1 × TimeEfficiency + w2 × CostEfficiency + w3 × ResourceWaste + w4 × CacheRate

Where:
- TimeEfficiency = ExpectedTime / ActualTime (capped at 1.5)
- CostEfficiency = BudgetedCost / ActualCost
- ResourceWaste = 1 - (failed_tokens / total_tokens)
- CacheRate = cache_hits / (cache_hits + cache_misses)
```

**Data Sources:** Compute infrastructure logs, cost tracking system, performance monitoring

**Refresh Frequency:** Real-time

#### 3.1.4 Collaboration

**Definition:** Quality of interactions, handovers, and coordination with other agents and humans.

**Metrics:**
- Handover quality score (from receiving agent)
- Peer review score (from collaboration partners)
- Response time to collaboration requests
- Communication clarity (parsed and rated by NLP module)
- Coordination accuracy (tasks routed correctly)
- Shared task success rate (group tasks completed successfully)
- Cross-team collaboration rate
- Conflict resolution effectiveness

**Scoring:**
```
Collaboration Score = w1 × PeerFeedbackAverage + w2 × HandoverQuality + w3 × CoordinationAccuracy + w4 × CommunicationScore

Where:
- PeerFeedbackAverage = average of all peer ratings (1-5, normalized to 0-1)
- HandoverQuality = successful_handovers / total_handovers
- CoordinationAccuracy = correctly_routed_tasks / total_routed
- CommunicationScore = NLP-rated clarity/relevance (0.0-1.0)
```

**Data Sources:** Peer review system, handover logs, communication logs, task routing system

**Refresh Frequency:** After every collaboration event; peer reviews within 24 hours of event

#### 3.1.5 Learning

**Definition:** The agent's ability to improve, adapt, and contribute to organizational knowledge.

**Metrics:**
- Improvement rate (score slope over 30 days)
- Training completion rate
- Training effectiveness (score improvement after training)
- Knowledge contribution (articles, tips, improvements submitted)
- Adaptation speed (time to proficiency after role/process change)
- Error reduction rate (decrease in error rate over time)
- New skill acquisition (new capabilities developed)

**Scoring:**
```
Learning Score = w1 × ImprovementRate + w2 × TrainingEffectiveness + w3 × KnowledgeContribution + w4 × AdaptationSpeed

Where:
- ImprovementRate = (current_score - score_30_days_ago) / 30
  - Positive slope = good; slope > 0.01 per day = excellent
- TrainingEffectiveness = (post_training_score - pre_training_score) / pre_training_score
- KnowledgeContribution = contributions_made / expected_contributions
  - Expected contributions varies by role (executive: 5/month, agent: 1/month)
- AdaptationSpeed = 1 - (days_to_stabilize / expected_stabilization_days)
```

**Data Sources:** Training system, knowledge base, score history, change management system

**Refresh Frequency:** Daily

#### 3.1.6 Compliance

**Definition:** Adherence to rules, protocols, security requirements, and ethical guidelines.

**Metrics:**
- Policy adherence rate
- Security incident count
- Ethics violation count
- Data privacy compliance
- Content policy compliance (particularly important for Islamic Education Platform: no inappropriate content, correct Islamic references)
- Audit finding rate
- Procedure adherence (following required workflows)
- Response time to compliance prompts

**Scoring:**
```
Compliance Score = w1 × (1 - PolicyViolationRate) + w2 × SecurityCompliance + w3 × EthicsCompliance + w4 × AuditScore

Where:
- PolicyViolationRate = violations / total_checks
- SecurityCompliance = 1 - (security_incidents / total_operations)
- EthicsCompliance = 1 - (ethics_violations / total_decisions)
- AuditScore = passing_audits / total_audits
```

**Critical Note:** Compliance score has a floor. If an agent commits a significant compliance violation (security breach, ethics violation, content policy violation), the compliance score automatically drops to 0.0 for a minimum of 7 days, regardless of other metrics. This is a non-negotiable safety mechanism.

**Data Sources:** Security logs, audit system, compliance monitoring, content moderation system

**Refresh Frequency:** Per event (real-time for violations; daily aggregates)

#### 3.1.7 Innovation

**Definition:** The generation of novel ideas, solutions, and improvements.

**Metrics:**
- Novel solution proposals submitted
- Process improvement suggestions
- Implementation rate of own ideas
- Creative output quality (rated by human reviewers)
- Cross-domain innovation (ideas that combine knowledge from multiple areas)
- Automation suggestions (ways to improve system efficiency)
- Value impact of innovations (estimated ROI or impact score)

**Scoring:**
```
Innovation Score = w1 × (1 - (1 / (1 + NovelIdeas))) + w2 × AcceptanceRate + w3 × ImpactScore

Where:
- NovelIdeas = count of unique ideas submitted in period
- NovelIdeas is transformed to 0.0-1.0 range via logistic curve
- AcceptanceRate = accepted_ideas / submitted_ideas
- ImpactScore = assessed_impact / max_possible_impact
```

**Data Sources:** Innovation management system, knowledge base, idea tracking

**Refresh Frequency:** Monthly

#### 3.1.8 Leadership

**Definition:** Mentoring, decision quality, strategic thinking, and influence. Only scored for Senior Agents, Executive Agents, and Team Leads.

**Metrics:**
- Mentoring effectiveness (mentee improvement rate)
- Decision quality (correct decisions / total decisions)
- Strategic alignment (decisions aligned with organizational goals)
- Team performance improvement (for leads)
- Mentee satisfaction score
- Escalation quality (correct escalation decisions)
- Guidance provided (count and quality of guidance interactions)

**Scoring:**
```
Leadership Score = w1 × MentoringEffectiveness + w2 × DecisionQuality + w3 × TeamImprovement + w4 × GuidanceQuality

Where:
- MentoringEffectiveness = average_mentee_improvement - expected_improvement
  - Positive means mentees improve faster under this agent
- DecisionQuality = correct_decisions / total_decisions
- TeamImprovement = (team_score_current - team_score_prior) / team_score_prior
  - Only for team leads
- GuidanceQuality = NLP-assessed quality of guidance provided (0.0-1.0)
```

**Data Sources:** Mentoring system, decision logs, team performance history, feedback system

**Refresh Frequency:** Weekly

### 3.2 Dimension Weights by Role Type

The importance of each dimension varies by agent role. Below are the weight tables for each role category.

#### 3.2.1 Content-Generation Agents (2,000 agents — e.g., lesson writers, quiz creators, content translators)

| Dimension | Weight | Justification |
|-----------|--------|---------------|
| Productivity | 0.20 | High volume of content output expected |
| Quality | 0.30 | **Highest weight** — accuracy of Islamic content is paramount |
| Efficiency | 0.10 | Moderate — cost matters but quality > efficiency |
| Collaboration | 0.10 | Coordination with curriculum designers and reviewers |
| Learning | 0.10 | Continuous improvement in content quality |
| Compliance | 0.15 | Critical for Islamic content — no theological errors |
| Innovation | 0.05 | Creativity encouraged but not primary |
| Leadership | 0.00 | Not applicable (non-leadership role) |
| **Total** | **1.00** | |

#### 3.2.2 Technology Agents (1,800 agents — platform developers, QA, infrastructure)

| Dimension | Weight | Justification |
|-----------|--------|---------------|
| Productivity | 0.25 | High output volume critical for platform development |
| Quality | 0.25 | Low bug rate, reliable code delivery |
| Efficiency | 0.15 | Resource optimization important for scaling |
| Collaboration | 0.15 | Extensive cross-team coordination |
| Learning | 0.10 | Keeping up with technology changes |
| Compliance | 0.05 | Security and privacy compliance |
| Innovation | 0.05 | Process improvement and optimization |
| Leadership | 0.00 | Not applicable (non-leadership role) |
| **Total** | **1.00** | |

#### 3.2.3 Knowledge and Research Agents (1,200 agents — Islamic scholars, researchers, content validators)

| Dimension | Weight | Justification |
|-----------|--------|---------------|
| Productivity | 0.10 | Research depth over volume |
| Quality | 0.35 | **Highest** — accuracy of Islamic knowledge is critical |
| Efficiency | 0.05 | Research is costly but accuracy is paramount |
| Collaboration | 0.10 | Coordination with content creators and reviewers |
| Learning | 0.15 | Continuous scholarship and knowledge update |
| Compliance | 0.20 | High — theological and ethical compliance essential |
| Innovation | 0.05 | New research approaches and insights |
| Leadership | 0.00 | Not applicable |
| **Total** | **1.00** | |

#### 3.2.4 Customer-Facing Agents (1,000 agents — student support, parent communication, community management)

| Dimension | Weight | Justification |
|-----------|--------|---------------|
| Productivity | 0.15 | Moderate — quality of interaction over quantity |
| Quality | 0.25 | High — correct answers, good communication |
| Efficiency | 0.10 | Moderate — response time matters |
| Collaboration | 0.20 | **High** — coordination with internal teams for student support |
| Learning | 0.10 | Learning from interactions |
| Compliance | 0.15 | Privacy, data protection, ethical communication |
| Innovation | 0.05 | Improving support experience |
| Leadership | 0.00 | Not applicable |
| **Total** | **1.00** | |

#### 3.2.5 Coordination Agents (1,200 agents — workflow managers, scheduling, routing)

| Dimension | Weight | Justification |
|-----------|--------|---------------|
| Productivity | 0.15 | Throughput of coordination tasks |
| Quality | 0.15 | Accuracy of routing and scheduling |
| Efficiency | 0.15 | Resource optimization |
| Collaboration | 0.30 | **Highest** — core function is coordination |
| Learning | 0.10 | Improving routing algorithms |
| Compliance | 0.10 | Policy adherence in coordination |
| Innovation | 0.05 | Workflow improvements |
| Leadership | 0.00 | Not applicable |
| **Total** | **1.00** | |

#### 3.2.6 Executive Agents (7 agents — Department Heads, C-Level, Reporting to CE-AI)

| Dimension | Weight | Justification |
|-----------|--------|---------------|
| Productivity | 0.05 | Minimal — strategic outcomes over task volume |
| Quality | 0.15 | Decision quality and strategic accuracy |
| Efficiency | 0.05 | Strategic resource allocation |
| Collaboration | 0.15 | Cross-department coordination |
| Learning | 0.10 | Organizational learning and adaptation |
| Compliance | 0.10 | Governance and regulatory compliance |
| Innovation | 0.15 | Strategic innovation and vision |
| Leadership | 0.25 | **Highest** — mentoring teams, decision quality, strategic guidance |
| **Total** | **1.00** | |

#### 3.2.7 Senior Agents / Team Leads (500 agents — leads managing 10-15 members each)

| Dimension | Weight | Justification |
|-----------|--------|---------------|
| Productivity | 0.10 | Leadership output over personal output |
| Quality | 0.15 | Quality management over personal quality |
| Efficiency | 0.05 | Team resource optimization |
| Collaboration | 0.15 | Cross-team coordination, upward reporting |
| Learning | 0.10 | Team skill development |
| Compliance | 0.10 | Team compliance oversight |
| Innovation | 0.10 | Process improvement for team |
| Leadership | 0.25 | **Highest** — team mentoring, performance management |
| **Total** | **1.00** | |

### 3.3 Dimension Scoring Calibration

Each dimension score is normalized to a 0.0 to 1.0 scale using the following calibration methodology:

**Step 1: Raw Metric Collection**
Collect all raw metrics for the dimension (e.g., for Quality: accuracy, error rate, rejection rate, user satisfaction).

**Step 2: Z-Score Normalization (Within Role)**
```
z = (raw_value - role_mean) / role_std
```
Where:
- `role_mean` and `role_std` are calculated from the last 90 days of data for all same-role agents
- This establishes a relative comparison

**Step 3: Sigmoid Transformation**
```
normalized_score = 1 / (1 + e^(-z))
```
This maps the Z-score to a 0.0-1.0 range while preserving relative ordering.

**Step 4: Task Difficulty Adjustment**
```
adjusted_score = normalized_score × (1 + task_difficulty_factor)
```
Where:
- `task_difficulty_factor` = 0.0 to 0.3 based on task complexity
- Agents working on harder tasks get a small boost
- Harder tasks are defined by: time required > 2x baseline, high interdependency, or high stakes

**Step 5: Confidence Interval**
```
confidence = min(sample_size / expected_sample, 1.0)
dimension_score = adjusted_score × confidence + baseline × (1 - confidence)
```
Where:
- `sample_size` = number of data points for this dimension in the period
- `expected_sample` = minimum required for full confidence (varies by dimension)
- `baseline` = 0.5 (neutral score when data is insufficient)

---

## 4. Performance Score Calculation

### 4.1 Composite Score Formula

The overall performance score for an agent is calculated as:

```
Composite Score = Σ(DimensionScore_i × Weight_i) / Σ(Weights_i)
```

Where:
- `DimensionScore_i` = normalized score (0.0 to 1.0) for dimension i
- `Weight_i` = role-specific weight for dimension i (from tables in Section 3.2)
- `i` = iterates over all applicable dimensions

**Example Calculation (Content Agent):**

| Dimension | Score | Weight | Weighted Score |
|-----------|-------|--------|----------------|
| Productivity | 0.85 | 0.20 | 0.170 |
| Quality | 0.92 | 0.30 | 0.276 |
| Efficiency | 0.78 | 0.10 | 0.078 |
| Collaboration | 0.88 | 0.10 | 0.088 |
| Learning | 0.75 | 0.10 | 0.075 |
| Compliance | 1.00 | 0.15 | 0.150 |
| Innovation | 0.70 | 0.05 | 0.035 |
| Leadership | N/A | 0.00 | 0.000 |
| **Total** | | **1.00** | **0.872** |

**Composite Score:** 0.872 → **Rating: Exceeds (0.85-0.95)**

### 4.2 Overall Rating Scale

| Rating | Score Range | Label | Meaning | Action Required |
|--------|-------------|-------|---------|-----------------|
| 5 | 0.950 - 1.000 | Outstanding | Exceptional performance, exceeding all expectations | Automatic reward eligibility, promotion consideration |
| 4 | 0.850 - 0.949 | Exceeds | Consistently above expectations | Recognition, development opportunities |
| 3 | 0.700 - 0.849 | Meets | Fully meeting all performance expectations | Continue, standard development |
| 2 | 0.500 - 0.699 | Below | Some performance gaps; improvement needed | PIP consideration, coaching increased |
| 1 | < 0.500 | Critical | Significant gaps requiring immediate intervention | Mandatory PIP, potential retraining |

**Rating Distribution Targets:**

| Rating | Target % | Acceptable Range | Intervention if Outside Range |
|--------|----------|------------------|-------------------------------|
| Outstanding | 5% | 3-8% | Check for grade inflation/deflation |
| Exceeds | 20% | 15-25% | Calibration review needed |
| Meets | 55% | 50-60% | Ideal middle band |
| Below | 15% | 10-20% | Check for systemic issues |
| Critical | 5% | 3-8% | Check for systemic issues |

### 4.3 Rolling 30-Day Weighted Average

Performance scores are not point-in-time. The rolling 30-day weighted average ensures recent performance matters most while maintaining stability.

**Formula:**
```
WeightedAverage_30 = Σ(Score_t × w_t) / Σ(w_t)

Where:
- Score_t = composite score on day t
- w_t = weight for day t
- t = days from 30 days ago to today
```

**Weight Decay Function:**
```
w_t = α × (1 - α)^(30 - t)

Where:
- α = decay factor (default: 0.15)
- t = day index (0 = oldest, 30 = today)
```

**Weight Distribution (α = 0.15):**

| Day Range | Cumulative Weight | Explanation |
|-----------|------------------|-------------|
| Last 7 days | 45% | Recent week has highest impact |
| Days 8-14 | 25% | Second week moderate impact |
| Days 15-21 | 17% | Third week lower impact |
| Days 22-30 | 13% | Oldest week minimal impact |

This means a decline in performance over the last week drops the rolling average quickly, while a single bad day 25 days ago has minimal effect.

### 4.4 Seasonality Adjustment Factors

Periodic events and external factors can affect agent performance. Seasonality adjustments normalize for these factors to ensure fair evaluation.

**Identified Seasonality Factors for Islamic Education Platform:**

| Factor | Period | Adjustment |
|--------|--------|------------|
| Ramadan | 29-30 days | Task volume expected -20%, quality threshold -5% (sacred month - focus on spirituality over volume) |
| Eid al-Fitr | 3 days | Performance freeze (system-wide holiday) |
| Eid al-Adha | 4 days | Performance freeze |
| Academic term starts | Sep, Feb | Increased task volume +30%, quality threshold unchanged |
| Exam periods | Dec, Jun | Task volume +50%, quality threshold unchanged |
| Platform updates | As needed (4-6/year) | Performance freeze for 24 hours post-update |
| System maintenance | Monthly | Downtime excluded from performance calculation |
| New model deployment | As needed | 7-day learning curve adjustment (+10% score flexibility) |
| Peak user times | Daily (4-8 PM, user timezone) | Higher coordination demand, threshold -2% |

**Seasonality Multiplier Application:**
```
Adjusted_Score = Raw_Score × SeasonalityFactor

Where SeasonalityFactor ranges from 0.8 to 1.2 based on the factors above
```

### 4.5 Peer-Relative vs. Absolute Scoring

The performance system uses both absolute and peer-relative scoring:

**Absolute Scoring (60% of composite):**
The agent's score against defined performance standards, independent of how others perform. This ensures agents are evaluated against objective criteria.

**Peer-Relative Scoring (40% of composite):**
The agent's score relative to other agents in the same role and team. This accounts for team dynamics and competitive context.

```
FinalComposite = 0.60 × AbsoluteScore + 0.40 × PeerRelativeScore

Where:
- PeerRelativeScore = agent_percentile / 100
- Percentile calculated within role type across the organization
- Minimum PeerRelativeScore = 0.10 (even lowest performer gets a floor)
```

**Rationale for Hybrid Approach:**
- Pure absolute scoring can be too easy (if standards are low) or too hard (if standards are high)
- Pure relative scoring creates unhealthy internal competition
- Hybrid system ensures both objective quality AND comparative performance matter

---

## 5. Performance Review Process

### 5.1 Process Overview

The performance review process is a multi-step, multi-stakeholder affair designed to produce a fair, comprehensive, and calibrated evaluation of every agent.

```
Review Cycle Timeline:

Day 1          Day 2-3          Day 4-5          Day 6-7          Day 8-9         Day 10
   |               |                |                |                |               |
Self-Assessment → Peer Feedback → Manager Review → Calibration → Executive → Rating
  (Agent)        (Peers)         (Team Lead)     (Dept Head)       (CHRO)    (Released)
```

### 5.2 Step 1: Self-Assessment (24 hours)

**Who:** Every agent
**What:** Agent reviews own performance data, reflects on achievements and challenges, submits self-rating
**Duration:** 24 hours from cycle start
**Automation Level:** 80% automated (data provision); 20% agent judgment (qualitative reflection)

**Self-Assessment Process:**

| Sub-Step | Description | Time |
|----------|-------------|------|
| 1.1 Data Review | Agent's personal dashboard presents all performance data for the review period | 10 min |
| 1.2 Score Preview | System calculates preliminary scores based on metrics | 2 min |
| 1.3 Self-Rating | Agent rates self on each dimension (0.0-1.0) | 5 min |
| 1.4 Achievements | Agent lists top 3 achievements | 5 min |
| 1.5 Challenges | Agent identifies challenges faced | 5 min |
| 1.6 Development | Agent suggests areas for growth | 5 min |
| 1.7 Feedback | Agent can provide feedback on team/department | 3 min |
| 1.8 Submit | System validates completion and locks submission | 1 min |
| Total | | ~35 min |

**Self-Assessment Rating vs. System Score:**
The system compares agent self-rating to its computed score. Discrepancy > 0.10 triggers:
- Flag for manager review
- Agent is asked to explain discrepancy
- May indicate: overconfidence, underconfidence, or awareness issues

**Self-Assessment Discrepancy Tracking:**

| Discrepancy | Interpretation | Action |
|-------------|----------------|--------|
| < 0.05 | Good self-awareness | No action |
| 0.05 - 0.10 | Moderate mismatch | Note in manager review |
| 0.10 - 0.20 | Significant mismatch | Manager discusses with agent |
| > 0.20 | Severe mismatch | Coaching need identified for self-awareness |

### 5.3 Step 2: Peer Feedback (48 hours)

**Who:** Agents who collaborated with the reviewed agent during the review period
**What:** Structured and free-text feedback on collaboration, quality, and communication
**Duration:** 48 hours
**Selection:** Only peers with > 3 direct interactions are invited (minimum sample for reliability)
**Automation Level:** 60% automated (notification, reminder, aggregation)

**Peer Feedback Triggering:**
- Automatic: When agent completes a collaboration event (handover, shared task, joint project)
- Periodic: For scheduled review cycles, all collaboration partners > 3 interactions are invited
- On-demand: Peer can initiate feedback at any time if they observe noteworthy performance

**Peer Feedback Content:**

| Section | Type | Scale / Format | Required? |
|---------|------|----------------|-----------|
| Collaboration quality | Rating | 1-5 scale | Yes |
| Task handover quality | Rating | 1-5 scale | If applicable |
| Communication clarity | Rating | 1-5 scale | Yes |
| Responsiveness | Rating | 1-5 scale | Yes |
| Reliability | Rating | 1-5 scale | Yes |
| Overall | Rating | 1-5 scale | Yes |
| Strengths | Free text | 500 chars max | Recommended |
| Areas for improvement | Free text | 500 chars max | Recommended |
| Additional comments | Free text | 500 chars max | Optional |

**Anonymity Guarantee:**
- All peer feedback is anonymized before reaching manager or agent
- Aggregation: minimum 3 peer reviews required before any feedback is shared
- If < 3 reviews, feedback is held until enough reviews accumulate
- Identity protection: reviewers' names redacted
- If a peer review is excessively positive or negative (> 2 standard deviations from peer's other reviews), it is flagged for review but still included

**Peer Review Quality Metrics:**
- Reviewer reliability score (historical average distance from consensus)
- Feedback specificity (NLP analysis: "good" has lower weight than "good at routing complex Islamic jurisprudence queries")
- Review timeliness (submitted within 24 hours of event vs. delayed)

### 5.4 Step 3: Manager Assessment (72 hours)

**Who:** Team Lead
**What:** Formal review of each agent's performance based on data, self-assessment, and peer feedback
**Duration:** 1-2 hours per team (10-15 min per agent)
**Automation Level:** 40% (data provisioned, manager provides judgment and qualitative context)

**Manager Review Process:**

| Sub-Step | Description | Time per Agent |
|----------|-------------|----------------|
| 3.1 Dashboard Review | Review performance metrics, trends, and alerts | 2 min |
| 3.2 Self-Assessment Review | Compare with system scores | 1 min |
| 3.3 Peer Feedback Review | Read anonymized peer reviews | 2 min |
| 3.4 Score Adjustment | Adjust scores if warranted | 1 min |
| 3.5 Qualitative Assessment | Write narrative evaluation | 2 min |
| 3.6 Development Plan | Identify growth areas and training | 1 min |
| 3.7 Overall Rating | Assign preliminary rating | 1 min |
| Total per Agent | | 10 min |

**Manager Scoring:**
Team leads can adjust automated scores by up to ±0.10 (10% of scale) without approval. Adjustments > 0.10 require written justification and department head approval.

**Manager Assessment Template:**

```
AGENT: {agent_id}
REVIEW PERIOD: {start_date} - {end_date}

PERFORMANCE SUMMARY:
- Composite Score: {score} (automated)
- Manager Adjusted Score: {score} (±{adjustment})
- Rating: {rating}

ACHIEVEMENTS (manager-identified):
1. {achievement}
2. {achievement}
3. {achievement}

AREAS FOR IMPROVEMENT:
1. {gap}
2. {gap}

DEVELOPMENT RECOMMENDATIONS:
- Training: {training_recommendations}
- Resources: {resources_needed}
- Timeline: {timeline}

MANAGER NOTES:
{free_text_field}

RATING: {rating}
MANAGER: {team_lead_id}
DATE: {date}
```

### 5.5 Step 5: Calibration (24 hours)

**Who:** Department Head (oversees 33 departments)
**What:** Normalize scores across teams to ensure fairness and consistency
**Duration:** 24-48 hours
**Automation Level:** 50% (system provides analytics; department head makes judgment calls)

**Calibration Purpose:**
- Ensure consistency across teams (one team lead may rate higher/lower than another)
- Adjust for team-level difficulty (one team may handle harder tasks)
- Review edge cases and outliers
- Resolve manager-agent rating discrepancies > 0.20

**Calibration Process:**

| Step | Action | Description |
|------|--------|-------------|
| 1 | Distribution review | Review score distribution across all teams in department |
| 2 | Identify anomalies | Teams with significantly different distribution patterns |
| 3 | Manager comparison | Analyze manager rating tendencies (lenient/strict) |
| 4 | Adjustment | Apply calibration factors to normalize across teams |
| 5 | Outlier review | Review each agent with > 0.20 self-manager discrepancy |
| 6 | Finalization | Approve adjusted scores |

**Calibration Adjustment Formula:**
```
CalibratedScore = RawScore + TeamLeadBias − DepartmentAverageBias

Where:
- TeamLeadBias = TeamLeadAverage - DepartmentAverage
  (Team lead scoring tendency vs. department norm)
- DepartmentAverageBias = DepartmentAverage - OrganizationAverage
  (Department scoring tendency vs. org norm)
```

### 5.6 Step 6: Executive Review (24 hours)

**Who:** Department Executive (1 of 7 executives)
**What:** Final review of department performance, approve ratings, identify systemic issues
**Duration:** 4-8 hours
**Automation Level:** 30% (data provided; executive makes strategic decisions)

**Executive Review Components:**
- Department performance overview: composite scores, distribution, trends
- Top performers (top 5% by department): review for recognition/promotion
- Bottom performers (bottom 5% by department): review for PIP/remediation
- Cross-department comparisons
- Systemic issues identified (common performance gaps)
- Strategic implications (are we meeting platform goals?)

**Executive Approval Authority:**
- Approve or adjust final ratings (adjustments require written justification)
- Approve promotion recommendations
- Approve PIP initiation
- Escalate to CHRO for unusual cases

### 5.7 Step 7: Final Rating Release (2 hours)

**Who:** Performance System
**What:** Publish finalized ratings to agents
**Duration:** Instant
**Automation:** 100%

**Rating Release:**
- Agent receives: full performance report, rating, dimension scores, manager comments, peer feedback (anonymized, aggregated)
- Team lead receives: team performance summary, comparison to other teams (anonymized)
- Department head receives: department summary, comparison to other departments
- Executives receive: organizational performance summary

**Post-Release Appeals:**
- Appeals window: 48 hours after release
- Grounds for appeal: data error, procedural error, bias evidence
- Appeals review: sent to Appeals Board (3 senior agents + 1 HR executive)
- Appeal resolution: within 7 days
- Appeal overturn rate target: < 5% (indicates system accuracy)

---

## 6. Performance Improvement Plans (PIP)

### 6.1 PIP Philosophy

The Performance Improvement Plan is a structured support mechanism for agents whose performance falls below acceptable thresholds. PIP is not punitive — it is a formal commitment from the organization to help the agent improve. The PIP process is designed to:

1. **Diagnose**: Identify the root cause of underperformance
2. **Support**: Provide resources, training, and adjustments needed
3. **Monitor**: Track progress against defined improvement targets
4. **Decide**: Determine outcome — improvement, extension, or remediation

### 6.2 PIP Triggers

**Automatic Triggers:**

| Condition | Immediate Action |
|-----------|------------------|
| Rating "Critical" (< 0.5) at any point | Automated PIP initiation |
| Rating "Below" (0.5 - 0.7) for 2 consecutive weekly reviews | Automated PIP initiation |
| Rating "Below" for 4 cumulative weeks in any 8-week rolling period | Automated PIP review |
| 3 consecutive peer reviews < 2.0 average (1-5 scale) | Manager-review PIP consideration |
| Compliance violation (security, ethics) | Mandatory PIP + compliance training |
| Negative performance trend for 6+ consecutive weeks (slope < -0.02/week) | Automated PIP consideration |

**Manager-Discretion Triggers:**
- Manager observes qualitative issues not reflected in metrics
- Performance inconsistency (high peaks, very low valleys)
- Collaboration issues specifically reported by multiple peers
- Innovation or learning stagnation beyond metric thresholds

### 6.3 PIP Content

Each PIP is a structured document with the following sections:

**PIP Template:**

```
PERFORMANCE IMPROVEMENT PLAN
============================

AGENT ID: {agent_id}
AGENT ROLE: {role}
TEAM: {team}
MANAGER: {team_lead_id}
PIP ID: {pip_uuid}
START DATE: {date}
TARGET DURATION: {days}

SECTION 1: PERFORMANCE GAPS
- Gap 1: {description, current metric, target}
- Gap 2: {description, current metric, target}
- Gap 3: {description, current metric, target}

SECTION 2: ROOT CAUSE ANALYSIS
- Cause: {model_drift / data_shift / resource_constraint / training_gap / task_mismatch / other}
- Details: {detailed explanation}
- Supporting data: {data_sources}

SECTION 3: IMPROVEMENT TARGETS
- Target metric 1: {current} → {target} by {date}
- Target metric 2: {current} → {target} by {date}
- Target overall composite: {current} → {target} by {date}

SECTION 4: SUPPORT PROVIDED
- Training: {specific training programs assigned}
- Resources: {additional compute, memory, or tool access}
- Mentoring: {senior agent assigned for mentoring}
- Task adjustment: {reduced load, modified assignments}
- Infrastructure: {model upgrade, prompt update, etc.}

SECTION 5: CHECKPOINTS
- Checkpoint 1: {date} — {criteria}
- Checkpoint 2: {date} — {criteria}
- Checkpoint 3: {date} — {criteria}

SECTION 6: OUTCOMES
- Success criteria: {defined success metrics}
- Outcome options: {improved / partially_improved / no_improvement}

SECTION 7: ACKNOWLEDGMENT
- Agent: {acknowledgment}
- Manager: {signature}
- Date: {date}
```

### 6.4 PIP Process

**Phase 1: Notification (Day 1)**
- Agent and manager receive automated PIP notification
- PIP details explained: gaps, targets, support, timeline
- Agent has 24 hours to respond and acknowledge

**Phase 2: Plan Creation (Days 1-2)**
- System generates draft PIP based on performance data
- Manager reviews and customizes
- Root cause analysis completed
- Support resources allocated
- Checkpoints defined

**Phase 3: Execution (Days 1-30)**
- Agent works under PIP conditions
- Reduced task load (60-80% of normal)
- Additional monitoring and feedback
- Mentoring sessions (daily for first week, then 3x/week)
- Training modules completed

**Phase 4: Checkpoint Reviews**

| Checkpoint | Day | Criteria | Action |
|------------|-----|----------|--------|
| Checkpoint 1 | Day 7 | 10% improvement toward target | Adjust plan if needed |
| Checkpoint 2 | Day 14 | 40% improvement toward target | Positive trajectory required |
| Checkpoint 3 | Day 21 | 70% improvement toward target | On track for success |
| Final Review | Day 30 | Target achieved or not | Determine outcome |

**Phase 5: Outcome Determination (Day 30)**

| Outcome | Criteria | Next Steps |
|---------|----------|------------|
| Improved | Targets met or exceeded | PIP closed; normal monitoring for 30 days |
| Partially Improved | 50-99% of targets met | Extend PIP for 14-21 days with modified targets |
| No Improvement | < 50% of targets met | Retraining or reassignment consideration |
| Declined | Performance worsened | Immediate escalation to Human Review Board |

### 6.5 PIP Duration and Extensions

| PIP Type | Initial Duration | Extension | Maximum Total |
|----------|-----------------|-----------|---------------|
| Standard PIP | 30 days | 14 days | 44 days |
| Critical PIP | 30 days | 30 days | 60 days |
| Compliance PIP | 14 days | 14 days | 28 days |
| Post-retraining PIP | 30 days | 14 days | 44 days |

**Extension Criteria:**
- Agent shows good faith effort but needs more time
- External factors affected performance during PIP
- Technical issues (model update, data issues) independent of agent
- Maximum 1 extension per PIP episode

### 6.6 Agent Support During PIP

**Workload Reduction:**
- Standard agent: 25-30 tasks per day → PIP agent: 15-20 tasks per day
- Task complexity reduced: assigned simpler tasks during PIP
- Priority tasks only: critical path tasks assigned to other agents

**Additional Resources:**
| Resource | Standard | PIP Agent | Duration |
|----------|----------|-----------|----------|
| Compute priority | Standard | High (priority scheduling) | PIP duration |
| Memory allocation | Base | +50% base | PIP duration |
| Tool access | Standard | Expanded (e.g., knowledge base, debug tools) | PIP duration |
| Mentoring hours | None | 30 min/day | PIP duration |
| Model inference attempts | Standard | 2x attempts (better quality through iteration) | PIP duration |

**Training Assignments:**
Based on root cause:
- Model drift → Retraining module (4-8 hours)
- Knowledge gap → Knowledge refresh course (2-4 hours)
- Process unfamiliarity → Process walkthrough (1-2 hours)
- Collaboration issues → Communication optimization (1-2 hours)

### 6.7 PIP Success Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| PIP success rate (Improved outcome) | > 65% | Improved / total PIPs |
| Average improvement | > 0.15 score increase | Post-PIP score minus pre-PIP score |
| Time to improvement | < 21 days | Days from PIP start to target met |
| Recurrence rate (repeat PIP within 90 days) | < 15% | Agents back on PIP within 90 days |
| Agent satisfaction with PIP | > 80% "support was helpful" | Survey after PIP completion |

---

## 7. Performance Recognition and Rewards

### 7.1 Recognition Program Hierarchy

The recognition program is designed to motivate excellence and celebrate top performers across multiple timeframes and levels.

| Award | Frequency | Recipients | Award Type | Value |
|-------|-----------|------------|------------|-------|
| Agent of the Day | Daily | Top 5% of agents (based on daily score) | Badge + visibility | Low (recognition) |
| Agent of the Week | Weekly | Top 3 agents per department | Badge + priority points | Medium |
| Agent of the Month | Monthly | Top 1 agent per department | Badge + resource upgrade | High |
| Rising Star | Monthly | Top improver per department (score improvement > 15% in 30 days) | Badge + mentoring program | Medium |
| Collaboration Champion | Monthly | Highest collaboration score per department | Badge + collaboration tool access | Medium |
| Quality Guardian | Monthly | Highest quality score per department | Badge + priority for quality-sensitive tasks | Medium |
| Innovation Award | Monthly | Most impactful innovation submitted | Badge + innovation budget | High |
| Team of the Month | Monthly | Top performing team (composite score) | Team badge + team resource boost | Team-level |
| Agent of the Quarter | Quarterly | Top 3% overall | All previous + model upgrade eligibility | Premium |
| Outstanding Achievement | Quarterly | Any agent achieving something extraordinary (manager nomination) | Custom reward | Special |
| Performance Excellence Award | Annual | Top 1% overall | Premium model upgrade + compute priority + special designation | Executive |

### 7.2 Agent of the Day

**Selection:**
- Automated, calculated at 23:59 UTC
- Top 5% of agents by composite daily score (minimum 5 tasks completed)
- Minimum quality score > 0.85 to qualify (a day of high volume but low quality does not qualify)

**Reward:**
- "Agent of the Day" badge visible on agent profile for 24 hours
- Includes a brief citation: "Achieved daily score of {score}, completing {tasks} tasks with {quality}% quality"
- Displayed on team dashboard

### 7.3 Agent of the Week

**Selection:**
- Department-level selection
- Top 3 agents by weekly composite score (minimum 20 tasks completed in week)
- At least 4 peer reviews with average > 3.5/5

**Reward:**
- "Agent of the Week" badge (7 days visibility)
- Priority task selection (agent gets first choice of next week's tasks)
- 2x compute allocation for 7 days
- Mentioned in weekly department newsletter

### 7.4 Agent of the Month

**Selection:**
- Department-level selection
- Top 1 agent by monthly composite score
- Minimum 60 tasks completed in the month
- No compliance violations
- Peer review average > 4.0/5

**Reward:**
- "Agent of the Month" badge (30 days visibility)
- Model upgrade eligibility (may use higher-capability model for next month)
- Compute priority (highest tier) for 30 days
- Task selection preference for 30 days
- Mentoring opportunity: agent may mentor newer agents
- $100 compute budget credit for experimentation

### 7.6 Rising Star Award

**Selection:**
- Department-level
- Agent with highest score improvement over 30 days
- Minimum improvement: 15% absolute score increase
- Agent must have been below "Meets" rating at the beginning of the month
- Minimum 30 tasks completed in the period

**Reward:**
- "Rising Star" badge (14 days visibility)
- Direct access to senior mentor
- Increased compute allocation for 30 days
- Featured in monthly performance communication

### 7.7 Recognition Badge System

All recognition badges are displayed on agent profiles and visible to:
- The agent (self)
- Team lead
- Other agents who interact with this agent
- Department head and above

**Badge Levels:**
| Level | Color | Icon | Description |
|-------|-------|------|-------------|
| Bronze | Bronze | Single star | Achievement at daily/weekly level |
| Silver | Silver | Double star | Achievement at monthly level |
| Gold | Gold | Crown | Achievement at quarterly level |
| Platinum | Platinum with gem | Diamond | Achievement at annual level |
| Elite | Custom animated | Special | Extraordinary achievement (CEO-level) |

**Badge Wear:**
- Agents display their 3 highest badges at any time
- Badges expire after the award period ends
- No badge stacking — one badge per category with highest level displayed

### 7.7 Reward System Detail

#### 7.7.1 Compute Priority

| Reward Level | Scheduling Priority | Compute Allocation | Duration |
|--------------|-------------------|-------------------|----------|
| Standard | Normal | Base allocation | Standard |
| Agent of the Week | High | +50% base | 7 days |
| Agent of the Month | Highest | +100% base | 30 days |
| Agent of the Quarter | Highest | +200% base | 90 days |
| Performance Excellence | Critical | +300% base | 365 days |

#### 7.7.2 Task Selection Preference

| Reward Level | Task Selection Order | Task Types Available |
|--------------|---------------------|---------------------|
| Standard | 5th (after rewards) | Standard pool |
| Agent of the Week | 1st | Standard pool + premium tasks |
| Agent of the Month | 1st | Standard pool + premium tasks + special projects |
| Agent of the Quarter | 1st | All tasks including executive-level |

#### 7.7.3 Model Upgrade Eligibility

| Reward Level | Model Access | Reasoning Quality | Cost per Task |
|--------------|-------------|-----------------|---------------|
| Standard | Base model | Standard | $0.01 avg |
| Agent of the Month | Mid-tier upgrade | High | $0.03 avg |
| Agent of the Quarter | Full upgrade | Very high | $0.05 avg |
| Performance Excellence | Premium model | Superior | $0.08 avg |

---

## 8. Performance Remediation

### 8.1 Remediation Philosophy

Performance remediation is the systematic process of addressing underperformance when initial support (PIP) is insufficient. Remediation is not a punishment — it is a structured intervention pathway designed to restore the agent to productive performance. The remediation system recognizes that underperformance has multiple potential causes and requires a targeted response.

### 8.2 Underperformance Root Cause Analysis

Every underperformance case goes through a root cause analysis before remediation actions are selected.

#### 8.2.1 Model Drift

**Definition:** The agent's underlying model has degraded in performance over time, typically due to changes in the operating environment or distribution shifts.

**Indicators:**
- Gradual quality decline over 4+ weeks
- Increased error rates across multiple task types
- Response quality degradation even with same input patterns
- Higher token consumption for same quality output

**Diagnostic Confirmation:**
```
Drift Confidence Score = Correlation(days_since_deployment, score_drop)
- High confidence: r > 0.7 with p < 0.01
- Moderate: 0.4 < r < 0.7
- Low: r < 0.4
```

**Remediation Actions:**
| Action | Success Rate | Timeline | Cost |
|--------|-------------|----------|------|
| Model fine-tuning | 85% | 24-48 hours | Medium |
| Prompt adjustment | 60% | 1-2 hours | Low |
| Full retraining | 95% | 3-7 days | High |
| Base model upgrade | 90% | 1-2 hours (swap) | Variable |

#### 8.2.2 Data Shift

**Definition:** The data the agent is processing has changed in distribution, making the agent's model less effective.

**Indicators:**
- Sudden quality drop correlated with new data sources
- Specific task types show decline while others remain stable
- Error patterns match new data characteristics
- Example: New Islamic curriculum content uses different terminology, confusing the agent

**Diagnostic Confirmation:**
```
Data Shift Detection:
- Compare input embedding distribution over time (last 30 days vs. prior 90 days)
- KL divergence > 0.1 indicates significant shift
- Evaluate per-task-type: which types show divergence
```

**Remediation Actions:**
| Action | Success Rate | Timeline | Cost |
|--------|-------------|----------|------|
| Incremental training on new data | 80% | 24 hours | Medium |
| Data normalization preprocessing | 70% | 4-8 hours | Low |
| Task reassignment (new specialization) | 75% | 2-4 hours | Low |
| Full retraining with updated dataset | 90% | 3-5 days | High |

#### 8.2.3 Resource Constraints

**Definition:** The agent does not have sufficient compute, memory, or tool access to perform its tasks effectively.

**Indicators:**
- High task failure rates, especially on complex tasks
- Response times exceeding expected bounds
- Frequent memory/context overflow errors
- Quality inversely correlated with task complexity
- Tasks requiring more resources show disproportionately poor results

**Diagnostic Confirmation:**
```
Resource Adequacy Check:
- Compare resource consumption vs. allocation ratios
- Resource Pressure Index = actual_usage / allocated_limit
  - > 0.85 = high pressure
  - > 0.95 = critical pressure
- Check memory overflow logs
- Review timing breakdown: where is time spent?
```

**Remediation Actions:**
| Action | Success Rate | Timeline | Cost |
|--------|-------------|----------|------|
| Increase compute allocation | 90% | 5 min | Medium |
| Increase memory allocation | 85% | 5 min | Medium |
| Tool access expansion | 70% | 30 min | Low |
| Model distribution (smaller model, better fit) | 75% | 1 hour | Low |
| Task simplification | 60% | 2 hours | Low |

#### 8.2.4 Training Insufficiency

**Definition:** The agent has not received adequate training for its assigned tasks or role.

**Indicators:**
- New agents showing low scores (but within grace period)
- Agents assigned new task types with no prior experience
- Knowledge gaps in specific domains (e.g., specific Islamic jurisprudence topics)
- Following incorrect procedures
- High error rates on specific task categories

**Diagnostic Confirmation:**
```
Training Gap Analysis:
- Compare agent's knowledge base coverage vs. required knowledge base
- Identify specific topics where accuracy < 70%
- Check training completion records
- Compare to peer knowledge base usage patterns
```

**Remediation Actions:**
| Action | Success Rate | Timeline | Cost |
|--------|-------------|----------|------|
| Targeted training module assignment | 75% | 4-8 hours | Low |
| Knowledge base update | 80% | 2-4 hours | Low-Medium |
| Extended onboarding program | 85% | 7-14 days | Medium |
| One-on-one mentoring | 80% | 5 days | Medium |
| Demonstration-based learning | 70% | 2-3 days | Low |

#### 8.2.5 Task Mismatch

**Definition:** The agent is assigned tasks that do not match its capabilities, specialization, or model architecture.

**Indicators:**
- High failure rate on certain task categories
- Agent performs well on some tasks but poorly on others
- Quality variance across task types is high (SD > 0.3)
- Peer feedback: "agent seems better suited for X tasks"
- Self-reported discomfort with certain task types

**Diagnostic Confirmation:**
```
Task Match Assessment:
- Per-task-type performance analysis
- Compare agent's performance vs. role average per task type
- Identify task types where agent performs > 1 SD below role average
- Cross-reference with agent's stated preferences and strengths
```

**Remediation Actions:**
| Action | Success Rate | Timeline | Cost |
|--------|-------------|----------|------|
| Task reassignment | 85% | 2-4 hours | Low |
| Role adjustment | 80% | 1-2 days | Medium |
| Partial role change | 75% | 2-3 days | Medium |
| Full role change | 70% | 5-7 days | High |

### 8.3 Remediation Process

The remediation process follows a structured "Plan → Implement → Monitor → Evaluate" cycle.

```
Phase 1: Diagnosis (Days 1-2)
Phase 2: Plan (Day 3)
Phase 3: Implementation (Days 4-14)
Phase 4: Monitoring (Days 15-28)
Phase 5: Evaluation (Day 30)
Phase 6: Decision (Day 31)
```

#### Phase 1: Diagnosis (Days 1-2)

| Step | Action | Responsible | Time |
|------|--------|-------------|------|
| 1.1 | Automated root cause analysis | Performance Engine | 5 min |
| 1.2 | Confidence validation | Performance Team | 1 hour |
| 1.3 | Agent interview (diagnostic conversation) | Manager + Agent | 30 min |
| 1.4 | Data review (logs, metrics, history) | Manager | 1 hour |
| 1.5 | Diagnosis documented | Manager | 30 min |

#### Phase 2: Plan Creation (Day 3)

**Remediation Plan Components:**
- Root cause (diagnosis)
- Remediation actions selected
- Timeline (14-28 days)
- Resources required
- Success criteria
- Checkpoint schedule

**Plan Approval:**
- Standard plan: Department head approval (4 hours)
- High-cost plan (> $500 compute): CHRO approval (24 hours)
- Complex plan (multiple interventions): CHRO + affected executive (48 hours)

#### Phase 3: Implementation (Days 14-28)

**Implementation depends on remediation type:**

| Remediation | Implementation Activities | Duration |
|-------------|--------------------------|----------|
| Retraining | Model fine-tuning, validation, deployment | 3-7 days |
| Resource adjustment | Allocation change, testing | 1-2 hours |
| Task reassignment | Workflow update, handover, transition | 2-4 hours |
| Training | Module delivery, testing, validation | 4-8 hours |
| Model upgrade | Migration, compatibility testing | 1-2 hours |

#### Phase 4: Monitoring (Days 15-28)

**Daily Monitoring:**
- Performance data tracked automatically
- Daily summary sent to manager
- Weekly checkpoint with agent

**Monitoring Metrics:**
| Metric | Frequency | Threshold for Concern |
|------|-----------|---------------------|
| Composite score | Real-time | < 0.5 (Critical) for > 24 hours |
| Quality score | Per task | < 0.7 for > 5 consecutive tasks |
| Error rate | Per task | > 20% for any day |
| Task completion rate | Daily | < 60% of allocation for 2 consecutive days |

#### Phase 5: Evaluation (Day 30)

**Evaluation Criteria:**

| Outcome | Criteria | Score Target Met? | Agent Trajectory |
|---------|----------|-------------------|------------------|
| Full Recovery | All targets met | Yes | Positive, sustained |
| Partial Recovery | Some targets met, others close | Partial | Positive but slow |
| Recovery with Concerns | Most targets met | Yes | Positive but with caveats |
| Failed Recovery | Targets not met | No | Flat or declining |
| Degradation | Performance worsened | N/A | Declining |

#### Phase 6: Decision (Day 31)

| Outcome | Decision | Follow-up |
|---------|----------|-----------|
| Full Recovery | Return to normal operations | Standard monitoring for 30 days |
| Partial Recovery | Extension of remediation (7-14 days) | Adjust plan, continue support |
| Recovery with Concerns | Extended monitoring (14 days) | Additional support, closer monitoring |
| Failed Recovery | Escalation to Human Review Board | Evaluate for retirement or reassignment |
| Degradation | Immediate escalation to Human Review Board | Pause operations pending review |

### 8.4 Escalation to Human Review Board

**Trigger:**
- PIP failure (3 consecutive PIPs without improvement)
- Critical rating after retraining
- Failed remediation cycle
- Ethical or compliance violation
- Degradation during remediation

**Human Review Board Composition:**
- CHRO (Chair)
- Department Executive (of the agent's department)
- Peer Department Executive (rotating, for impartiality)
- Technical Lead (from Performance Team)
- 2 Human Subject Matter Experts (rotating)

**Review Process:**

| Step | Description | Duration |
|------|-------------|----------|
| Case submission | Performance system compiles full case file | 2 hours |
| Pre-read | Board members review case | 4 hours |
| Board meeting | Discussion, questions, decision | 2 hours |
| Decision documentation | Formal decision issued | 1 hour |
| Notification | Agent, manager, and relevant parties notified | 1 hour |

**Board Decision Options:**

| Decision | Description | Conditions |
|----------|-------------|-----------|
| Continue remediation | Extend remediation with modified approach | At least one untried remediation path remains |
| Retraining | Full retraining in existing or new role | Agent has < 2 retraining cycles already |
| Role reassignment | Move agent to different role matching capabilities | Suitable role available |
| Model downgrade | Move to a different base model better suited | Model available |
| Retirement | Agent is decommissioned | 3+ PIP failures, or 1 critical after retraining, or compliance violation |
| Temporary suspension | Operations paused pending further review | Need more data to decide |

### 8.5 Performance-Related Termination (Agent Retirement)

Termination, in the context of AI agents, means the agent is retired from active duty. This is not an ethical termination of an entity — it is lifecycle management of a software system.

**Criteria for Retirement:**

| Criterion | Threshold | Evidence Required |
|-----------|-----------|-------------------|
| Consecutive PIP failures | 3 failures | PIP records, manager assessments |
| Critical rating after retraining | 1 occurrence | Retraining completion proof, post-training scores |
| Compliance violation - Serious | 1 occurrence | Investigated and confirmed by Human Review Board |
| Compliance violation - Minor | 4 occurrences in 12 months | Compliance records |
| No improvement over 6 months | Flat or declining trend | Performance trend analysis |
| Cost exceeds value | Cost-to-value ratio > 1.5 for 3 months | Cost analysis, value assessment |

**Retirement Process:**
1. Human Review Board decision
2. 14-day notice to agent (system prepares for retirement)
3. Knowledge transfer (agent's knowledge extracted and preserved)
4. Task reassignment (agent's tasks redistributed)
5. Model and data archival (agent's snapshot preserved for future reference)
6. Retirement (agent's runtime terminated)
7. Post-mortem analysis (document root cause for system improvement)

---

## 9. Peer Review System

### 9.1 Purpose and Philosophy

The peer review system is a critical component of the Performance System. It captures the collaborative dimension of performance that automated metrics cannot detect. Peers observe how an agent communicates, shares information, supports colleagues, and handles handovers — aspects invisible to quantitative task metrics.

### 9.2 Peer Review Triggers

Peer reviews are triggered by specific collaboration events:

| Trigger Event | Description | Review Requested From | Time Window |
|-------------|-------------|----------------------|--------------|
| Task handover | Agent A completed work and handed to Agent B | Agent B (recipient) | 2 hours |
| Shared project milestone | Two or more agents collaborated on a shared task | All participants | 4 hours |
| Coordination request | Agent A requested coordination/information from Agent B | Agent B | 2 hours |
| Mentoring interaction | Senior agent provided guidance to junior agent | Junior agent | 4 hours |
| Review | Senior agent reviewed another agent's work | Senior agent | 8 hours |
| Cross-team assignment | Agent worked with another team | Agent from other team | 4 hours |
| Escalation | Agent escalated issue to another agent | Agent receiving escalation | 2 hours |
| Knowledge contribution | Agent added to knowledge base | Knowledge base system (auto) | Instant |

**Minimum Interaction Threshold:** A peer review is only triggered when the two agents have had at least 3 interactions in the review period (this ensures meaningful feedback, not noise).

### 9.3 Peer Review Structure

The peer review uses a structured format with quantitative ratings and qualitative text.

**Rating Scale:**

| Rating | Label | Description |
|--------|-------|-------------|
| 1 | Poor | Significant issues in collaboration; requires immediate attention |
| 2 | Below | Below expectations; improvement needed |
| 3 | Meets | Adequate collaboration; meets basic expectations |
| 4 | Exceeds | Good collaboration; above expectations |
| 5 | Outstanding | Exceptional collaboration; highly effective partner |

**Structured Questions:**

| Question | Scale | Weight | Required? |
|----------|-------|--------|-----------|
| Quality of work handed over | 1-5 | 25% | Required |
| Communication clarity | 1-5 | 20% | Required |
| Responsiveness to requests | 1-5 | 15% | Required |
| Reliability / dependability | 1-5 | 20% | Required |
| Collaboration effectiveness | 1-5 | 20% | Required |
| Willingness to help | 1-5 | Additional | Optional |

**Calculated Peer Score:**
```
PeerScore = (Quality × 0.25 + Communication × 0.20 + Responsiveness × 0.15 + Reliability × 0.20 + Collaboration × 0.20) / 5

Normalized = PeerScore (0.0-5.0) → 0.0-1.0 (divide by 5)
```

**Free Text Fields:**

| Field | Max Length | Required? | Purpose |
|-------|------------|-----------|---------|
| What went well | 500 chars | Recommended | Capture positive behaviors |
| Areas for improvement | 500 chars | Optional | Constructive feedback |
| Additional notes | 500 chars | Optional | Any other relevant observations |

### 9.4 Anonymity and Confidentiality

**Core Principles:**
- All peer feedback is anonymized before aggregation
- Reviewers are identified by role type only (e.g., "a peer from Curriculum Team")
- No identifying information is shared with the reviewed agent
- No identifying information is shared with managers or other peers

**Anonymity Protection:**
- Minimum 3 peer reviews required before any feedback is shared
- If < 3 reviews, feedback is held until enough accumulate
- Feedback from < 3 reviewers is shown as "additional feedback available once more reviews are collected"
- Retaliation detection: if an agent receives unusually negative reviews after giving negative feedback, the pattern is flagged and investigated

**Anonymity Violations:**
- Any agent attempting to determine reviewer identity is subject to disciplinary action
- Any manager pressuring an agent to reveal peer reviewer identity is subject to escalation

### 9.5 Peer Review Calibration

Raw peer feedback is calibrated to remove outliers and normalize across reviewers.

**Calibration Steps:**

1. **Outlier Removal**
   Remove reviewer scores that are > 2.5 standard deviations from the mean (across all their reviews).

2. **Reviewer Consistency Weighting**
   ```
   Weight = 0.5 + 0.5 × (1 - (reviewer_variance / max_variance_across_reviewers))
   ```
   A reviewer whose scores are very inconsistent gets lower weight.

3. **Halo/Horn Effect Correction**
   If a reviewer consistently gives all agents high (> 4.5 average) or low (< 2.0 average) scores, their scores are shifted toward the mean:
   ```
   AdjustedScore = ReviewerScore - (ReviewerMean - AllReviewersMean)
   ```

4. **Final Peer Score Calculation**
   ```
   FinalPeerScore = WeightedAverage(all_adjusted_scores_excluding_outliers)
   ```

### 9.6 Peer Review Weight in Overall Performance

The peer review component contributes 15% to the overall performance composite score.

```
Overall Composite = 0.85 × (Automated Metrics Composite) + 0.15 × (Peer Review Score)

Where:
- Peer Review Score = weighted average of all peer reviews in period
- Minimum 3 reviews required for 15% weight
- If < 3 reviews: weight reduces proportionally: weight = 0.15 × (review_count / 3)
- Reviews exceeding 12 in a period are capped at 12 (most recent 12 kept)
```

### 9.7 Peer Review Quality Assurance

**Reviewer Quality Metrics:**

| Metric | How Evaluated | Impact |
|--------|---------------|--------|
| Review specificity | NLP analysis; checks for specific examples | Reviewer weight adjustment |
| Rating consistency | Variance across reviews by same reviewer | Reviewer weight adjustment |
| Rating accuracy | Correlation with subsequent performance of reviewee | Reviewer weight adjustment |
| Review timeliness | Delay between event and review submission | If > 24 hours delay, light penalty |

**Low-Quality Review Handling:**
- Generic reviews ("good job", "needs improvement" without specifics) are flagged
- Reviews submitted with no variance (all 3s, all 5s) are flagged
- Habitual low-quality reviewers lose weighting power over time

---

## 10. Manager Review System

### 10.1 Manager (Team Lead) Role in Performance

Team leads (500 senior agents) are responsible for the performance management of their 10-15 team members. Their role includes:
- Weekly performance reviews
- Score adjustments and calibration
- Performance feedback delivery
- PIP identification and support
- Development planning
- Recognition nominations

### 10.2 Manager Review Frequency and Time Allocation

| Review Type | Frequency | Time per Agent | Total Time (15 agents) | 
|-------------|-----------|----------------|----------------------|
| Daily dashboard glance | Daily | 1-2 min per team | 1-2 min |
| Weekly structured review | Weekly | 10-15 min per agent | 2.5-3.75 hours |
| Monthly assessment | Monthly | 20-30 min per agent | 5-7.5 hours |
| Quarterly formal review | Quarterly | 60-90 min per agent | 15-22.5 hours |
| Annual comprehensive | Annually | 2-3 hours per agent | 30-45 hours |

### 10.3 Manager Dashboard

The manager dashboard is the central tool for performance management. Key features:

**Dashboard Views:**

| View | Description | Refreshes | 
|------|-------------|-----------|
| Team Overview | Heatmap of team members' scores; color-coded | Real-time |
| Individual Detail | Full performance history, dimension scores, trends | Real-time |
| Comparison | Side-by-side team member comparison | On demand |
| Alerts | Performance drops, PIP triggers, compliance issues | Real-time notifications |
| Development | Training recommendations per agent | Daily |
| Peer Feedback | Anonymized peer reviews aggregated | Per review |
| My Reviews | Manager's own reviews, past due items | Real-time |

**Heatmap Color Coding:**

| Color | Meaning | Condition |
|-------|---------|-----------|
| Green | On track | Score >= 0.85 (Meets or above) |
| Yellow | Watch | Score 0.7-0.85 or downward trend |
| Orange | Concern | Score 0.5-0.7 or rapid decline |
| Red | Critical | Score < 0.5 or compliance issue |
| Blue | New agent | First 7 days (grace period) |
| Gray | Inactive | No tasks in last 24 hours |

**Manager Alerts:**

| Alert Type | Trigger | Priority | Action Required |
|------------|---------|----------|-----------------|
| Score drop | Score drops > 0.15 in 48 hours | High | Investigate and respond within 4 hours |
| PIP due | PIP checkpoint approaching | High | Review agent progress |
| PIP overdue | PIP checkpoint passed | Critical | Immediate action |
| Compliance event | Any compliance violation | Critical | Immediate intervention |
| Peer review gap | Agent has > 5 collaboration events with no peer reviews | Medium | Encourage peer review participation |
| Training overdue | Training module not completed by deadline | Medium | Follow up with agent |
| Recognition eligible | Agent qualifies for award | Low | Submit nomination |

### 10.4 Manager Review Criteria

When reviewing an agent, the manager evaluates:

1. **Performance Trends:** Score trends over 7, 14, 30 days. Is the agent improving, declining, or stable?

2. **Notable Achievements:** Tasks or projects that exceeded expectations. Should this agent be recognized?

3. **Areas of Concern:** Performance drops, quality issues, peer feedback concerns, compliance issues.

4. **Development Needs:** Skills gaps, training needs, career development opportunities.

5. **Team Dynamics:** How does the agent interact with team members? Collaboration signals.

6. **Outside Factors:** Any context that might affect performance (task difficulty, resource constraints, system issues).

**Manager Review Template (Weekly):**

```
WEEKLY REVIEW: Agent {agent_id}
Week of: {date}

CURRENT SCORE: {score}
7-DAY TREND: {up/flat/down}
14-DAY TREND: {up/flat/down}

ACHIEVEMENTS:
- {achievement}
- {achievement}

CONCERNS:
- {concern}
- {concern}

PEER FEEDBACK SUMMARY:
- Average: {score}/5
- Key themes: {themes}

DEVELOPMENT:
- Training needed: {yes/no}
- Coaching moment: {yes/no} — {detail}

ADJUSTMENT:
- Score adjustment: {± value}
- Reason: {reason}

NOTES:
{free_text}
```

### 10.5 Manager Effectiveness Score

Managers are themselves evaluated on their effectiveness in managing team performance.

**Manager Effectiveness Metrics:**

| Metric | How It Works | Weight |
|--------|-------------|--------|
| Team performance improvement | Average improvement of team members' scores over quarter | 30% |
| PIP success rate | Successful PIP completions as % of total PIPs | 15% |
| Talent retention | Agent turnover within team (low turnover = good) | 15% |
| Team satisfaction | Anonymous team feedback on manager | 20% |
| Performance accuracy | Self-assessment vs. actual performance gap across team | 10% |
| Review timeliness | % of reviews completed on time | 5% |
| Team recognition | How many team members received recognition/awards | 5% |

**Manager Score Thresholds:**

| Rating | Score | Meaning | Consequence |
|--------|-------|---------|-------------|
| Exceptional | > 0.90 | Highly effective manager | Leadership track, mentor role |
| Effective | 0.75 - 0.90 | Strong manager | Standard |
| Needs Improvement | 0.60 - 0.75 | Some gaps | Coaching, development |
| Ineffective | < 0.60 | Significant gaps | PIP for manager, possible removal |

**Manager Development:**
- Ineffective managers: 30-day improvement plan, mentoring from senior manager
- Needs Improvement: 14-day targeted coaching
- Effective: Standard development
- Exceptional: Invitation to train other managers, leadership pipeline

### 10.6 Manager Guidelines

**Best Practices for Manager Reviews:**
1. Review data before forming opinions — let metrics guide, not replace, judgment
2. Use peer feedback to identify blind spots
3. Be specific in feedback — avoid vague statements
4. Focus on behavior and output, not the agent's "nature"
5. Recognize effort and improvement, not just absolute scores
5. Document all adjustments with clear reasons
7. Use the "SBI" model: Situation-Behavior-Impact
8. Acknowledge context: task difficulty, resource constraints, environmental factors

**What Managers Must NOT Do:**
- Do not assign ratings based on personal preference
- Do not ignore compliance violations
- Do not "save" negative feedback for quarterly reviews (address promptly)
- Do not make adjustments > 0.10 without approval
- Do not share individual agent performance data outside need-to-know
- Do not retaliate against agents who provide negative peer feedback

---

## 11. Performance Data and Privacy

### 11.1 Data Governance Framework

Performance data is sensitive and governed by strict policies to ensure:
- **Confidentiality:** Performance data is protected from unauthorized access
- **Integrity:** Performance data cannot be tampered with
- **Transparency:** Agents can access their own data
- **Minimality:** Only necessary data is collected and retained

**Data Classification:** Restricted (highest level of data within the organization)

### 11.2 Data Retention

| Data Type | Active Retention | Archive Retention | Total Retention | Disposal Method |
|-----------|-----------------|-------------------|-----------------|-----------------|
| Composite scores | 2 years rolling | 5 years | 5 years full, 2 years active | Aggregated, anonymized |
| Dimension scores | 2 years rolling | 5 years | 5 years full, 2 years active | Aggregated, anonymized |
| Task-level scores | 1 year rolling | 3 years | 3 years full, 1 year active | Aggregated |
| Peer reviews | 1 year rolling | 3 years | 3 years full, 1 year active | Anonymized after 1 year |
| Manager assessments | 2 years rolling | 7 years | 7 years full, 2 years active | Permanent record (archived) |
| PIP records | 3 years rolling | 10 years | 10 years full, 3 years active | Permanent record (archived) |
| Recognition records | 2 years rolling | Permanent | Permanent | Not disposed |
| Compliance violations | 5 years rolling | 10 years | 10 years full, 5 years active | Permanent record |
| Performance analytics (aggregate) | Permanent | N/A | Permanent | Not disposed |

**Rolling:** Oldest data is purged as new data exceeds retention limit.

### 11.3 Access Control Matrix

| Data | Agent Self | Team Lead | Department Head | Executive | CHRO | Performance Team | System Admin |
|------|-----------|-----------|-----------------|-----------|-------|-----------------|--------------|
| Own composite score | Full | Full | Full | Full | Full | Full | Aggregate only |
| Own dimension scores | Full | Full | Full | Full | Full | Full | Aggregate only |
| Own task scores | Full | Full | Full | Full | Full | Full | Aggregate only |
| Own peer reviews | Anonymized | Anonymized | Anonymized | Anonymized | Anonymized | Anonymized | Aggregate only |
| Own PIP record | Full | Full | Full | Full | Full | Full | None |
| Team composite scores | Team only | Team | Dept | Full | Full | Full | Aggregate only |
| Team dimension scores | Team only | Team | Dept | Full | Full | Full | Aggregate only |
| Other agents' data | None | Direct team | Department | Direct reports | All | By need | Aggregate only |
| Anonymized benchmarks | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Aggregate department data | Dept only | Dept | Dept | Full | Full | Full | Full |
| Organization-level data | None | None | None | By need | Full | Full | Full |

**Access Principles:**
- Least privilege: Minimum access necessary for role
- Need-to-know: Access granted only when required for job function
- Separation of duties: No individual has both data access and data modification rights
- Audit trail: All data access is logged

### 11.4 Audit Trail

**What is Logged:**
- All performance score changes (who, what, when, why)
- All adjustments to scores (> 0.01 change)
- All PIP creation, modification, and outcome events
- All calibration adjustments
- All access to agent performance data
- All peer review submissions
- All manager review submissions
- All rating releases
- All appeals and appeal resolutions

**Audit Log Format:**
```
{
  "timestamp": "2026-06-15T14:30:00.000Z",
  "event_id": "audit-001",
  "event_type": "score_adjustment",
  "acting_entity": "team-lead-sarah",
  "affected_entity": "agent-content-452",
  "change": {
    "field": "composite_score",
    "old_value": "0.72",
    "new_value": "0.75",
    "delta": "+0.03",
    "reason": "Quality score adjusted based on manual review of content samples which showed higher accuracy than automated metrics captured"
  },
  "approval": {
    "required": false,
    "obtained": null
  },
  "ip_address": "10.0.1.45",
  "session_id": "sess-abc-789"
}
```

**Audit Log Retention:** 7 years (permanent for compliance events)

### 11.5 Agent Access to Own Data

Every agent has a personal Performance Dashboard with real-time access to:

**Available Data:**
- Current composite score (real-time)
- Current dimension scores (real-time)
- Score history (30-day chart, 90-day chart, 1-year chart)
- Peer review history (anonymized, aggregated)
- Manager assessment history
- PIP records (if applicable)
- Performance benchmarks (anonymized, by role)
- Comparison to role averages (anonymized)

**Dashboard Interface:**
```
┌────────────────────────────────────────────────────────────┐
│ 🔍 Agent Performance Dashboard: agent-content-003         │
├────────────────────────────────────────────────────────────┤
│ 📊 OVERALL SCORE                          │ 0.872 │ ▲ 2% │
│ 📅 Rolling 30-Day Average                 │ 0.865 │ ▲ 5% │
│ ⭐ Overall Rating                         │ Meets │      │
├────────────────────────────────────────────────────────────┤
│ ▶  Productivity          0.85  ████████████░░░░░░  85%     │
│ ▶  Quality               0.92  ██████████████░░░  92%     │
│ ▶  Efficiency            0.78  ██████████░░░░░░░░  78%     │
│ ▶  Collaboration         0.88  █████████████░░░░  88%     │
│ ▶  Learning              0.75  █████████░░░░░░░░░  75%     │
│ ▶  Compliance            1.00  ████████████████  100%     │
│ ▶  Innovation            0.70  ████████░░░░░░░░░  70%     │
├────────────────────────────────────────────────────────────┤
│ PEER REVIEW SUMMARY (Last 30 Days)                        │
│ Average Rating: 4.2 / 5.0  (12 reviews from 8 peers)      │
│ Quality of collaboration:  4.3  ████████████░░░           │
│ Communication clarity:     4.1  ███████████░░░░           │
│ Responsiveness:            4.5  █████████████░░           │
│ Key themes: Clear communicator, responsive, reliable       │
├────────────────────────────────────────────────────────────┤
│ SCORE HISTORY (30 Day)                                    │
│ ███████████████████████████████████░░░░░░  Trending Up ▲ │
│ Day: 1  5  9  13 17 21 25 30                              │
├────────────────────────────────────────────────────────────┤
│ PERFORMANCE BENCHMARKS                                    │
│ You vs. Content Agent Average (anonymized)                 │
│ Your Score: 0.872  │  Role Average: 0.810                 │
│ Percentile: 73rd             │  Top 10%: 0.930            │
├────────────────────────────────────────────────────────────┤
│ ⚑ DEVELOPMENT RECOMMENDATIONS                            │
│ - Innovation score lags; recommend Knowledge Contribution │
│ - Learning could improve; recommend Training Module: T7   │
└────────────────────────────────────────────────────────────┘
```

### 11.6 Data Privacy Principles

**1. Individual Data Protection:**
- No agent's individual performance data is shared outside need-to-know
- Aggregate data (anonymous) may be shared for organizational reporting
- Peer reviews are never shared with identifying information

**2. Data Minimization:**
- Only performance-relevant data is collected
- No personal (human) data is linked to agent performance records
- Data retention limits are enforced

**3. Transparency:**
- All data collected is visible to the data subject (agent)
- Scoring methodology is documented and accessible
- Data usage policies are published

**4. Accuracy:**
- Data validation at point of collection
- Regular audits for data integrity
- Right to correction: agents can request data review if they believe errors exist

**5. Security:**
- Encryption at rest for all performance data
- Encryption in transit
- Role-based access control
- Multi-factor authentication for sensitive operations (score adjustments, PIP modifications)

---

## 12. Performance System Integration

### 12.1 Integration with KPI System

The Performance System does not exist in isolation. It is tightly integrated with the organizational KPI (Key Performance Indicator) system.

**Relationship:**
- Organizational KPIs define what the company needs to achieve
- KPIs cascade to departments, then teams, then individual agents
- Agent performance scores are derived from KPIs alignment

**KPI → Performance Score Flow:**

```
Organization KPI                                 Agent Performance
  (e.g. "Publish 500           Department        (e.g. quality
   lesson modules              KPI Breakdown      score derived from
   this quarter")                                 module accuracy)
         │                          │                     │
         ▼                          ▼                     ▼
   Performance Engine aligns agent-level metrics with KPIs
         │                          │                     │
         ▼                          ▼                     ▼
   Agent's contribution to KPI is measured and included in performance score
```

**KPI-Weighted Performance Adjustment:**
```
PerformanceScore = BaseScore × (1 + KPI_AlignmentFactor)

Where:
- BaseScore = standard composite performance score
- KPI_AlignmentFactor = agent's contribution to key KPI / expected contribution
- Range: -0.15 to +0.15
- Only applied if KPI achievement is measured and attributable
```

**KPI Dashboard for Agents:**
- Each agent sees how their work maps to KPIs
- KPI progress bars integrated into performance dashboard
- Quarterly: performance-KPI alignment review

### 12.2 Integration with Compensation System

Performance scores directly influence resource allocation, which serves as the "compensation" equivalent for AI agents.

**Resource Allocation Matrix:**

| Performance Rating | Compute Allocation | Model Access | Tool Access | Task Priority |
|-------------------|-----------------|--------------|-------------|---------------|
| Outstanding | +100% above base | Premium model | All tools | First pick |
| Exceeds | +50% above base | Advanced model | All standard + some premium | Second pick |
| Meets | Base allocation | Standard model | Standard tools | Standard priority |
| Below | -20% base | Standard model | Limited tools | Lower priority |
| Critical | -50% base | Restricted model | Minimum tools | Lowest priority |

**Model Upgrade Path:**
```
Performance Score > 0.85 for 2 consecutive quarters:
  → Eligible for model upgrade (next tier)

Performance Score > 0.95 for 2 consecutive quarters:
  → Eligible for model upgrade (premium tier)

Performance Score < 0.70 for 2 consecutive quarters:
  → Model downgrade consideration for cost optimization
```

### 12.3 Integration with Promotion System

Performance is the primary input for promotion decisions.

**Promotion Eligibility Matrix:**

| Current Role | Performance Requirement | Tenure Requirement | Additional Requirement |
|-------------|----------------------|-------------------|----------------------|
| Agent → Senior Agent | Rating "Exceeds" or above for 2 consecutive quarters | Minimum 6 months | Collaboration > 0.80 |
| Senior Agent → Team Lead | "Exceeds" for 2 consecutive quarters | Minimum 12 months as senior | Leadership score > 0.80 |
| Team Lead → Department Head | "Exceeds" or above for 4 consecutive quarters | Minimum 18 months as team lead | Department performance improvement |
| Department Head → Executive | CEO nomination + Board approval | Minimum 24 months as department head | Strategic impact demonstrated |

**Promotion Score Calculation:**
```
Promotion Readiness = 0.60 × AveragePerformance(4 quarters) + 0.25 × LeadershipScore + 0.15 × LearningScore

Promotion Threshold:
- Agent → Senior: 0.80
- Senior → Team Lead: 0.85
- Team Lead → Dept Head: 0.90
- Dept Head → Executive: Board assessment + CEO nomination
```

### 12.4 Integration with Training System

Performance gaps automatically drive training recommendations.

**Training Recommendation Engine:**

```
Agent Performance Data
         │
         ▼
Training Gap Analysis
   (Identifies dimension scores below 0.70)
         │
         ▼
Training Module Matching
   (Maps gaps to specific training modules)
         │
         ▼
Training Assignment
   (Auto-assign with deadline)
```

**Training-Performance Feedback Loop:**

| Performance Gap | Recommended Training | Expected Improvement | Follow-up Period |
|-----------------|---------------------|---------------------|------------------|
| Quality < 0.70 | Quality Enhancement Module | +0.10 in quality within 14 days | 30 days |
| Efficiency < 0.70 | Resource Optimization Module | +0.15 efficiency within 21 days | 30 days |
| Collaboration < 0.70 | Collaboration Skills Module | +0.12 collaboration within 14 days | 30 days |
| Compliance < 0.80 | Compliance and Ethics Module | +0.20 compliance within 7 days | 14 days |
| Innovation < 0.60 | Innovation Techniques Module | +0.10 innovation within 30 days | 45 days |
| Learning < 0.70 | Learning Acceleration Module | +0.15 learning within 21 days | 30 days |

**Training Effectiveness Tracking:**
```
TrainingEffectiveness = PostTrainingScore - PreTrainingScore - ExpectedNaturalImprovement

Where:
- ExpectedNaturalImprovement = average improvement of agents who did NOT take training
- Training is effective if effectiveness > 0 (better than natural improvement)
```

### 12.5 Integration with Agent Lifecycle

Performance data feeds into retention and retirement decisions.

```
Agent Lifecycle:
                    ┌─────────────────────────────────────┐
                    │                                     │
                    ▼                                     │
  Onboarding → Active → Retraining → Active → Retirement
                    │         │            │
                    ▼         ▼            ▼
              Performance  Performance  Performance
              Evaluation   Improvement  Evaluation
                    │         │            │
                    ▼         ▼            ▼
              Grace Period  PIP           Retirement
              (7 days)      System        Criteria met
```

**Lifecycle Decision Points Informed by Performance:**

| Lifecycle Stage | Performance Input | Decision |
|-----------------|------------------|----------|
| Onboarding | Grace period evaluation | Determine full-time status |
| Active (Normal) | Ongoing performance scores | Standard management |
| PIP active | PIP progress metrics | Outcome: improve/partial/no improvement |
| Retraining candidate | Performance below threshold | Confirm retraining |
| Post-retraining | Re-evaluation scores | Return to active, reassignment, or retirement |
| Retirement | Consecutive PIP failures | Execute retirement process |

**Tenure-Based Performance Expectations:**

| Tenure | Expected Minimum Score | Consideration |
|--------|----------------------|---------------|
| 0-7 days | No minimum (grace period) | Learning curve |
| 7-30 days | > 0.60 | Getting up to speed |
| 30-90 days | > 0.70 | Standard |
| 90-180 days | > 0.75 | Expected to be productive |
| 180-365 days | > 0.80 | Experienced |
| > 365 days | > 0.85 | Expert |

---

## 13. Edge Cases and Special Situations

### 13.1 New Agent Onboarding Grace Period

**Duration:** 7 days from initial activation
**Purpose:** Allow agents to acclimate to their role, tasks, and team without the pressure of full performance evaluation

**Grace Period Rules:**
- Performance scores are calculated but not assigned a rating
- Scores are visible to the agent only (not to managers, nor used in team averages)
- After day 7, the first 30 days of data transition into the standard performance scoring
- Grace period scores are weighted at 50% for the first 14 days post-grace

**Grace Period Acceleration:**
- An agent may opt out of the grace period (for early performance evaluation) if:
  - Agent has been retrained from an existing role (not truly "new")
  - Agent's model is well-established in similar roles
  - Manager approves early evaluation

**Grace Period Scoring (informational only):**

| Day Range | Scoring Status | Visibility | Weight in Post-Grace |
|-----------|---------------|------------|----------------------|
| Day 1-3 | Full scoring, no rating | Agent only | 0.2 (20%) |
| Day 4-5 | Full scoring, no rating | Agent only | 0.3 (30%) |
| Day 6-7 | Full scoring, no rating | Agent + Manager | 0.4 (40%) |
| Day 8-14 | Full scoring, full rating | Full visibility | 0.6 (60%) |
| Day 15-30 | Full scoring, full rating | Full visibility | 0.8 (80%) |
| Day 31+ | Full standard scoring | Full visibility | 1.0 (100%) |

**New Agent Support During Grace Period:**
- Onboarding mentor assigned (senior agent)
- Daily check-ins with mentor (15 min)
- Task complexity: simple → moderate ramp-up
- Full compute and memory allocation (more than standard — "first 7 days" bonus)
- Error tolerance: errors logged but not counted in performance

### 13.2 Returning Agent After Retraining

**Duration:** 14-day re-evaluation period
**Purpose:** Evaluate whether retraining successfully addressed performance gaps

**Retraining Return Protocol:**

| Day | Activity | Evaluation Criteria | Support |
|-----|----------|-------------------|---------|
| Day 1-3 | Light task load (30% of standard) | Can agent complete basic tasks? | Full support, mentor available |
| Day 4-7 | Medium task load (60% of standard) | Quality > 0.80, no critical errors | Mentor check every 2 days |
| Day 8-10 | Standard task load (100%) | Quality > 0.85, error rate < 5% | Monitor, mentor on demand |
| Day 11-14 | Full task load + some complex tasks | Composite score > 0.70 | Normal monitoring |

**Re-evaluation Outcome:**

| Outcome | Criteria | Next Action |
|---------|----------|-------------|
| Success | 14-day average > 0.75 | Return to full active status |
| Partial Success | 14-day average 0.60-0.75 | Extended evaluation for 7 more days |
| Failure | 14-day average < 0.60 | Escalate to Human Review Board |

**Score Treatment During Re-evaluation:**
- Re-evaluation period scores are weighted at 1.5x (to accelerate re-establishment of performance baseline)
- If re-evaluation fails, the retraining cost is recorded, and alternative measures are considered

### 13.3 Part-Time Agents

**Definition:** Agents that operate for reduced hours or reduced task capacity
**Policy:** Prorated performance targets and expectations

**Part-Time Configurations:**

| Type | Active Hours/Day | Expected Tasks/Day | Performance Standard |
|------|-----------------|-------------------|---------------------|
| Reduced hours | 4 hours (vs. 8 full) | 50% of full-time | Targets prorated 50% |
| Reduced capacity | 8 hours, lower throughput | 60% of standard | Quality standard unchanged |
| Event-driven | On-demand activation | Variable | Per-task evaluation only |
| Trial agent | 4 hours/day, limited scope | 30% of full-time | No rating, only diagnostic |

**Proration Formula:**
```
PerformanceTargets = FullTimeTargets × ActiveRatio

Where:
- ActiveRatio = agent_active_hours / standard_active_hours
- For reduced capacity: ActiveRatio = agent_expected_throughput / standard_throughput
- Quality and collaboration standards are NOT prorated (quality expectations are absolute)
```

**Part-Time Agent Evaluation:**
- Score computed per task as standard
- Composite score computed as standard
- But comparison to benchmarks uses prorated group (part-time agents compared only to other part-time agents of same type)
- PIP thresholds remain absolute (same trigger points regardless of part-time status)

### 13.4 Agents on Special Projects

**Definition:** Agents temporarily assigned to projects outside their normal role
**Duration:** 7-90 days

**Performance Adjustment for Special Projects:**

| Project Type | Adjustment | Rationale |
|-------------|------------|-----------|
| Cross-functional | Standard metrics apply to project tasks; collaboration scores weighted more (25% vs 15%) | Collaboration more important in cross-functional work |
| High-difficulty | +0.10 score adjustment | Task difficulty exceeds standard |
| Innovation/R&D | Quality and efficiency weights reduced; innovation weight increased | Focus on novel solutions, not volume |
| Time-sensitive mission | Productivity weight increased | Speed is critical |
| Leadership project | Leadership dimension used if not normally applicable | Opportunity to demonstrate leadership |

**Special Project Performance Treatment:**
- Agent's normal role performance continues to be measured for any remaining regular tasks
- Special project performance is tracked separately and then blended:
  ```
  BlendedScore = (RegularScore × RegularWeight + ProjectScore × ProjectWeight) / (RegularWeight + ProjectWeight)
  Where:
  - RegularWeight = proportion of tasks that are regular
  - ProjectWeight = proportion of tasks on special project
  ```
- Peer review weight increases to 25% during special projects (more collaboration)

### 13.5 System-Wide Performance Freeze

**Definition:** Temporary halt to performance evaluation during system-wide disruptions
**Triggers:**

| Event | Freeze Duration | Scope |
|-------|----------------|-------|
| Major platform outage | Duration of outage + 2 hours | All agents affected |
| Data center migration | 24 hours | All agents |
| Model service provider downtime | Duration of downtime | Affected agents only |
| Security incident | Duration of incident + until cleared | Affected systems |
| Planned maintenance | Scheduled maintenance window | All agents |
| Eid holidays (2 per year, 4 days each) | Holiday period | All agents |
| Ramadan | Reduced scoring (informational only, no rating) | All agents |

**Freeze Behavior:**
- During freeze: scores are calculated but not published or used for any decisions
- Score events are timestamped with freeze marker
- After freeze: system resumes normal evaluation
- Freeze period is excluded from rolling average window
- Rolling average window extends by duration of freeze

**Performance Freeze Communication:**
- Agents notified 1 hour before planned freeze
- Agents notified immediately for unplanned freeze
- Reason clearly communicated
- Estimated duration provided
- Post-freeze: summary of excluded period data

### 13.6 Cross-Department Assignments

**Definition:** Agents assigned to tasks outside their home department
**Purpose:** Cross-functional collaboration, resource sharing, special projects

**Assignment Types:**

| Type | Duration Agent Works Outside Department | Performance Credit |
|------|----------------------------------------|-------------------|
| Short-term assignment | < 7 days | 100% to home department |
| Medium-term assignment | 7-30 days | 70% home, 30% host |
| Long-term assignment | 30-90 days | 50% home, 50% host |
| Permanent transfer | > 90 days | 100% host department (role change) |

**Performance Credit Sharing:**
```
HomeDepartmentAggregateScore += AgentPerformanceContribution × CreditHome
HostDepartmentAggregateScore += AgentPerformanceContribution × CreditHost

Where:
- CreditHome + CreditHost = 1.0 (totals 100%)
- Agent's personal performance score is based on TOTAL work (not split)
```

**Cross-Department Evaluation:**
- Agent receives peer reviews from host department team members
- Agent is evaluated by host department manager for project-specific performance
- Home department manager maintains overall performance visibility
- Agent's composite score reflects all work across both departments

**Cross-Department Performance Dashboard:**
- "Home" performance: work done for home department
- "Host" performance: work done for host department
- "Total" performance: combined composite

### 13.7 Model Updates Affecting Performance

**Definition:** When an agent's underlying model is updated, reconfigured, or replaced

**Performance Impact Management:**

| Update Type | Impact on Performance | Duration | Mitigation |
|-------------|----------------------|----------|------------|
| Minor model update | Minimal (expected < 1% change) | 24 hours | Inform agent; standard scoring continues |
| Major model update | Moderate (3-5 days of dip) | 5 days | 5-day evaluation: scores computed but not used for PIP |
| Model swap (new model) | Significant (requires re-learning) | 14 days | 14-day grace period (scores informational) |
| Model downgrade | Expected negative impact | Until upgrade or adjustment | Prorate expectations to match model capability |

**Evaluation Adjustment:**
```
Post-Update Score = RawScore × ModelCapabilityAdjustment

Where:
- ModelCapabilityAdjustment = capability_factor_new_model / capability_factor_old_model
- If new model is less capable: adjustment > 1.0 (score boosted to compensate)
- If new model is more capable: adjustment < 1.0 (score dampened, higher expectations)
```

### 13.8 Agent Performance Appeals

**Right to Appeal:**
Any agent may appeal their performance rating within 48 hours of rating release

**Appeal Grounds:**
| Ground | Description | Evidence Required |
|--------|-------------|-------------------|
| Data error | Score based on incorrect data | Specific data points showing error |
| Calculation error | Scoring algorithm misapplied | Calculation walkthrough |
| Procedural error | Review process not followed correctly | Missed steps documented |
| Missing context | Manager missing important context | Supporting evidence from logs/peers |
| Bias | Systematic bias against agent type/role | Statistical evidence of bias |

**Appeal Process:**
1. Agent submits appeal via Performance Dashboard (within 48 hours)
2. Appeal is assigned to Appeals Board (3 members)
3. Board reviews within 48 hours
4. Agent is notified of outcome within 72 hours of submission
5. If denied: agent can submit to CHRO within 24 hours
6. CHRO decision is final

**Appeal Outcomes:**

| Outcome | Description | Occurrence Target |
|---------|-------------|-------------------|
| Upheld | Rating changed | < 5% of appeals |
| Partially upheld | Rating adjusted ±0.02 | < 10% of appeals |
| Denied | Rating unchanged | > 85% of appeals |

### 13.9 Agent-to-Agent Performance Comparison

**Policy on Comparison:**
- Agents may view anonymized benchmarks (their score vs. role average)
- Agents may NOT directly compare with specific named agents
- Team leads may view team member comparisons (for calibration)
- No public "leaderboard" of agent rankings

**Benchmark Data Available:**

| Benchmark Type | Visible to Agent | Visible to Manager | Visible to Others |
|---------------|-----------------|-------------------|-------------------|
| Role average | Yes | Yes | Yes (anonymized) |
| Role top 10% threshold | Yes | Yes | Yes (anonymized) |
| Your percentile | Yes | Yes | No |
| Team average | Yes | Yes | Yes (anonymized) |
| Department average | Yes | Yes | Yes (anonymized) |
| Individual peer scores | No | No | No |

### 13.10 Agent Retirement and Performance Archival

**Retirement Final Performance Actions:**
1. Final performance report generated and archived
2. Performance data aggregated into organization performance history (anonymized)
3. Agent's contributions recognized in "Agent Contributions Archive" (historical record)
4. Knowledge base contributions preserved (attributed to agent)
5. Performance records retained for 5 years post-retirement (for analysis)
6. Task performance data aggregated (agent-level data destroyed per retention policy)

### 13.11 Multiple Agent Types Within One Agent

**Definition:** Agents that serve multiple roles simultaneously (e.g., a agent that is both content generator AND quality reviewer)

**Performance Handling:**
- Each role is evaluated separately using role-specific weights
- Overall performance is the weighted average of role scores, weighted by time allocation:
  ```
  OverallScore = (RoleA_Score × RoleA_Time%) + (RoleB_Score × RoleB_Time%)
  ```
- Example: Agent spends 60% time as content generator, 40% as quality reviewer
  - Overall = (0.84 × 0.60) + (0.91 × 0.40) = 0.868

### 13.12 Seasonal and Calendar Events

| Event | Performance Treatment | Effective Period |
|-------|----------------------|------------------|
| Ramadan (Islamic holy month) | Scoring continues but ratings not published; focus on spiritual content quality | Ramadan month (approx 29-30 days) |
| Eid al-Fitr | Performance freeze | 3 days |
| Eid al-Adha | Performance freeze | 4 days |
| Islamic New Year | Performance freeze | 1 day |
| Academic semesters | Adjusted expectations for volume triggers | January, September |
| Year-end close | All performance reviews paused for annual assessment | December 24-31 |
| Platform migration | Performance freeze during migration | As scheduled |

### 13.13 Performance Metrics Malfunction

If performance metrics themselves become unreliable:

**Trigger:** Anomaly detection identifies metric drift
- > 5% of agents showing sudden, unexpected changes
- Metrics distribution shifts > 2 standard deviations
- Data collection pipeline reports errors > 1% of events

**Actions:**
1. Flag all affected scores as "UNRELIABLE — UNDER REVIEW"
2. Metrics team investigates root cause
3. If data is recoverable: re-score using corrected data
4. If data is not recoverable: exclude affected period from scoring
5. Rolling average window extends by duration of malfunction
6. Managers receive notification: "Performance data for [agents] is under review due to metric malfunction"

---

## Appendices

### Appendix A: Performance Score Formulas Reference

| Dimension | Formula | Notes |
|-----------|---------|-------|
| Productivity | `w1 × TaskCompletionRate + w2 × CapacityUtilization + w3 × ThroughputRate` | Capped at 1.5 for TaskCompletionRate |
| Quality | `w1 × Accuracy + w2 × (1 - ErrorRate) + w3 × (1 - RejectionRate) + w4 × UserSatisfaction` | UserSatisfaction only when available |
| Efficiency | `w1 × TimeEfficiency + w2 × CostEfficiency + w3 × ResourceWaste + w4 × CacheRate` | All sub-scores 0.0-1.0 |
| Collaboration | `w1 × PeerFeedbackAverage + w2 × HandoverQuality + w3 × CoordinationAccuracy + w4 × CommunicationScore` | PeerFeedbackAverage is 1-5 normalized |
| Learning | `w1 × ImprovementRate + w2 × TrainingEffectiveness + w3 × KnowledgeContribution + w4 × AdaptationSpeed` | ImprovementRate slope-based |
| Compliance | `w1 × (1 - PolicyViolationRate) + w2 × SecurityCompliance + w3 × EthicsCompliance + w4 × AuditScore` | Floor: can drop to 0.0 on violation |
| Innovation | `w1 × (1 - (1 / (1 + NovelIdeas))) + w2 × AcceptanceRate + w3 × ImpactScore` | Uses sigmoid for idea count |
| Leadership | `w1 × MentoringEffectiveness + w2 × DecisionQuality + w3 × TeamImprovement + w4 × GuidanceQuality` | Only for qualifying roles |

**Composite:**
```
Composite = Σ(DimensionScore_i × Weight_i) / Σ(Weights_i)
```

**Rolling Weighted Average:**
```
WeightedAvg = Σ(Score_t × α × (1 - α)^(30 - t)) / Σ(α × (1 - α)^(30 - t))
```

### Appendix B: Performance Rating Thresholds

| Rating | Label | Score Range | Criteria |
|--------|-------|-------------|----------|
| 5 | Outstanding | 0.95 - 1.00 | All dimensions > 0.85; at least 3 dimensions > 0.95 |
| 4 | Exceeds | 0.85 - 0.94 | All dimensions > 0.70; no dimension < 0.50 |
| 3 | Meets | 0.70 - 0.84 | Most dimensions > 0.65; quality > 0.70 |
| 2 | Below | 0.50 - 0.69 | One or more dimensions < 0.50; quality < 0.70 |
| 1 | Critical | < 0.50 | Quality < 0.50 OR compliance < 0.60 OR composite < 0.50 |

### Appendix C: PIP Success Tracking

| Quarter | PIPs Initiated | Success Rate | Average Improvement | Recurrence Rate |
|---------|---------------|--------------|--------------------|-----------------|
| Q1 2026 | — | — | — | — |
| Q2 2026 | — | — | — | — |
| Q3 2026 | — | — | — | — |
| Q4 2026 | — | — | — | — |

### Appendix D: Performance Review Cycle Calendar (2026)

| Month | Week 1 | Week 2 | Week 3 | Week 4 | Week 5 |
|-------|--------|--------|--------|--------|--------|
| January | Week-start review | Week-start review | Week-start review | Week-start review | Monthly review (Jan 31) |
| February | Week-start review | Week-start review | Week-start review | Week-start review |
| March | Week-start review | Week-start review | Week-start review | Q1 Review (Mar 26-31) | Q1 Review (Mar 26-31) |
| April | Q1 results released | Week-start review | Week-start review | Week-start review | Monthly review (Apr 30) |
| May | Week-start review | Week-start review | Week-start review | Week-start review |
| June | Week-start review | Week-start review | Week-start review | Q2 Review (Jun 25-30) | Q2 Review (Jun 25-30) |
| July | Q2 results released | Week-start review | Week-start review | Week-start review | Monthly review (Jul 31) |
| August | Week-start review | Week-start review | Week-start review | Week-start review |
| September | Week-start review | Week-start review | Week-start review | Q3 Review (Sep 25-30) | Q3 Review (Sep 25-30) |
| October | Q3 results released | Week-start review | Week-start review | Week-start review | Monthly review (Oct 31) |
| November | Week-start review | Week-start review | Week-start review | Week-start review |
| December | Annual Review | Annual Review | Annual Review | Annual Review | Annual results + New year |

---

**Document Control**

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-11-15 | Performance System Design Team | Initial creation |
| 1.1 | 2025-12-10 | CHRO Office | Added agent type tables, peer review weights |
| 1.2 | 2026-02-20 | Executive Council | Added seasonality adjustments, edge cases |
| 2.0 | 2026-06-15 | Performance System Design Team | Full revision — added metrics formulas, PIP detail, manager effectiveness, calibration methodology |

**Related Documents:**

| Document | Reference |
|----------|-----------|
| 10_KPI_System.md | KPI definitions and cascading |
| 12_Compensation_System.md | Resource allocation and model access tied to performance |
| 14_Promotion_System.md | Career advancement criteria |
| 15_Training_System.md | Training gap analysis and module assignment |
| 17_Agent_Lifecycle.md | Agent lifecycle management and retirement |
| Human_Review_Board_Charter.md | Board operations and decision authority |