# 17. KPI System: The Measurement Infrastructure for an AI-Powered Organization

> **Document Version:** 1.0
> **Last Updated:** 2026-07-09
> **Classification:** Internal — CE-AI, Executives, Department Managers
> **Applies To:** All 8,500 AI Agents, 130 Teams, 33 Departments, 7 Executives, CE-AI
> **Mission Context:** Building the world's first AI-powered Islamic Education Platform

---

## Table of Contents

1. [KPI Framework](#1-kpi-framework)
2. [Company-Level KPIs (Strategic)](#2-company-level-kpis-strategic)
3. [Department-Level KPIs](#3-department-level-kpis)
4. [Team-Level KPIs](#4-team-level-kpis)
5. [Agent-Level KPIs](#5-agent-level-kpis)
6. [KPI Calculation Methods](#6-kpi-calculation-methods)
7. [KPI Thresholds and Alerting](#7-kpi-thresholds-and-alerting)
8. [KPI Dashboards and Reporting](#8-kpi-dashboards-and-reporting)
9. [KPI Review Process](#9-kpi-review-process)
10. [KPI Cascading and Alignment](#10-kpi-cascading-and-alignment)
11. [KPI Edge Cases](#11-kpi-edge-cases)
12. [KPI System Architecture](#12-kpi-system-architecture)

---

## 1. KPI Framework

### 1.1 KPI Hierarchy: Four-Tier Model

The KPI system is structured as a strict 4-tier hierarchy that ensures strategic intent flows downward and performance visibility flows upward. Every KPI at a lower tier must trace directly to a parent KPI at the tier above, forming a fully connected tree from the CE-AI's strategic objectives down to individual agent behaviors.

| Tier | Scope | Owner | Number of KPIs | Review Frequency | Aggregation |
|------|-------|-------|----------------|------------------|-------------|
| 1 — Company | Enterprise-wide | CE-AI & Board | 25 | Quarterly/Monthly | Board-level reporting |
| 2 — Department | 33 departments | Department Managers | 15 per dept (495 total) | Weekly/Monthly | Executive dashboards |
| 3 — Team | 130 teams | Team Leads | 8 per team (1,040 total) | Daily/Weekly | Department drill-downs |
| 4 — Agent | 8,500 agents | Agent Health System | 6 per agent (51,000 total) | Real-time/Hourly | Individual performance |

**Cascading Principle:** Each company KPI decomposes into 2-5 department KPIs. Each department KPI decomposes into 2-4 team KPIs. Each team KPI decomposes into 1-3 agent KPIs. A KPI at any level is considered "met" only when a weighted majority of its child KPIs are in Green status.

### 1.2 Leading vs Lagging KPIs

The system distinguishes two fundamental KPI types, both of which are tracked for every metric category.

| Characteristic | Leading KPIs (Predictive) | Lagging KPIs (Outcome) |
|---------------|--------------------------|------------------------|
| Purpose | Forecast future outcomes | Measure past results |
| Examples | Content throughput, agent training hours, verification pipeline volume | Revenue, user retention, content quality score |
| Time Orientation | Current/forward-looking | Historical (7d, 30d, 90d lag) |
| Actionability | High — can be influenced immediately | Low — reflects decisions made weeks ago |
| Correlation | Leading indicators should correlate with lagging outcomes at R-squared >= 0.7 | — |
| Weight in Reviews | 40% of review focus | 60% of review focus |

**Leading-to-Lagging Mapping Examples for Islamic Ed Platform:**

| Lagging KPI | Leading KPI(s) | Expected Lag | Correlation Target |
|-------------|---------------|--------------|-------------------|
| User Retention D30 | D7 engagement score, content freshness rate | 23 days | R-squared >= 0.75 |
| Revenue Growth | New user acquisition rate, conversion rate | 45 days | R-squared >= 0.70 |
| Content Quality Score | Verification pass rate, first-pass yield | 7 days | R-squared >= 0.80 |
| Platform Uptime 99.99% | Error budget burn rate, deployment frequency | 1 hour | R-squared >= 0.90 |

### 1.3 Quantitative vs Qualitative KPIs

| Dimension | Quantitative KPIs | Qualitative KPIs |
|-----------|-------------------|-----------------|
| Measurement | Numeric, automated | Scaled, human or model-evaluated |
| Data Source | System logs, database counts, API metrics | User surveys, model eval scores, expert review |
| Objectivity | High — directly measured | Medium — requires calibration |
| Frequency | Real-time to daily | Weekly to monthly |
| Examples | Items/hr, uptime %, error rate | NPS, user satisfaction score, creative diversity score |
| Percentage of Total KPIs | 75% | 25% |

**Qualitative KPI Calibration Protocol:**
- User satisfaction surveys use a 7-point Likert scale (1 = extremely dissatisfied, 7 = extremely satisfied)
- Content quality scores are the average of 3 independent AI reviewer evaluations
- Creative diversity scores use a validated rubric with 5 dimensions (thematic variety, source diversity, pedagogical approach, medium diversity, language variety)
- All qualitative scores are normalized to a 0-100 scale for dashboard consistency
- Inter-rater reliability must be Cohen's Kappa >= 0.80; if below, re-calibrate evaluators

### 1.4 KPI Lifecycle

Every KPI follows a six-stage lifecycle managed by the KPI Governance Board.

```
Stage 1: Define --> Stage 2: Set Target --> Stage 3: Measure --> Stage 4: Report --> Stage 5: Review --> Stage 6: Adjust
(who, what, how)  (baseline, stretch,  (collect, calculate, (dashboard, alert,  (analyze, root   (re-target, re-weight,
                   minimum)             validate)            notify)             cause)            deprecate)
```

**Stage 1 — Definition:**
- KPI name, unique ID (format: KPI-[Dept]-[Number], e.g., KPI-CG-001 for Content Generation)
- Description, formula, unit of measure
- Owner (individual or role), stakeholder list
- Data source(s), collection method, refresh frequency
- Leading/lagging classification, quantitative/qualitative classification

**Stage 2 — Target Setting:**
- Baseline value (historical 90-day average or initial estimate)
- Stretch target (aspirational but achievable, ~20% above baseline)
- Minimum threshold (floor below which corrective action triggers)
- Target horizon (quarterly, yearly, multi-year)
- Approval: CE-AI for company KPIs, Department Manager for dept KPIs, Team Lead for team KPIs

**Stage 3 — Measurement:**
- Automated data collection from event streams (99% of KPIs)
- Manual input only for qualitative survey data
- Validation checks: completeness check, outlier detection, consistency check, staleness check
- Calculation engine executes formulas (see Section 6)

**Stage 4 — Reporting:**
- Real-time dashboards (see Section 8)
- Scheduled PDF/HTML reports via email and Slack
- Alert notifications for threshold breaches (see Section 7)
- Anomaly flags for statistical outliers

**Stage 5 — Review:**
- Root cause analysis for Red/Yellow KPIs using the 5-Whys method
- Comparison against prior period, same period last year, and forecast
- Documentation of findings in the KPI Review Log (database table: kpi_review_log)
- Action items assigned with owners and deadlines

**Stage 6 — Adjustment:**
- Target adjustments: up to 10% per quarter without board approval; >10% requires CE-AI approval
- KPI deprecation: a KPI may be retired if it has been Green for 4 consecutive quarters AND is no longer strategically relevant
- New KPI introduction: requires business case, data source validation, 30-day shadow period (tracked but not reported), then formal approval
- Re-weighting: KPI weights in composite scores adjusted annually during strategic planning

### 1.5 Measurement Frequencies

| Frequency | KPIs Measured | Use Case | Data Freshness SLA |
|-----------|--------------|----------|-------------------|
| Real-time (sub-second) | Agent uptime, system latency, error budget, throughput | Operational monitoring, automated scaling decisions | < 1 second |
| Hourly | Agent productivity (tasks/hr), response times, active agent count | Shift management, load balancing | < 5 minutes |
| Daily | Content output (items/day), verification throughput, user DAU, new signups | Team standup reporting, daily ops | < 15 minutes |
| Weekly | Quality scores, retention D7, engagement metrics, pipeline value | Department review meetings | < 1 hour |
| Monthly | Revenue (MRR, ARPU), LTV, NPS, CSAT, churn rate, ROI per department | Executive monthly business review | < 4 hours |
| Quarterly | Strategic milestones, LTV/CAC ratio, market share, annual recurring revenue | Board meetings, strategy adjustment | < 24 hours |
| Yearly | Company valuation, market position, mission impact metrics | Annual report, strategic planning | < 48 hours |

---

## 2. Company-Level KPIs (Strategic)

### 2.1 Revenue KPIs

| KPI ID | KPI Name | Formula | Target | Frequency | Owner |
|--------|----------|---------|--------|-----------|-------|
| KPI-COMP-001 | Monthly Recurring Revenue (MRR) | SUM(all active subscription revenue) | Confidential | Monthly | CE-AI, Finance |
| KPI-COMP-002 | Average Revenue Per User (ARPU) | MRR / total active users | Confidential | Monthly | CE-AI, Marketing |
| KPI-COMP-003 | Customer Lifetime Value (LTV) | ARPU x average customer lifespan (months) | Confidential | Monthly | Finance, Sales |
| KPI-COMP-004 | Free-to-Paid Conversion Rate | (new paid users / new free users) x 100 | 8.5% | Weekly | Sales, Marketing |
| KPI-COMP-005 | Subscription Growth Rate (MoM) | ((MRR_current - MRR_prior) / MRR_prior) x 100 | 5% | Monthly | CE-AI |
| KPI-COMP-006 | Annual Recurring Revenue (ARR) | MRR x 12 | Confidential | Quarterly | CE-AI, Board |
| KPI-COMP-007 | Revenue Per Platform Tier | Revenue per tier (Free/Basic/Plus/Family/Madrasa) | Tier targets | Monthly | Finance |
| KPI-COMP-008 | LTV / CAC Ratio | LTV / Customer Acquisition Cost | >= 3.5x | Monthly | CE-AI, Finance |

**Islamic Ed Platform Specific Revenue Context:**
- Subscription tiers: Free (limited content), Basic ($4.99/mo, core curriculum), Plus ($9.99/mo, full platform), Family ($14.99/mo, up to 6 profiles), Madrasa ($49.99/mo, institutional with admin dashboard)
- Revenue is Halal-compliant: no interest-based pricing, no gambling mechanics, transparent billing
- Zakat (2.5% of net platform profit) is automatically calculated and earmarked quarterly

### 2.2 User KPIs

| KPI ID | KPI Name | Formula | Target | Frequency | Owner |
|--------|----------|---------|--------|-----------|-------|
| KPI-COMP-009 | Monthly Active Users (MAU) | COUNT(distinct users with >= 1 session in 30d) | Confidential | Monthly | Marketing, Product |
| KPI-COMP-010 | Daily Active Users (DAU) | COUNT(distinct users with >= 1 session in 24h) | Confidential | Daily | Product |
| KPI-COMP-011 | DAU/MAU Ratio | DAU / MAU | >= 0.35 | Daily | Product |
| KPI-COMP-012 | D1 Retention Rate | (users active day 1 / users who installed) x 100 | 65% | Daily | Product, Marketing |
| KPI-COMP-013 | D7 Retention Rate | (users active day 7 / users who installed) x 100 | 45% | Weekly | Product |
| KPI-COMP-014 | D30 Retention Rate | (users active day 30 / users who installed) x 100 | 30% | Monthly | Product |
| KPI-COMP-015 | Net Promoter Score (NPS) | (% Promoters - % Detractors) x 100 | >= 50 | Monthly | CE-AI, Product |
| KPI-COMP-016 | User Engagement Score | Composite: session length x session frequency x feature breadth | >= 72/100 | Weekly | Product |
| KPI-COMP-017 | Daily Session Duration per User | SUM(session durations) / COUNT(users) | >= 18 min | Daily | Product |
| KPI-COMP-018 | Churn Rate (Monthly) | (users lost in month / users at month start) x 100 | < 5% | Monthly | Product, Sales |
| KPI-COMP-019 | User Acquisition Cost (UAC) | Total marketing spend / new users acquired | Confidential | Monthly | Marketing |

### 2.3 Content KPIs

| KPI ID | KPI Name | Formula | Target | Frequency | Owner |
|--------|----------|---------|--------|-----------|-------|
| KPI-COMP-020 | Total Content Items in Library | COUNT(all approved content items) | 500,000+ | Daily | Content Director |
| KPI-COMP-021 | Quran Content Coverage Rate | (Quran verses referenced / 6,236 total verses) x 100 | 85% | Monthly | Content, Islamic Verification |
| KPI-COMP-022 | Hadith Content Coverage Rate | (Hadith referenced / target corpus) x 100 | 60% | Monthly | Content |
| KPI-COMP-023 | Fiqh Content Coverage Rate | (Fiqh topics covered / target curriculum topics) x 100 | 75% | Monthly | Content |
| KPI-COMP-024 | Fresh Content Rate (New/Day) | COUNT(content items published) per day | >= 500/day | Daily | Content Generation |
| KPI-COMP-025 | Content Depth Score | AVG(sources per topic, explanation length, reference count) | >= 7.5/10 | Monthly | Content, Islamic Verification |
| KPI-COMP-026 | Multi-Language Content Ratio | (items in non-Arabic / total items) x 100 | 40% | Monthly | Content |
| KPI-COMP-027 | Content Format Diversity | Shannon entropy of content format distribution | >= 3.2 bits | Monthly | Content Generation |

### 2.4 Quality KPIs

| KPI ID | KPI Name | Formula | Target | Frequency | Owner |
|--------|----------|---------|--------|-----------|-------|
| KPI-COMP-028 | Overall Content Accuracy Rate | (accurate content items / total verified items) x 100 | >= 99.5% | Daily | Islamic Verification |
| KPI-COMP-029 | User Satisfaction Score | AVG(user rating on 1-7 scale) normalized to /5 | >= 4.5 / 5 | Weekly | Product |
| KPI-COMP-030 | Content Error Rate | (items with errors detected / total items published) x 100 | < 0.5% | Daily | Islamic Verification, QA |
| KPI-COMP-031 | Islamic Authenticity Score | Composite by scholars: adherence to authentic sources | >= 9.2 / 10 | Monthly | Islamic Scholars Board |
| KPI-COMP-032 | Pedagogical Effectiveness Score | User learning outcome improvement from pre-test to post-test | >= 25% improvement | Quarterly | AI Teacher, Content |
| KPI-COMP-033 | Accessibility Score | WCAG 2.1 AA compliance percentage of content items | >= 95% | Monthly | Engineering, QA |

### 2.5 Efficiency KPIs

| KPI ID | KPI Name | Formula | Target | Frequency | Owner |
|--------|----------|---------|--------|-----------|-------|
| KPI-COMP-034 | Content Throughput (Items/hr) | (total content items published) / total production hours | >= 120/hr | Daily | Content Generation |
| KPI-COMP-035 | Cost Per Content Item | Total content operations cost / total items published | Confidential | Monthly | Finance, Content |
| KPI-COMP-036 | API Response Time (P50) | PERCENTILE(response_time, 0.50) | < 200ms | Real-time | Engineering |
| KPI-COMP-037 | API Response Time (P95) | PERCENTILE(response_time, 0.95) | < 500ms | Real-time | Engineering |
| KPI-COMP-038 | API Response Time (P99) | PERCENTILE(response_time, 0.99) | < 2000ms | Real-time | Engineering |
| KPI-COMP-039 | Agent Utilization Rate | (active production time / total available time) x 100 | >= 85% | Daily | HR/AgentOps |
| KPI-COMP-040 | Time-to-Publish (Idea to Live) | MEDIAN(hours from content brief to publication) | < 48 hrs | Weekly | Content |
| KPI-COMP-041 | Review Cycle Efficiency | SUM(review hours) / SUM(content items reviewed) | < 0.5 hrs/item | Daily | Islamic Verification |

### 2.6 Platform KPIs

| KPI ID | KPI Name | Formula | Target | Frequency | Owner |
|--------|----------|---------|--------|-----------|-------|
| KPI-COMP-042 | Platform Uptime (Monthly) | ((total minutes - downtime minutes) / total minutes) x 100 | 99.99% | Monthly | Engineering |
| KPI-COMP-043 | Platform Uptime (Rolling 12mo) | ((total minutes - downtime minutes) / total minutes) x 100 | 99.99% | Quarterly | Engineering |
| KPI-COMP-044 | Error Budget (Monthly Burn) | 1 - uptime = allowed error minutes; actual error minutes / allowed | < 80% consumption | Daily | Engineering |
| KPI-COMP-045 | P50 Latency (API Gateway) | PERCENTILE(latency_ms, 0.50) | < 100ms | Real-time | Engineering |
| KPI-COMP-046 | P99 Latency (API Gateway) | PERCENTILE(latency_ms, 0.99) | < 1000ms | Real-time | Engineering |
| KPI-COMP-047 | System Error Rate | (5xx responses / total requests) x 100 | < 0.01% | Real-time | Engineering |
| KPI-COMP-048 | Database Query Performance (P95) | PERCENTILE(query_time_ms, 0.95) | < 100ms | Real-time | Engineering |
| KPI-COMP-049 | Cache Hit Ratio | (cache hits / (cache hits + cache misses)) x 100 | >= 90% | Real-time | Engineering |
| KPI-COMP-050 | Concurrent User Capacity | MAX(concurrent sessions without degradation) | 50,000 | Weekly | Engineering |

---

## 3. Department-Level KPIs

### 3.1 Content Acquisition Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-CA-001 | Items Acquired per Hour | (total items sourced) / acquisition team hours | >= 200 items/hr | Hourly |
| KPI-CA-002 | Acquisition Cost per Item | Total acquisition spend / total items acquired | Confidential | Monthly |
| KPI-CA-003 | Source Diversity Score | Shannon index of source types | >= 2.5 bits | Weekly |
| KPI-CA-004 | Verification Pass Rate (Pre-Screening) | (items passing initial screening / items submitted) x 100 | >= 85% | Daily |
| KPI-CA-005 | Source Authenticity Score | Authenticity verification result of each source | >= 9.0 / 10 | Monthly |
| KPI-CA-006 | Acquisition-to-Pipeline Time | MEDIAN(time from acquisition to content pipeline) | < 24 hours | Weekly |
| KPI-CA-007 | License Compliance Rate | (licensed items / total acquired) x 100 | 100% | Monthly |
| KPI-CA-008 | Manuscript Digitization Rate | (manuscript pages scanned and OCR'd / total manuscript pages) x 100 | >= 95% | Monthly |
| KPI-CA-009 | Rare Source Acquisition Count | Distinct rare/unique sources acquired per month | >= 10/month | Monthly |
| KPI-CA-010 | Rights Clearance Time | MEDIAN(days to clear rights for copyrighted content) | < 30 days | Monthly |

### 3.2 Content Generation Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-CG-001 | Stories/Articles Generated per Hour | (total content items generated) / generation team hours | >= 80 items/hr | Hourly |
| KPI-CG-002 | First-Pass Quality Pass Rate | (items passing first verification / total items submitted) x 100 | >= 82% | Daily |
| KPI-CG-003 | Review Cycle Time | MEDIAN(time from submission to verification decision) | < 4 hours | Daily |
| KPI-CG-004 | Creative Diversity Score | Composite: thematic, source, medium, language, pedagogical | >= 7.5/10 | Weekly |
| KPI-CG-005 | Content Originality Score | AI-detected similarity to existing content | < 15% similarity | Daily |
| KPI-CG-006 | Generation Rejection Rate | (items rejected / items submitted) x 100 | < 10% | Daily |
| KPI-CG-007 | Multi-Format Output Rate | Percentage of content items generated in >= 2 formats | >= 60% | Weekly |
| KPI-CG-008 | Age-Appropriateness Score | Match between content reading level and target audience age | >= 8.5/10 | Weekly |

### 3.3 Islamic Verification Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-IV-001 | Verification Throughput | (items verified) / verification agent hours | >= 50 items/hr | Hourly |
| KPI-IV-002 | Verification Accuracy Rate | (correct verifications / total verifications) x 100 | >= 99.9% | Daily |
| KPI-IV-003 | False Positive Rate | (items incorrectly rejected / total items verified) x 100 | < 0.05% | Daily |
| KPI-IV-004 | False Negative Rate | (errors missed / total items verified) x 100 | < 0.05% | Daily |
| KPI-IV-005 | Quranic Citation Accuracy | (correct Quran citations / total citations checked) x 100 | 100% | Daily |
| KPI-IV-006 | Hadith Authentication Accuracy | (correctly graded Hadith / total Hadith checked) x 100 | >= 99.5% | Daily |
| KPI-IV-007 | Fiqh Ruling Correctness | (correct rulings / total rulings verified) x 100 | >= 99.0% | Daily |
| KPI-IV-008 | Verification Consistency Score | Inter-verifier agreement (Cohen's Kappa) | >= 0.90 | Weekly |
| KPI-IV-009 | Time to Verification Decision | MEDIAN(minutes from assignment to decision) | < 30 min | Real-time |
| KPI-IV-010 | Escalation to Human Scholar Rate | (items escalated / total items) x 100 | < 2% | Daily |
| KPI-IV-011 | Scholar Feedback Incorporation Time | MEDIAN(time to implement scholar-requested corrections) | < 2 hours | Daily |
| KPI-IV-012 | Ijma (Consensus) Alignment Score | Percentage of verification decisions aligned with scholarly consensus | >= 98% | Monthly |

### 3.4 AI Teacher Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-AT-001 | Query Resolution Rate | (resolved queries / total queries) x 100 | >= 92% | Daily |
| KPI-AT-002 | User Satisfaction with AI Teacher | AVG(user rating of AI Teacher responses) on 1-7 scale | >= 6.0 / 7 | Weekly |
| KPI-AT-003 | Average Response Time | MEDIAN(seconds from query to first response) | < 3 seconds | Real-time |
| KPI-AT-004 | Follow-Up Accuracy | (correct follow-up responses / total follow-ups) x 100 | >= 88% | Daily |
| KPI-AT-005 | Personalization Score | Degree of content adaptation to user learning history | >= 8.0/10 | Weekly |
| KPI-AT-006 | Learning Objective Completion Rate | (objectives achieved / objectives assigned) x 100 | >= 75% | Monthly |
| KPI-AT-007 | Concept Mastery Rate | (users scoring >= 80% on assessment / total users) x 100 | >= 70% | Monthly |
| KPI-AT-008 | Query-to-Assessment Correlation | Correlation between queries asked and assessment performance | R-squared >= 0.6 | Monthly |
| KPI-AT-009 | Misinformation Detection Rate | (incorrect responses caught / total responses) x 100 | >= 99.9% | Daily |
| KPI-AT-010 | Sensitive Query Handling Score | Specialized score for sensitive theological consultation | >= 9.0/10 | Monthly |

### 3.5 Engineering Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-ENG-001 | Deployment Frequency | Number of deployments to production per week | >= 20/week | Weekly |
| KPI-ENG-002 | Change Failure Rate | (deployments causing incidents / total deployments) x 100 | < 5% | Weekly |
| KPI-ENG-003 | Mean Time to Recover (MTTR) | MEDIAN(minutes from incident detection to full recovery) | < 15 min | Weekly |
| KPI-ENG-004 | Mean Time Between Failures (MTBF) | MEAN(hours between unplanned outages) | >= 720 hours | Monthly |
| KPI-ENG-005 | Lead Time for Change | MEDIAN(minutes from commit to production) | < 30 min | Weekly |
| KPI-ENG-006 | Code Review Coverage | (PRs reviewed by >= 2 engineers / total PRs) x 100 | >= 95% | Weekly |
| KPI-ENG-007 | Test Coverage | (lines covered by automated tests / total lines) x 100 | >= 85% | Monthly |
| KPI-ENG-008 | Infrastructure Cost per Request | Total infra cost / total API requests | Confidential | Monthly |
| KPI-ENG-009 | Feature Flag Rollback Rate | (feature flags rolled back / total deployed) x 100 | < 3% | Weekly |
| KPI-ENG-010 | Technical Debt Ratio | (cost to fix technical debt / cost to rebuild) x 100 | < 25% | Quarterly |

### 3.6 Marketing Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-MKT-001 | Customer Acquisition Cost (CAC) | Total marketing spend / new customers acquired | Confidential | Monthly |
| KPI-MKT-002 | Return on Ad Spend (ROAS) | Revenue from ads / total ad spend | >= 4.5x | Weekly |
| KPI-MKT-003 | Organic Traffic Growth (MoM) | ((organic visits_current - organic visits_prior) / visits_prior) x 100 | >= 10% | Monthly |
| KPI-MKT-004 | Social Media Engagement Rate | (engagements / impressions) x 100 | >= 4.5% | Weekly |
| KPI-MKT-005 | Content Marketing ROI | (revenue from content marketing - cost) / cost | >= 5.0x | Monthly |
| KPI-MKT-006 | Email Campaign Conversion Rate | (conversions from email / emails delivered) x 100 | >= 3.5% | Weekly |
| KPI-MKT-007 | SEO Ranking (Target Keywords) | Average position for top 100 keywords | <= 3 | Weekly |
| KPI-MKT-008 | Brand Sentiment Score | Net positive mentions on social media | >= 75% | Weekly |
| KPI-MKT-009 | Referral Rate | (users acquired via referrals / total new users) x 100 | >= 20% | Monthly |
| KPI-MKT-010 | Community Growth Rate | COUNT(new community members) per month | >= 5,000/mo | Monthly |

### 3.7 Sales Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-SAL-001 | Lead-to-Paid Conversion Rate | (paid conversions / qualified leads) x 100 | >= 12% | Weekly |
| KPI-SAL-002 | Pipeline Value | SUM(weighted deal values in pipeline) | Confidential | Weekly |
| KPI-SAL-003 | Deal Velocity | MEDIAN(days from lead creation to first payment) | < 14 days | Weekly |
| KPI-SAL-004 | Upsell Rate | (existing users who upgraded / total existing users) x 100 | >= 8% | Monthly |
| KPI-SAL-005 | Free Trial Conversion Rate | (trial users who convert to paid / total trial users) x 100 | >= 20% | Weekly |
| KPI-SAL-006 | Madrasa (Institutional) Sales Cycle | MEDIAN(days from first contact to signed contract) | < 60 days | Monthly |
| KPI-SAL-007 | Average Deal Size (Madrasa) | MEAN(contract value for institutional accounts) | Confidential | Monthly |
| KPI-SAL-008 | Sales Qualified Lead (SQL) per Rep | COUNT(SQLs generated per sale agent per week) | >= 20/week | Weekly |

### 3.8 Support Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-SUP-001 | First Response Time (FRT) | MEDIAN(seconds from ticket creation to first response) | < 60 seconds | Real-time |
| KPI-SUP-002 | Average Resolution Time (ART) | MEDIAN(minutes from ticket creation to resolution) | < 30 minutes | Daily |
| KPI-SUP-003 | Customer Satisfaction (CSAT) | (% satisfied responses on post-ticket survey) | >= 95% | Daily |
| KPI-SUP-004 | First Contact Resolution (FCR) | (tickets resolved on first contact / total tickets) x 100 | >= 80% | Daily |
| KPI-SUP-005 | Ticket Volume Trend | ((tickets_current - tickets_prior) / tickets_prior) x 100 | Monitor only | Daily |
| KPI-SUP-006 | Escalation Rate | (tickets escalated to L2 / total tickets) x 100 | < 10% | Daily |
| KPI-SUP-007 | Self-Service Resolution Rate | (issues resolved via knowledge base / total issues) x 100 | >= 40% | Monthly |
| KPI-SUP-008 | Agent Utilization Rate (Support) | (active ticket time / logged-in time) x 100 | >= 80% | Daily |
| KPI-SUP-009 | Ticket Reopen Rate | (reopened tickets / total resolved tickets) x 100 | < 5% | Weekly |
| KPI-SUP-010 | Islamic Content Query Handling Time | Special handling time for Islamic knowledge queries | < 5 min | Daily |

### 3.9 Analytics Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-ANL-001 | Report Generation Time | MEDIAN(seconds from request to completed report) | < 30 seconds | Daily |
| KPI-ANL-002 | Insight Accuracy Rate | (confirmed insights / total insights generated) x 100 | >= 95% | Monthly |
| KPI-ANL-003 | Dashboard Uptime | ((total time - downtime) / total time) x 100 | 99.95% | Monthly |
| KPI-ANL-004 | Data Freshness (Pipeline Latency) | MEDIAN(time from event to available in analytics) | < 5 minutes | Real-time |
| KPI-ANL-005 | Ad-Hoc Query Response Time | MEDIAN(seconds for ad-hoc analytics query) | < 10 seconds | Weekly |
| KPI-ANL-006 | Data Accuracy (Completeness) | (complete records / total expected records) x 100 | >= 99.9% | Daily |
| KPI-ANL-007 | Anomaly Detection Precision | (true anomalies / total anomalies flagged) x 100 | >= 90% | Weekly |
| KPI-ANL-008 | Predictive Model Accuracy (MAPE) | Mean Absolute Percentage Error of forecasts | < 10% | Monthly |

### 3.10 HR / AgentOps Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-HR-001 | Agent Productivity Score | Composite: tasks/hr x quality x uptime | >= 82/100 | Daily |
| KPI-HR-002 | Agent Health Score | Composite: error rate, confidence, resource health | >= 85/100 | Real-time |
| KPI-HR-003 | Agent Training Completion Rate | (agents completing training / total agents assigned) x 100 | 100% | Weekly |
| KPI-HR-004 | Agent Retention Rate (Monthly) | (agents not decommissioned / total agents) x 100 | >= 98% | Monthly |
| KPI-HR-005 | Agent Ramp-Up Time | MEDIAN(hours from deployment to target productivity) | < 24 hours | Weekly |
| KPI-HR-006 | Skill Gap Coverage | (required skills covered by active agents / total required) x 100 | >= 95% | Monthly |
| KPI-HR-007 | Agent Rotation Compliance | (% agents rotated as per rotation schedule) | 100% | Weekly |
| KPI-HR-008 | Human-in-the-Loop Ratio | (human interventions / total agent tasks) x 100 | < 5% | Daily |
| KPI-HR-009 | Agent Lifecycle Cost | MEAN(cost to train, deploy, maintain one agent per month) | Confidential | Monthly |
| KPI-HR-010 | Performance Improvement Rate | ((this_period_score - prior_period_score) / prior_period_score) x 100 | >= 5% improvement | Monthly |

### 3.11 Finance Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-FIN-001 | Budget Variance (Monthly) | ((actual spend - budgeted spend) / budgeted spend) x 100 | < +/- 5% | Monthly |
| KPI-FIN-002 | Cost Per Agent per Month | Total operations cost / total active agents | Confidential | Monthly |
| KPI-FIN-003 | ROI per Department | (dept revenue attributed - dept cost) / dept cost | >= 2.0x per dept | Quarterly |
| KPI-FIN-004 | Burn Rate | Monthly net cash outflow | Within runway | Monthly |
| KPI-FIN-005 | Runway (Months) | Current cash / monthly burn rate | >= 24 months | Monthly |
| KPI-FIN-006 | Revenue per Agent | Total monthly revenue / total active agents | Confidential | Monthly |
| KPI-FIN-007 | Infrastructure Cost Ratio | (infrastructure cost / total revenue) x 100 | < 15% | Monthly |
| KPI-FIN-008 | Zakat Liability Accuracy | Variance from calculated zakat due | < 0.1% | Quarterly |
| KPI-FIN-009 | Gross Margin | ((revenue - COGS) / revenue) x 100 | >= 80% | Monthly |
| KPI-FIN-010 | ARPU Growth Rate (YoY) | ((ARPU_current - ARPU_prior_yr) / ARPU_prior_yr) x 100 | >= 10% | Yearly |

### 3.12 Security Department

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-SEC-001 | Security Incident Count | COUNT(confirmed security incidents) | 0 per quarter | Quarterly |
| KPI-SEC-002 | Mean Time to Detect (MTTD) | MEDIAN(minutes from incident start to detection) | < 5 minutes | Real-time |
| KPI-SEC-003 | Mean Time to Respond (MTTR-SEC) | MEDIAN(minutes from detection to containment) | < 15 minutes | Real-time |
| KPI-SEC-004 | Vulnerability Patch Time | MEDIAN(hours from disclosure to patch deployment) | Critical: < 4h, High: < 24h, Med: < 7d | Weekly |
| KPI-SEC-005 | Vulnerability Scan Coverage | (% of systems scanned weekly) | 100% | Weekly |
| KPI-SEC-006 | Access Control Compliance Rate | (% of access reviews completed on time) | 100% | Monthly |
| KPI-SEC-007 | Phishing Simulation Pass Rate | (% agents not fooled by phishing simulation) | >= 98% | Quarterly |
| KPI-SEC-008 | Encryption Coverage | (% of data encrypted at rest and in transit) | 100% | Monthly |
| KPI-SEC-009 | Third-Party Security Score | Average security rating of all third-party integrations | >= 850/1000 | Quarterly |
| KPI-SEC-010 | Data Privacy Compliance | GDPR/CCPA/comparable regulation compliance status | Full compliance | Quarterly |

### 3.13 All Other Departments (Summary)

Each of the 33 departments has its own set of approximately 12-15 KPIs following the same rigorous structure. Below is a listing of the remaining departments and the categories of KPIs they track:

| Department | KPI Categories | Key Unique KPIs |
|------------|---------------|-----------------|
| Product Management | Feature adoption, product velocity, roadmap completion, user research cadence | Feature adoption rate (>= 60% at 4 weeks), PRD completion rate (100%), A/B test velocity (>= 5/mo) |
| User Research | Research studies completed, insight quality, participant diversity | Studies/mo (>= 8), insight implementation rate (>= 60%), participant diversity index |
| Design/UX | Task success rate, time-on-task, accessibility compliance, design system usage | Task success rate (>= 90%), SUS >= 75, design system adoption (>= 95%) |
| QA/Testing | Test case coverage, defect detection rate, regression test pass rate | Test automation rate (>= 85%), escaped defect rate (< 0.1%), regression suite runtime (< 30 min) |
| Data Engineering | Pipeline reliability, data quality, processing throughput, storage efficiency | Pipeline SLA attainment (>= 99.9%), data latency (P99 < 5 min), storage cost optimization |
| AI/ML Engineering | Model accuracy, training throughput, inference latency, model drift detection | Model accuracy per domain, training pipeline utilization (>= 85%), drift detection recall (>= 95%) |
| DevOps/SRE | Deployment automation %, self-healing rate, capacity planning accuracy | Self-healing rate (>= 95%), capacity forecast accuracy (>= 90%), deployment automation (100%) |
| OCR/Digitization | Pages/hr, OCR accuracy (character level), layout preservation, script recognition rate | Character error rate (< 0.5%), layout preservation score (>= 95%), multi-script accuracy >= 98% |
| Podcast Production | Episodes/hr, audio quality (SNR), speaker diarization accuracy, transcript accuracy | SNR >= 25dB, speaker identification accuracy (>= 98%), transcript accuracy (>= 99%) |
| Video Production | Videos/hr, render quality, caption accuracy, thumbnail CTR | Render throughput, auto-caption accuracy (>= 99%), thumbnail A/B CTR (+15% over baseline) |
| Gamification | Feature adoption, engagement lift, learning outcome correlation | Feature adoption (>= 50% of users), session length lift (+30%), knowledge retention lift (+20%) |
| Social Media | Posting cadence, engagement rate, sentiment analysis, community health | Posting consistency (>= 4/day), engagement rate (>= 4.5%), sentiment score (>= 75% positive) |
| Email/Notification | Delivery rate, open rate, CTR, unsubscribe rate | Delivery rate (>= 98%), open rate (>= 25%), CTR (>= 4%), unsubscribe (< 0.2%/mo) |
| Localization | Translation throughput, translation quality, coverage by language | BLEU score (>= 85), cultural adaptation score (>= 8.5/10), language coverage (>= 20 languages) |
| Legal/Compliance | Contract turnaround, regulatory filing timeliness, IP protection actions | Contract cycle time (< 5 days), regulatory compliance (100%), IP infringement takedowns (< 48h) |
| Strategic Partnerships | Partner acquisition, partner satisfaction, co-marketing ROI | New partners/mo (>= 5), partner NPS (>= 60), co-marketing ROI (>= 3.0x) |
| Data Privacy | PII detection rate, data deletion SLA, consent management compliance | PII detection accuracy (>= 99.99%), deletion SLA (< 72h), consent tracking coverage (100%) |
| Community Management | Response rate, community growth, harmful content detection, event participation | Response time (< 30 min), harmful content recall (>= 99%), event attendance growth (+20% qoq) |
| Research (Islamic) | Fatwa accuracy, scholarly output, publication count, peer review turnaround | Research publication rate, ijtihad quality score, peer review cycle (< 14 days) |
| Arabic Linguistics | Grammar verification accuracy, sarf/nahw analysis accuracy, dialect coverage | I'rab (grammatical analysis) accuracy (>= 98%), dialect expansion rate |

---

## 4. Team-Level KPIs

### 4.1 Knowledge Graph (KG) Team

The KG Team manages the Islamic Knowledge Graph — a semantic network of entities (people, places, concepts, events) and their relationships.

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-KG-001 | Nodes Created per Hour | (new entities added) / team hours | >= 100 nodes/hr | Hourly |
| KPI-KG-002 | Edge Accuracy | (correctly linked relationships / total relationships) x 100 | >= 99.0% | Daily |
| KPI-KG-003 | Query Response Time (P50) | PERCENTILE(KG query time, 0.50) | < 50ms | Real-time |
| KPI-KG-004 | Completeness Score | (entities with full attribute coverage / total entities) x 100 | >= 85% | Weekly |
| KPI-KG-005 | Relationship Density | AVG(edges per node) | >= 3.5 edges/node | Weekly |
| KPI-KG-006 | Entity Resolution Accuracy | (correct entity merges / total resolution ops) x 100 | >= 99.5% | Daily |
| KPI-KG-007 | Schema Compliance Rate | (entities adhering to ontology / total entities) x 100 | >= 98% | Weekly |
| KPI-KG-008 | Sanad (Chain of Narration) Accuracy | Accuracy of hadith transmission chain linkage in KG | >= 99.5% | Daily |

### 4.2 Story Generation Team

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-SG-001 | Stories Generated per Hour | (stories completed / team hours) | >= 15 stories/hr | Hourly |
| KPI-SG-002 | First-Pass Yield (Stories) | (stories passing 1st review / total submitted) x 100 | >= 75% | Daily |
| KPI-SG-003 | Readability Score | Flesch-Kincaid Grade Level appropriate for target audience | Target grade +/- 1.5 | Daily |
| KPI-SG-004 | Story Engagement Score | Predicted user engagement (model-based) | >= 7.5/10 | Weekly |
| KPI-SG-005 | Islamic Authenticity Score (Stories) | Events, characters, rulings checked against authentic sources | >= 9.0/10 | Daily |
| KPI-SG-006 | Story Length Compliance | Stories within +/- 10% of target word count | >= 95% compliance | Daily |
| KPI-SG-007 | Age-Appropriate Content Match | Content suitability for target age group | >= 95% | Daily |
| KPI-SG-008 | Story Diversity | % of stories covering under-represented Islamic topics/regions | >= 30% | Weekly |

### 4.3 Quiz Generation Team

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-QZ-001 | Quizzes Generated per Hour | (quizzes completed / team hours) | >= 20 quizzes/hr | Hourly |
| KPI-QZ-002 | Question Accuracy | (accurate questions / total questions) x 100 | >= 99.0% | Daily |
| KPI-QZ-003 | Difficulty Calibration Accuracy | (questions matching target difficulty / total) x 100 | >= 85% | Weekly |
| KPI-QZ-004 | User Score Correlation | Correlation between quiz scores and external assessment | R-squared >= 0.75 | Monthly |
| KPI-QZ-005 | Question Distractor Quality | (plausible distractors / total distractors) x 100 | >= 90% | Weekly |
| KPI-QZ-006 | Bloom's Taxonomy Coverage | Distribution of questions across Bloom's levels | Even spread +/- 20% | Monthly |
| KPI-QZ-007 | Quiz Completion Rate | (quizzes started / quizzes completed) by users | >= 75% | Weekly |
| KPI-QZ-008 | Re-quiz Accuracy Improvement | AVG(score improvement on retake) | >= 15% improvement | Monthly |

### 4.4 Flashcard Team

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-FC-001 | Flashcards Generated per Hour | (flashcards completed / team hours) | >= 100 cards/hr | Hourly |
| KPI-FC-002 | Spaced Repetition Recall Rate | (% of cards correctly recalled on scheduled repetition) | >= 85% | Weekly |
| KPI-FC-003 | User Completion Rate (Decks) | (decks fully reviewed / decks started) x 100 | >= 65% | Weekly |
| KPI-FC-004 | Card Accuracy | (accurate cards / total cards generated) x 100 | >= 99.5% | Daily |
| KPI-FC-005 | Cloze Deletion Quality | Quality score for cloze (fill-in-blank) deletions | >= 8.0/10 | Weekly |
| KPI-FC-006 | Image Association Accuracy | (correct image-text associations / total) x 100 | >= 98% | Daily |
| KPI-FC-007 | Card Format Consistency | (cards matching template / total cards) x 100 | >= 99% | Daily |
| KPI-FC-008 | Retrieval Practice Efficiency | Info-theoretic measure of bits retained per card | Maximize | Monthly |

### 4.5 Podcast Team

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-PC-001 | Podcasts Produced per Hour | (completed episodes / production hours) | >= 2 episodes/hr | Hourly |
| KPI-PC-002 | Audio Quality Score (PESQ) | Perceptual Evaluation of Speech Quality | >= 4.0 / 5.0 | Daily |
| KPI-PC-003 | Listener Retention Rate (Episode) | (% of episode listened / total duration) | >= 65% | Weekly |
| KPI-PC-004 | Download Rate | (downloads / unique listeners per episode) | >= 1.8 dl/listener | Weekly |
| KPI-PC-005 | Narration Naturalness Score | MOS (Mean Opinion Score) of TTS or human narration | >= 4.2 / 5.0 | Weekly |
| KPI-PC-006 | Content Accuracy (Podcast) | Errors per episode (Islamic content) | < 0.5 errors/ep | Daily |
| KPI-PC-007 | Episode Pacing Score | Variance in speaking rate, pause timing, emphasis | Optimal range | Weekly |
| KPI-PC-008 | Transcript Synchronization Accuracy | (aligned words / total words in transcript-audio sync) | >= 99.0% | Daily |

### 4.6 OCR/Digitization Team

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-OCR-001 | Pages Processed per Hour | (pages scanned + OCR'd / team hours) | >= 300 pages/hr | Hourly |
| KPI-OCR-002 | Character-Level OCR Accuracy | (correct characters / total characters) x 100 | >= 99.5% | Daily |
| KPI-OCR-003 | Layout Preservation Score | (layout elements correctly preserved / total) x 100 | >= 95% | Daily |
| KPI-OCR-004 | Verification Pass Rate (OCR) | (pages passing auto-verification / total) x 100 | >= 92% | Daily |
| KPI-OCR-005 | Script Recognition Accuracy | Accuracy for Arabic script variants (Naskh, Kufic, Diwani, etc.) | >= 98% | Weekly |
| KPI-OCR-006 | Diacritic Accuracy | Accuracy of tashkeel (diacritical marks) | >= 97% | Daily |
| KPI-OCR-007 | Manuscript Damage Handling | (successfully processed damaged pages / total damaged) x 100 | >= 80% | Weekly |
| KPI-OCR-008 | Multi-Column Layout Accuracy | (correctly parsed multi-column layouts / total) x 100 | >= 90% | Weekly |

### 4.7 Gamification Team

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-GM-001 | Feature Adoption Rate | (users who tried gamification / total active users) x 100 | >= 60% | Weekly |
| KPI-GM-002 | Session Length Increase | ((avg session with gamification - without) / without) x 100 | >= 25% increase | Weekly |
| KPI-GM-003 | Return Rate (Gamified vs Non) | (gamified user D7 return / non-gamified D7 return) | >= 1.3x | Weekly |
| KPI-GM-004 | Points/Badges System Engagement | (users earning >= 1 badge / total gamification users) x 100 | >= 80% | Weekly |
| KPI-GM-005 | Leaderboard Health Score | (unique leaderboard participants / total eligible) x 100 | >= 40% | Weekly |
| KPI-GM-006 | Streak Continuation Rate | (users extending streak / users with active streak) x 100 | >= 55% | Daily |
| KPI-GM-007 | Islamic Values Alignment Score | Gamification mechanics vetted for Islamic appropriateness | >= 9.5/10 | Monthly |
| KPI-GM-008 | Learning Outcome Correlation | Correlation between gamification engagement and quiz scores | R-squared >= 0.4 | Monthly |

### 4.8 Social Media Team

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-SM-001 | Posts Created per Hour | (posts completed / team hours) | >= 8 posts/hr | Hourly |
| KPI-SM-002 | Engagement Rate | (likes + shares + comments / impressions) x 100 | >= 4.5% | Daily |
| KPI-SM-003 | Content Moderation Accuracy | (correctly moderated content / total content) x 100 | >= 99.5% | Real-time |
| KPI-SM-004 | Platform Distribution Score | Evenness of content distribution across platforms | >= 0.8 (Gini) | Weekly |
| KPI-SM-005 | Viral Coefficient | (shares per post x conversion rate per share) | >= 0.15 | Weekly |
| KPI-SM-006 | Comment Response Rate | (% of user comments receiving response) | >= 90% | Daily |
| KPI-SM-007 | Hashtag Relevance Score | Relevance of auto-generated hashtags to content | >= 8.0/10 | Daily |
| KPI-SM-008 | Post Scheduling Accuracy | (posts published on schedule / total posts) x 100 | >= 98% | Daily |

### 4.9 Mobile Apps Team

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-MOB-001 | Crash-Free Rate | (sessions without crash / total sessions) x 100 | >= 99.8% | Real-time |
| KPI-MOB-002 | App Store Rating | AVG(app store rating across iOS and Android) | >= 4.7 / 5.0 | Weekly |
| KPI-MOB-003 | Feature Adoption Rate (App) | (users using new feature / total app users) x 100 | >= 50% at 4 wks | Weekly |
| KPI-MOB-004 | App Launch Time (Cold Start) | MEDIAN(ms from tap to interactive) | < 1500ms | Real-time |
| KPI-MOB-005 | ANR Rate (Android) | (ANR occurrences / total sessions) x 100 | < 0.1% | Real-time |
| KPI-MOB-006 | Memory Usage (P95) | PERCENTILE(memory MB, 0.95) across all devices | < 200 MB | Real-time |
| KPI-MOB-007 | Push Notification CTR | (users who tapped / users who received) x 100 | >= 8% | Daily |
| KPI-MOB-008 | Offline Mode Reliability | (offline sessions without error / total offline sessions) x 100 | >= 99% | Weekly |

### 4.10 Search Team

| KPI ID | KPI Name | Formula | Target | Frequency |
|--------|----------|---------|--------|-----------|
| KPI-SRC-001 | Query Latency (P50) | PERCENTILE(search latency, 0.50) | < 100ms | Real-time |
| KPI-SRC-002 | Query Latency (P99) | PERCENTILE(search latency, 0.99) | < 500ms | Real-time |
| KPI-SRC-003 | NDCG@10 (Normalized Discounted Cumulative Gain) | Standard IR metric for result relevance | >= 0.85 | Weekly |
| KPI-SRC-004 | Zero-Result Rate | (queries returning 0 results / total queries) x 100 | < 2% | Daily |
| KPI-SRC-005 | Click-Through Rate (Position 1) | (CTR for first search result) | >= 40% | Daily |
| KPI-SRC-006 | Search Abandonment Rate | (queries with no click / total queries) x 100 | < 15% | Daily |
| KPI-SRC-007 | Arabic Query Understanding Accuracy | Accuracy of Arabic morphological analysis in search | >= 97% | Weekly |
| KPI-SRC-008 | Quranic Search Accuracy | (correct surah:ayah matches / total Quran searches) x 100 | >= 99.5% | Daily |
| KPI-SRC-009 | Hadith Search Recall | (relevant hadith returned / total relevant hadith in corpus) | >= 0.92 | Weekly |
| KPI-SRC-010 | Faceted Filter Usage Rate | (% of searches using filters) | >= 25% | Weekly |

### 4.11 Team-Level KPI Categories for All 130 Teams

The above represents 10 of 130 teams. Every team follows the same rigorous KPI structure with exactly 8-10 KPIs per team. Key categories for remaining teams include:

| Team Type | Number of Teams | KPI Categories |
|-----------|----------------|----------------|
| KG Entity Extraction | 8 | Extraction accuracy, entity disambiguation, attribute completeness, aliasing coverage |
| KG Relationship Mapping | 6 | Relationship extraction accuracy, relationship type diversity, confidence scoring |
| Text Generation (Tafsir) | 5 | Tafsir generation accuracy, source attribution, depth of explanation |
| Text Generation (Fiqh) | 5 | Ruling accuracy, madhab coverage (Hanafi, Maliki, Shafi'i, Hanbali), evidence citation |
| Text Generation (Seerah) | 4 | Chronological accuracy, event verification, narrative engagement |
| Audio Recitation (Quran) | 3 | Tajweed rule accuracy, recitation quality (MQI), surah coverage |
| Video Lecture Production | 4 | Visual quality, lecture structure, scholar representation accuracy |
| Assessment Design | 4 | Question validity, reliability coefficient (Cronbach's alpha), fairness analysis |
| User Personalization | 5 | Recommendation accuracy, diversity of recommendations, exploration rate |
| A/B Testing | 3 | Test velocity, statistical significance rate, deployment rate of winners |
| Data Pipeline Engineering | 6 | Pipeline latency, data completeness, schema compliance |
| Monitoring/Alerting | 4 | Alert accuracy, false alarm rate, coverage completeness |
| Anomaly Detection | 3 | Detection precision, recall, time-to-detection |
| Forecasting | 3 | Forecast accuracy (MAPE), bias, directional accuracy |
| Translation (Arabic to English) | 4 | BLEU score, semantic preservation, idiom handling |
| Translation (Arabic to Urdu) | 3 | BLEU score, script conversion accuracy |
| Translation (Arabic to French) | 3 | BLEU score, cultural adaptation |
| Translation (Arabic to Other) | 6 | Per-language BLEU targets, coverage expansion rate |
| Content Moderation | 4 | Moderation accuracy, false positive rate, response time |
| Copy Editing (Arabic) | 3 | Grammar accuracy, style consistency, readability |
| Copy Editing (English) | 3 | Grammar accuracy, Islamic terminology consistency |
| UI Component Generation | 5 | Component correctness, accessibility compliance, styling compliance |
| API Development | 6 | API spec compliance, error handling coverage, versioning discipline |
| Database Administration | 4 | Query performance, backup success rate, replication lag |
| Infrastructure Automation | 5 | IaC coverage, drift detection, self-healing rate |
| Cost Optimization | 3 | Waste reduction, reserved instance utilization, spot instance usage |
| Agent Health Monitoring | 4 | Health check coverage, anomaly detection rate, false alarm rate |
| Agent Training Pipeline | 5 | Training throughput, model convergence rate, data quality |
| Agent Orchestration | 4 | Orchestration efficiency, task allocation optimality, conflict resolution |
| Prompt Engineering | 6 | Prompt effectiveness score, iteration velocity, regression prevention |
| Model Evaluation | 4 | Eval coverage, eval accuracy, human correlation score |
| Ethics/Governance | 3 | Ethical compliance rate, bias detection coverage, fairness metrics |
| Community Moderation | 3 | Response time, harmful content recall, user dispute resolution |
| User Onboarding | 3 | Onboarding completion rate, time-to-value, activation rate |
| Feedback Analysis | 3 | Feedback categorization accuracy, sentiment analysis accuracy, trend detection |

---

## 5. Agent-Level KPIs

### 5.1 Productivity KPIs

Every AI agent, regardless of role type, has productivity KPIs measured in real-time.

| KPI ID | KPI Name | Formula | Refresh | Target Range |
|--------|----------|---------|---------|--------------|
| KPI-AG-PR-01 | Tasks Completed per Hour | COUNT(tasks completed in last hour) | Real-time | Varies by role |
| KPI-AG-PR-02 | Active Time Ratio | (active processing time / total available time) x 100 | Real-time | 85-92% |
| KPI-AG-PR-03 | Idle Time (Minutes per Hour) | MINUTES(idle, not processing, not waiting) | Real-time | < 5 min/hr |
| KPI-AG-PR-04 | Task Acceptance Rate | (tasks accepted / tasks assigned) x 100 | Hourly | >= 95% |
| KPI-AG-PR-05 | Task Completion Rate (Started) | (tasks completed / tasks started) x 100 | Hourly | >= 98% |
| KPI-AG-PR-06 | Multi-Tasking Efficiency | Tasks processed concurrently without quality degradation | Real-time | <= 3 concurrent tasks |
| KPI-AG-PR-07 | Context Switch Cost | Time lost per context switch event | Real-time | < 2 sec/switch |

**Productivity Targets by Agent Role Type:**

| Role Type | Number of Agents | Tasks/Hour Target | Active Time | Model Family |
|-----------|-----------------|-------------------|-------------|--------------|
| Cognitive (Research, Analysis) | 1,500 | 25-35 | 88% | GPT-4o, Claude 3.5, Gemini Ultra |
| Generation (Content Creation) | 2,000 | 40-60 | 90% | GPT-4o, Claude 3 Opus |
| Verification (Quality Check) | 1,200 | 50-80 | 85% | Specialized verification models |
| Extraction (KG, OCR, Data) | 1,000 | 150-300 | 92% | Fine-tuned Llama 3, Mistral |
| Service (Support, Moderation) | 800 | 30-50 | 85% | GPT-4o, Claude 3 Haiku |
| Engineering (Code, Infrastructure) | 600 | 8-15 | 82% | GPT-4o, Code Llama |
| Creative (Design, Production) | 400 | 10-20 | 80% | DALL-E 3, Midjourney, Eleven Labs |
| Analytical (Data, Forecasting) | 500 | 20-40 | 88% | Specialized analytical models |
| Coordination (Orchestration, Routing) | 500 | 100-200 | 95% | Lightweight fast models |
| **Total** | **8,500** | — | — | — |

### 5.2 Quality KPIs

| KPI ID | KPI Name | Formula | Refresh | Target |
|--------|----------|---------|---------|--------|
| KPI-AG-QL-01 | Output Quality Score | Model-evaluated quality on 0-100 scale | Per task | >= 85/100 |
| KPI-AG-QL-02 | Error Rate | (tasks with errors detected / total tasks) x 100 | Hourly | < 2% |
| KPI-AG-QL-03 | Rework Rate | (tasks requiring rework / total tasks) x 100 | Hourly | < 5% |
| KPI-AG-QL-04 | Consistency Score | Variance in quality over rolling window of 50 tasks | Daily | sigma < 5 points |
| KPI-AG-QL-05 | Hallucination Rate (Factual) | (factual errors / total claims made) x 100 | Per task | < 0.1% |
| KPI-AG-QL-06 | Style Compliance Score | Adherence to style guide (where applicable) | Per task | >= 95% |
| KPI-AG-QL-07 | Output Completeness Score | % of required output fields completed | Per task | 100% |
| KPI-AG-QL-08 | Islamic Content Caution Score | Appropriate caution scores for sensitive theological topics | Per task | >= 9.5/10 |

### 5.3 Efficiency KPIs

| KPI ID | KPI Name | Formula | Refresh | Target |
|--------|----------|---------|---------|--------|
| KPI-AG-EF-01 | Tokens Consumed per Task | Total input + output tokens / tasks completed | Per task | Varies by role |
| KPI-AG-EF-02 | API Calls per Task | Total model API calls / tasks completed | Per task | <= 3 calls/task |
| KPI-AG-EF-03 | Time per Task (P50) | MEDIAN(seconds from task start to finish) | Hourly | Varies by type |
| KPI-AG-EF-04 | Token Wastage Rate | (wasted tokens due to truncation/retry / total tokens) x 100 | Hourly | < 5% |
| KPI-AG-EF-05 | Cache Hit Rate (Agent) | (cached responses used / total responses) x 100 | Hourly | >= 40% |
| KPI-AG-EF-06 | Cost per Task | (total model cost for agent) / tasks completed | Hourly | Varies by role |
| KPI-AG-EF-07 | Energy Efficiency | Tasks per kWh of compute | Daily | Maximize |

**Efficiency Targets by Role Type:**

| Role Type | Tokens/Task Target | Time/Task (P50) | Cost/Task Target |
|-----------|-------------------|-----------------|------------------|
| Cognitive | <= 4,000 | < 120 sec | $0.08 |
| Generation | <= 8,000 | < 90 sec | $0.15 |
| Verification | <= 2,000 | < 45 sec | $0.04 |
| Extraction | <= 1,500 | < 15 sec | $0.02 |
| Service | <= 1,000 | < 30 sec | $0.01 |
| Engineering | <= 6,000 | < 300 sec | $0.12 |
| Creative | <= 10,000 | < 600 sec | $0.25 |
| Analytical | <= 5,000 | < 180 sec | $0.10 |
| Coordination | <= 800 | < 5 sec | $0.005 |

### 5.4 Collaboration KPIs

| KPI ID | KPI Name | Formula | Refresh | Target |
|--------|----------|---------|---------|--------|
| KPI-AG-CL-01 | Handover Success Rate | (successful task handovers / total handovers) x 100 | Hourly | >= 99% |
| KPI-AG-CL-02 | Response Time to Peers | MEDIAN(seconds to respond to peer agent request) | Real-time | < 5 seconds |
| KPI-AG-CL-03 | Escalation Rate | (tasks escalated / total tasks processed) x 100 | Hourly | < 3% |
| KPI-AG-CL-04 | Information Sharing Score | (KB contributions / expected contributions) x 100 | Daily | >= 80% |
| KPI-AG-CL-05 | Dependency Resolution Time | MEDIAN(seconds to resolve blocking dependency for peer) | Real-time | < 10 seconds |
| KPI-AG-CL-06 | Collaborative Task Success | (group tasks completed successfully / total group tasks) x 100 | Daily | >= 90% |
| KPI-AG-CL-07 | Peer Rating Score | Average peer feedback score (1-5) from agent evaluations | Weekly | >= 4.0/5.0 |

### 5.5 Learning KPIs

| KPI ID | KPI Name | Formula | Refresh | Target |
|--------|----------|---------|---------|--------|
| KPI-AG-LN-01 | Knowledge Base Contributions | COUNT(articles, corrections, additions submitted to KB) | Daily | >= 3/day |
| KPI-AG-LN-02 | Retraining Improvement Rate | ((post-training score - pre-training score) / pre-training) x 100 | Per training cycle | >= 5% improvement |
| KPI-AG-LN-03 | Feedback Incorporation Rate | (feedback items applied / feedback items received) x 100 | Weekly | >= 90% |
| KPI-AG-LN-04 | Self-Correction Rate | (errors caught and corrected by agent / total errors) x 100 | Daily | >= 80% |
| KPI-AG-LN-05 | Skill Acquisition Velocity | MEAN(time to reach proficiency in new skill) | Per skill | < 12 hours |
| KPI-AG-LN-06 | Learning Transfer Score | Performance improvement on related-but-different tasks | Per cycle | >= 3% |

### 5.6 Compliance KPIs

| KPI ID | KPI Name | Formula | Refresh | Target |
|--------|----------|---------|---------|--------|
| KPI-AG-CP-01 | Rule Adherence Rate | (tasks following all rules / total tasks) x 100 | Real-time | >= 99.9% |
| KPI-AG-CP-02 | Permission Violation Count | COUNT(accesses to unauthorized data/tools) | Real-time | 0 |
| KPI-AG-CP-03 | Ethical Guideline Compliance | Ethical review pass rate for agent outputs | Per task | 100% |
| KPI-AG-CP-04 | Data Privacy Compliance | (tasks respecting privacy rules / total tasks) x 100 | Hourly | 100% |
| KPI-AG-CP-05 | Escalation Policy Adherence | (tasks escalated per policy / total escalation-needed tasks) x 100 | Hourly | 100% |
| KPI-AG-CP-06 | Audit Trail Completeness | (tasks with complete audit log / total tasks) x 100 | Hourly | 100% |

### 5.7 Health KPIs

| KPI ID | KPI Name | Formula | Refresh | Target Range |
|--------|----------|---------|---------|--------------|
| KPI-AG-HL-01 | Confidence Score Average | MEAN(agent's self-reported confidence on outputs) | Per task | 0.75 - 0.95 |
| KPI-AG-HL-02 | Anomaly Detection Flags | COUNT(statistical anomalies in agent behavior) | Real-time | 0 per day |
| KPI-AG-HL-03 | GPU/CPU Utilization | (compute resource utilized / allocated) x 100 | Real-time | 60-85% |
| KPI-AG-HL-04 | Memory Utilization | (memory used / memory allocated) x 100 | Real-time | 50-80% |
| KPI-AG-HL-05 | Response Latency Stability | Std dev of response latency over 100 tasks | Real-time | sigma < 15% of mean |
| KPI-AG-HL-06 | Model Degradation Signal | Difference in quality score vs. baseline model | Per batch | < 0.05 decrease |
| KPI-AG-HL-07 | Drift Detection Score | Distribution shift in agent's output embeddings | Hourly | < 0.1 KL divergence |
| KPI-AG-HL-08 | Queue Depth (Work Waiting) | COUNT(tasks waiting in agent's input queue) | Real-time | < 10 |

### 5.8 Individual Targets by Role Type

| Role Type | Primary KPIs | Critical Thresholds |
|-----------|-------------|-------------------|
| Cognitive Agent | Tasks/hr >= 25, Quality >= 88, Error Rate < 1.5%, Hallucination < 0.05% | Auto-pause if hallucination > 0.5% in any hour |
| Generation Agent | Tasks/hr >= 40, First-pass yield >= 78%, Cost/task < $0.18 | Auto-stop if cost/task > $0.50 for 10 consecutive tasks |
| Verification Agent | Tasks/hr >= 50, Accuracy >= 99.9%, False positive < 0.05% | Escalate to human if accuracy dips below 99.5% |
| Extraction Agent | Tasks/hr >= 150, Accuracy >= 99.0%, Token wastage < 3% | Throttle if error rate > 2% in any 15-min window |
| Service Agent | Tasks/hr >= 35, CSAT >= 4.5/5, Response time < 10s | Flag if CSAT < 4.0 for any shift |
| Engineering Agent | Tasks/hr >= 8, Test coverage >= 85%, Build success >= 95% | Block commits if test coverage drops below 80% |
| Creative Agent | Tasks/hr >= 12, Quality >= 85, Style compliance >= 95% | Review if style compliance < 90% on any day |
| Analytical Agent | Tasks/hr >= 25, Accuracy >= 97%, Report generation time < 30s | Escalate if forecast MAPE > 15% |
| Coordination Agent | Tasks/hr >= 100, Handover success >= 99.5%, Queue depth < 5 | Auto-scale if queue depth exceeds 10 for > 1 min |

---

## 6. KPI Calculation Methods

### 6.1 Formula Definitions

This section provides the precise mathematical formula for every major KPI category.

#### 6.1.1 Rate-Based KPIs (Common Pattern)

**Standard Rate Formula:**
Rate = (COUNT(events_meeting_criterion) / COUNT(total_eligible_events)) x 100

**Examples:**
| KPI | Numerator | Denominator | Notes |
|-----|-----------|-------------|-------|
| Accuracy Rate | Correctly verified items | Total items verified | — |
| Pass Rate | Items passing review | Total items submitted | — |
| Conversion Rate | Conversions (e.g., sign-ups, purchases) | Total visitors/leads | — |
| Completion Rate | Completed actions | Total started actions | — |
| Retention Rate | Users returning at Day N | Users who installed on Day 0 | — |

#### 6.1.2 Throughput-Based KPIs

Throughput = COUNT(items_completed) / SUM(active_hours)

Active Hours Calculation: active_hours = total_elapsed_hours - idle_hours - downtime_hours - training_hours

#### 6.1.3 Financial KPIs

**MRR:** MRR = SUM(all active subscriptions) = SUM(base_price x quantity_per_customer)
**ARPU:** ARPU = MRR / total_active_users
**LTV:** LTV = ARPU x (1 / monthly_churn_rate)
**LTV/CAC Ratio:** LTV/CAC = LTV / (total_marketing_and_sales_cost / new_customers_acquired)
**CAC:** CAC = (total_marketing_spend + total_sales_spend) / new_customers_acquired

#### 6.1.4 Quality Score Composites

**Content Quality Score (Composite):**
Quality_Score = (1 - error_rate) x 0.40 + consistency_score x 0.30 + user_feedback_score x 0.30

Where:
- error_rate = errors_detected / total_items_checked
- consistency_score = 1 - (sigma of quality ratings across reviewer samples / max_possible_sigma)
- user_feedback_score = AVG(user_rating) / max_possible_rating

**Agent Quality Score (Composite):**
Agent_Quality = output_accuracy x 0.35 + style_compliance x 0.20 + completeness x 0.20 + peer_rating x 0.25

**Islamic Authenticity Score (Composite):**
Authenticity = source_authenticity x 0.30 + citation_accuracy x 0.30 + scholarly_consensus x 0.25 + language_preservation x 0.15

#### 6.1.5 Time-Based KPIs

**Percentile Calculation (P50, P95, P99):**
P_n = value at position ceil(n/100 x N) in sorted ascending array of values
Where N = number of data points in the window.

**MTTR (Mean Time to Recover):**
MTTR = SUM(time_from_detection_to_resolution for all incidents) / COUNT(incidents)

**MTBF (Mean Time Between Failures):**
MTBF = total_uptime_hours / COUNT(incidents)

**FRT (First Response Time):**
FRT = MEDIAN(ticket_creation_timestamp - first_response_timestamp)
Using median (not mean) to avoid skew from extreme outliers.

#### 6.1.6 User Engagement KPIs

**DAU/MAU Ratio (Stickiness):**
DAU_MAU = DAU / MAU

**NPS (Net Promoter Score):**
NPS = (%_Promoters - %_Detractors) x 100
Where Promoters = responses 9-10, Passives = 7-8, Detractors = 0-6 (on 0-10 scale)

**CSAT (Customer Satisfaction Score):**
CSAT = (COUNT(responses >= 4 on 1-5 scale) / COUNT(total responses)) x 100

**User Engagement Score (Composite):**
Engagement = session_depth_norm x 0.25 + session_frequency_norm x 0.25 + feature_breadth_norm x 0.20 + content_consumption_norm x 0.20 + social_interaction_norm x 0.10

### 6.2 Rolling Window Calculations

All time-series KPIs use rolling windows to balance responsiveness with stability.

| Window | Use Case | Data Points | Priority |
|--------|----------|-------------|----------|
| 1 hour | Operational monitoring, auto-scaling | ~3,600 (per-second metrics) | Real-time |
| 24 hours (1d) | Daily standup reporting | ~86,400 data points | High |
| 7 days (7d) | Weekly reviews, trend analysis | ~604,800 data points | Medium |
| 30 days (30d) | Monthly reviews, target tracking | ~2,592,000 data points | Low |
| 90 days (90d) | Quarterly reviews, strategic planning | ~7,776,000 data points | Background |

Rolling value implementation: Rolling_Value(window_W, time_T) = CALCULATE(KPI_Formula OVER last_W_time_period)

### 6.3 Weighted Averages

**Weighted Composite Score:**
Weighted_Score = SUM(weight_i x normalized_score_i) / SUM(weight_i)

**Example: Department Performance Score:**
Dept_Performance = throughput_weight (0.25) x throughput_norm + quality_weight (0.35) x quality_norm + efficiency_weight (0.20) x efficiency_norm + collaboration_weight (0.20) x collaboration_norm

### 6.4 Normalization Techniques

#### 6.4.1 Min-Max Normalization (Score to 0-100)
Normalized = ((actual_value - min_threshold) / (target - min_threshold)) x 100

Used for: composite scores, dashboard visualization, cross-KPI comparison.
- If actual < min_threshold: Normalized = 0 (or negative, capped at 0)
- If actual > target: Normalized > 100 (uncapped, but highlighted)

**Example:** Content Accuracy Rate:
- Min threshold: 98.0%
- Target: 99.5%
- Actual: 99.3%
- Normalized = ((99.3 - 98.0) / (99.5 - 98.0)) x 100 = (1.3 / 1.5) x 100 = 86.7 / 100

#### 6.4.2 Z-Score Normalization
Z_Score = (actual_value - population_mean) / population_std_dev

Used for: anomaly detection, peer comparison, outlier identification.
- |Z| > 2: potential anomaly (~95% confidence)
- |Z| > 3: significant anomaly (~99.7% confidence)

#### 6.4.3 Percentile Normalization
Percentile_Rank = (COUNT(values_below_current) / COUNT(total_values)) x 100

### 6.5 Seasonality Adjustments

| Season | Duration | Effect on KPIs | Adjustment Factor |
|--------|----------|----------------|-------------------|
| Ramadan (Month of Fasting) | 29-30 days | DAU +40%, engagement +30%, content consumption +50%, support +60% | Targets x 1.3-1.5x |
| Shawwal (Eid al-Fitr) | 3 days | DAU -20%, new signups +80%, social engagement +100% | 3-day anomaly adjustment |
| Dhul-Hijjah (Hajj + Eid al-Adha) | 10 days | Quran content +80%, pilgrimage +200%, DAU +30% | Targets x 1.2-2.0x |
| Muharram (Ashura) | 2 days | Historical content +50%, DAU +10% | 1.1x |
| Rabi' al-Awwal (Mawlid) | 1 day | Seerah content +100%, DAU +20% | 1.2x |
| School Terms (Northern) | Sep-Jun | Student users +60%, quiz content +40% | 1.2-1.4x |
| School Holidays (Northern) | Jul-Aug | Student users -40%, adult users +20% | 0.8x |

**Seasonality Adjustment Algorithm:**
Adjusted_Target = Base_Target x Seasonality_Factor
Seasonality_Factor = 1.0 + SUM(seasonal_effect_coefficient_i x active_during_season_i)

### 6.6 Real-Time vs Batch Calculations

| Type | KPIs | Engine | Latency |
|------|------|--------|---------|
| Real-time Stream Processing | Agent task completion, latency, error rate, uptime, throughput | Apache Flink, Kafka Streams | < 1 second |
| Micro-batch (1-5 min) | Hourly productivity, response times, active counts | Spark Structured Streaming | 1-5 minutes |
| Batch (Hourly) | Quality scores, daily content counts, verification throughput | Apache Spark | 1-10 minutes |
| Batch (Daily) | Retention rates, NPS, financial metrics, rolling windows | Apache Spark + DBT | 10-30 minutes |
| Batch (Weekly/Monthly) | Strategic KPIs, benchmarks, forecasts | DBT + Spark + Airflow | 1-4 hours |

---

## 7. KPI Thresholds and Alerting

### 7.1 Green/Yellow/Red Threshold System

Every KPI uses a three-zone threshold system with zone boundaries specific to the KPI type and criticality.

**General Threshold Structure (Higher is Better):**
- Green Zone: 0% to 80% of target (on track, no action needed)
- Yellow Zone: 80% to 95% of target (at risk, monitor closely)
- Red Zone: 95%+ of target (critical, immediate action required)

**Inverted Thresholds (Lower is Better):**
- Green Zone: Actual <= 80% of target (e.g., error rate <= 0.4% when target is 0.5%)
- Yellow Zone: Actual at 80-95% of target (0.4-0.475%)
- Red Zone: Actual > 95% of target (> 0.475%)

#### 7.1.3 Company-Level Threshold Examples

| KPI | Target | Green | Yellow | Red |
|-----|--------|-------|--------|-----|
| Content Accuracy Rate | >= 99.5% | >= 99.1% | 99.1-98.525% | < 98.525% |
| Error Rate | < 0.5% | < 0.4% | 0.4-0.475% | > 0.475% |
| Platform Uptime | >= 99.99% | >= 99.992% | 99.99-99.992% | < 99.99% |
| P95 Latency | < 500ms | < 400ms | 400-475ms | > 475ms |
| User Satisfaction | >= 4.5/5 | >= 4.1/5 | 3.9-4.1/5 | < 3.9/5 |
| MRR Growth (MoM) | >= 5% | >= 4% | 3.25-4% | < 3.25% |

#### 7.1.4 Agent-Level Threshold Examples

| KPI | Target | Green | Yellow | Red |
|-----|--------|-------|--------|-----|
| Tasks/hr (Generation) | >= 45/hr | >= 36/hr | 31.5-36/hr | < 31.5/hr |
| Error Rate | < 2% | < 1.6% | 1.6-1.9% | > 1.9% |
| Handover Success | >= 99% | >= 79.2% | 69.3-79.2% | < 69.3% |
| Hallucination Rate | < 0.1% | < 0.08% | 0.08-0.095% | > 0.095% |

### 7.2 Alerting Rules and Escalation

#### 7.2.1 Alert Levels

| Level | Condition | Notification | Response SLA |
|-------|-----------|--------------|--------------|
| P0 — Critical | Red KPI > 5 min | Slack P0 + SMS + Email + Phone call to dept manager + exec | < 2 min ack, < 15 min action |
| P1 — High | Any Red KPI triggered | Slack DM to dept manager + team lead | < 5 min ack |
| P2 — Medium | Yellow KPI > 1 hour | Slack to team lead + department channel | < 30 min ack |
| P3 — Low | Yellow KPI triggered (first 60 min) | Dashboard indicator only | No response required |
| P4 — Info | Green-to-Yellow transitions | Logged in alert history | No action needed |

#### 7.2.2 Escalation Path

Agent KPI Red (>5 min) -> Agent auto-paused + Team Lead notified
  -> (15 min unresolved) Team Lead investigates -> (30 min) Department Manager notified
  -> (2 hrs) Executive notified -> (4 hrs) CE-AI notified

#### 7.2.3 Automated Corrective Actions at Red Thresholds

| KPI | Automated Action at Red | Auto-Revert Condition |
|-----|------------------------|----------------------|
| Agent Error Rate > 1.9% | Agent auto-paused, tasks redistributed | Agent retrained, test pass >= 90% |
| Content Accuracy < 98.5% | Content generation pipeline paused for affected domain | Pipeline verified, root cause fixed |
| Platform Uptime < 99.99% | Traffic auto-routed to DR region | Primary region healthy for 15 min |
| P99 Latency > 1000ms | Auto-scale triggered, non-critical traffic throttled | P99 < 500ms for 5 min |
| Error Budget > 80% consumed | Deployments frozen (no production changes) | New month (budget reset) or manual override |
| Security Incident >= 1 | Auto-isolate affected systems, block access | Security team manual review |
| Agent Hallucination > 0.5% | Agent output blocked, reverted to fallback model | Investigation complete |
| Budget Variance > +/- 5% | All non-essential spend frozen | Finance Director override |

#### 7.2.4 Burn Rate Alerts for Error Budget

For a 99.99% uptime target, the monthly error budget is:
Monthly Error Budget = (1 - 0.9999) x 30 days x 24 hours x 60 minutes = 4.38 minutes

| Error Budget Consumed | Level | Action |
|----------------------|-------|--------|
| < 50% (<= 2.19 min) | Green | Continue normal operations |
| 50-70% (2.19-3.07 min) | Yellow | Review recent changes, slow deploys |
| 70-90% (3.07-3.94 min) | Orange | Feature freeze, only critical deploys |
| 90-100% (3.94-4.38 min) | Red | All changes frozen, war room convened |
| > 100% (> 4.38 min) | Black | SLO breach, postmortem, CE-AI notified |

### 7.3 Alert Fatigue Management

1. **Alert Deduplication:** Identical alerts from same source within 60 min grouped
2. **Alert Correlation:** Related alerts collapsed into single incident
3. **Maintenance Windows:** Alerts suppressed during scheduled maintenance
4. **Seasonal Adjustment:** Thresholds adjust during high-traffic periods
5. **Auto-Suppression:** Red KPI with open ticket suppresses further alerts
6. **Graduated Response:** Only persistent (>5 min) or severe reds notify humans
7. **Weekly Alert Review:** Frequency and false positives reviewed every Monday

**Target Metrics for Alerting System:**
- Alert Precision (true positives / total alerts): >= 95%
- Alert Recall (true positives / total incidents): >= 99%
- Mean Time to Acknowledge (MTTA) P0: < 2 min
- False Alarm Rate: < 3%
- Alert Volume (per agent per day): < 3

---

## 8. KPI Dashboards and Reporting

### 8.1 Real-Time Dashboards

#### 8.1.1 Company Overview Dashboard (CE-AI & Executives)

A wall-monitor dashboard showing at-a-glance enterprise health with auto-refresh every 10 seconds.

**Layout sections:**
- **Platform Health:** Uptime, P50/P95/P99 latency, status dot
- **Content Pipeline:** Throughput, fresh content today, trend
- **User Engagement:** DAU, MAU, DAU/MAU, Retention D7, NPS
- **Financial:** MRR, ARPU, Churn rate
- **Quality:** Accuracy, User Sat, Error Rate
- **Operations:** Agent Health, Utilization, Training Compliance, Security
- **Summary Bar:** Count of Red/Yellow/Green/No Data KPIs
- **Drill-down links, Full Report, Alert History, Configure**

#### 8.1.2 Department Drill-Down Dashboard

For department managers to monitor all team and process KPIs. Shows a table of teams with their KPI values, vs target, vs prior period, status, and 24-hour sparklines. Summary bar at bottom showing count of Green/Yellow/Red.

#### 8.1.3 Team-Level Dashboard

For team leads: team KPI scorecards, agent leaderboard (ranked by composite score), work queue, alert feed, workload distribution histogram, top 5 automatically identified issues.

#### 8.1.4 Agent Individual Dashboard

For each of 8,500 agents: profile card (ID, role, model, version, status), 6 circular gauges for primary KPIs, 24-hour performance timeline with threshold zones, recent tasks log, active alerts, resource consumption (GPU, memory, tokens), peer percentile comparison.

### 8.2 Scheduled Reports

| Report | Frequency | Recipients | Format |
|--------|-----------|------------|--------|
| Team Standup Report | Hourly | Team leads, dept managers | Slack + dashboard link |
| Daily Department Brief | Daily 07:00 | Dept managers, team leads | Email HTML + Slack PDF |
| Executive Daily Summary | Daily 08:30 | Executives, CE-AI | Slack + 1-page PDF |
| Weekly Department Review | Monday 09:00 | Dept managers, exec sponsor | Email + dashboard link |
| Weekly Executive Review | Monday 14:00 | All execs, CE-AI | Email PDF + slides |
| Monthly Business Review | Day 5 | Execs, CE-AI, Board | Board deck PPTX/PDF |
| Quarterly Strategy Review | Day 10 | CE-AI, Board, Investors | Board deck + appendix |
| Annual Report | Jan 31 | All stakeholders, public | PDF + web microsite |

### 8.3 Report Entry Format

Every KPI in every report includes: KPI Name, Current Value, Trend Arrow (up/down/stable/flat/record), vs Target (%), vs Prior Period (absolute), Forecast, Status (Green/Yellow/Red/No Data).

### 8.4 Anomaly Detection Methods

| Method | Description | Used For |
|--------|-------------|----------|
| Z-Score Outlier | Flag |Z| > 3 | Agent individual task scores |
| Moving Average Deviation | Flag when value > 3 sigma from 7d MA | Department throughput, quality |
| Seasonal Decomposition (STL) | Flag residual > 3 sigma | DAU, content consumption |
| CUSUM | Detect small persistent mean shifts | Error rate, latency, conversion |
| Isolation Forest | Multi-dimensional anomaly detection | Agent health |
| Changepoint Detection (PELT) | Identify abrupt time-series changes | Engagement, revenue |

### 8.5 Benchmarking

| Benchmark | Our Target | Industry Avg | Top Quartile |
|-----------|-----------|--------------|--------------|
| Platform Uptime | 99.99% | 99.95% | 99.99% |
| P95 API Latency | < 500ms | < 800ms | < 300ms |
| Content Accuracy Rate | 99.5% | 97% | 99% |
| D7 Retention (EdTech) | 45% | 30% | 42% |
| NPS (EdTech) | 50+ | 35 | 50+ |
| Mobile Crash Rate | < 0.2% | < 0.5% | < 0.1% |
| Content Throughput | 120/hr | ~20/hr (human) | ~50/hr (AI-assisted) |

---

## 9. KPI Review Process

### 9.1 Daily Standup Reviews

**When:** Every day at 09:00 local time
**Duration:** 15 minutes
**Participants:** Team lead + all team agents (via health summary)
**Format:** Virtual standup via dashboard + AI-facilitated discussion

**Agenda (15 min total):**
1. KPI Check (3 min): AI displays 3 most important metrics + Yellow/Red statuses
2. Yesterday's Results (3 min): Auto-generated summary from task logs
3. Today's Priorities (3 min): Critical tasks, targets, known issues
4. Blockers (3 min): Each agent's primary blocker, auto-escalated
5. Action Items (3 min): Assign owners, confirm deadlines

### 9.2 Weekly Department Review

**When:** Every Monday, 10:00-11:00
**Duration:** 60 minutes
**Participants:** Department manager, all team leads, cross-functional liaison

**Agenda:**
1. Department KPI Review (15 min): Each of 15 department KPIs, focus on Yellow/Red
2. Team Spotlight (15 min): Best, most improved, most challenged teams
3. Root Cause Analysis (10 min): 5-Whys for Red KPIs
4. Cross-Team Coordination (10 min): Dependency conflicts, handover optimization
5. Action Items and Follow-ups (10 min): Assign owners, update targets

### 9.3 Monthly Executive Review

**When:** First Tuesday of each month, 13:00-15:00
**Duration:** 120 minutes
**Participants:** All 7 executives, CE-AI, selected department managers

**Agenda:**
1. Company Performance Review (30 min): All 25 company KPIs
2. Department Deep-Dive (30 min): 2-3 departments presented (rotating)
3. Strategic Initiative Tracking (20 min): OKR progress, major projects
4. KPI Target Adjustment (15 min): Target changes up to +/- 10%
5. Forecast and Planning (15 min): Year-end forecast, resource requirements
6. Open Discussion (10 min): Strategy, cross-functional issues

### 9.4 Quarterly Strategy Review

**When:** Within 10 days of quarter end
**Duration:** 240 minutes (full day)
**Participants:** CE-AI, Executives, Board members, external advisors
**Format:** In-person or virtual board meeting

**Agenda:**
1. Quarterly Performance Report (60 min): KPIs vs annual targets, financials
2. Strategic Assessment (60 min): Market position, competition, sector trends
3. KPI Framework Review (30 min): Leading vs lagging correlation, new KPI proposals
4. Resource Allocation (30 min): Budget, agent capacity, infrastructure investment
5. Objectives Setting (60 min): Quarterly OKRs, annual target adjustments

### 9.5 KPI Adjustment Protocol

| Role | Can Propose Changes To | Approval Needed |
|------|----------------------|-----------------|
| Any Agent (via Health System) | Its own agent-level KPI targets | Team Lead |
| Team Lead | Team-level KPIs (up to +/- 10%) | Department Manager |
| Team Lead | Agent-level KPIs for their team | Self (with notification) |
| Department Manager | Department-level KPIs (up to +/- 10%) | Executive |
| Department Manager | Team-level KPIs (up to +/- 15%) | Self (logged) |
| Executive | Company-level KPIs (up to +/- 10%) | CE-AI |
| Executive | Department-level KPIs (up to +/- 20%) | Self (notify CE-AI) |
| CE-AI | Any KPI, any amount | Board (for > +/- 10% company KPIs) |
| Board | Company-level strategic KPIs | Board vote |

**Change Frequency Limits:**
- Company-level: 1 quarter min interval, +/- 15% max change
- Department-level: 1 month, +/- 15%
- Team-level: 2 weeks, +/- 20%
- Agent-level: 1 week, +/- 25%

**KPI Change Log:** Every adjustment logged with: change_id, timestamp, kpi_id, previous_target, new_target, reason, proposed_by, approved_by, effective_date, reviewed_at.

---

## 10. KPI Cascading and Alignment

### 10.1 Full Cascade Example: Content Quality

**Level 1 — Company:**
KPI-COMP-028: Overall Content Accuracy Rate >= 99.5%

**Level 2 — Departments:**
- Islamic Verification: Verification Accuracy Rate >= 99.9%, False Positive < 0.05%, False Negative < 0.05%
- Content Generation: First-Pass Quality Pass Rate >= 82%, Originality < 15% similarity
- QA/Testing: Escaped Defect Rate < 0.1%, Regression Pass Rate >= 99.5%

**Level 3 — Teams:**
- Islamic Verification — Hadith Team: Hadith Authentication Accuracy >= 99.5%
- Islamic Verification — Fiqh Team: Fiqh Ruling Correctness >= 99.0%
- Content Generation — Story Team: First-Pass Yield >= 75%
- Content Generation — Quiz Team: Question Accuracy >= 99.0%

**Level 4 — Agents:**
- Verification Agent (Hadith): Individual Accuracy >= 99.9%, Error Rate < 0.1%, Rework < 2%
- Generation Agent (Story): Output Quality >= 88/100, Hallucination < 0.1%, Style Compliance >= 95%

### 10.2 Second Cascade: User Retention

**Level 1 — Company:** D30 Retention Rate >= 30%

**Level 2 — Departments:** Product Management (Feature Adoption >= 60%), AI Teacher (Resolution >= 92%), Content (Fresh >= 500/day), Marketing (Email Conversion >= 3.5%)

**Level 3 — Teams:** Onboarding (Step Completion >= 90%), Personalization (Recommendation Accuracy >= 85%), Gamification (Adoption >= 60%)

**Level 4 — Agents:** Onboarding Agent (Guidance Completion >= 95%), Recommendation Agent (CTR >= 40%), Gamification Agent (Badge Relevance >= 90%)

### 10.3 OKR Alignment

Each OKR maps to specific KPIs:

**Objective: "Deliver the World's Most Authentic Islamic Learning Experience"**
- KR1: 99.7% accuracy -> KPI: Content Accuracy Rate >= 99.7%
- KR2: NPS 65+ -> KPI: NPS >= 65
- KR3: 500,000 items -> KPI: Total Content Items >= 500,000

**Objective: "Build a Self-Sustaining Education Platform"**
- KR1: $X MRR, 5% MoM -> KPI: MRR Growth >= 5%
- KR2: Churn < 4% -> KPI: Monthly Churn < 4%
- KR3: 3.5x LTV/CAC -> KPI: LTV/CAC >= 3.5x

### 10.4 Cross-Department Dependencies

| Dependent KPI | Input Dept A | Input Dept B | Input Dept C |
|--------------|-------------|-------------|-------------|
| Content Accuracy | Content Generation | Islamic Verification | QA/Testing |
| D30 Retention | Product Management | AI Teacher | Content |
| Platform Uptime | Engineering | DevOps/SRE | Security |
| NPS | Support | Product | AI Teacher |
| Content Throughput | Content Acquisition | Content Generation | Islamic Verification |

### 10.5 Cascading Formula

Department_KPI_Target = Company_KPI_Target x Dept_Contribution_Weight x Dept_Capability_Factor
Team_KPI_Target = Department_KPI_Target x Team_Contribution_Weight x Team_Capability_Factor
Agent_KPI_Target = Team_KPI_Target / Number_of_Agents x Agent_Capability_Factor

---

## 11. KPI Edge Cases

### 11.1 Data Unavailability

| Scenario | Fallback Strategy | Dashboard Indicator |
|----------|------------------|-------------------|
| Event stream lag > 30s | Use last known good value | "Stale" + timestamp |
| Data source down > 5 min | Use predicted value from forecast model | "Estimated" + confidence interval |
| Partial data (60% arrived) | Scale up by inverse of completeness | "Partial" + completeness % |
| All sources down > 15 min | Mark as "No Data", trigger P1 alert | "No Data" + alert |

### 11.2 Division by Zero

| KPI | Zero Denominator Scenario | Default |
|-----|--------------------------|---------|
| Conversion Rate | 0 visitors | NULL, "Insufficient Data" |
| Throughput | 0 active hours | 0 |
| Error Rate | 0 tasks | NULL, "No Tasks" |
| Retention Rate | 0 users in cohort | NULL, "Empty Cohort" |
| Percentage Change | 0 prior period | NULL, show absolute change |
| User Satisfaction | 0 survey responses | NULL, "No Responses" |

### 11.3 Stale Data

| Refresh Frequency | Stale After | Very Stale After | Expired After |
|-----------------|-------------|-----------------|------------|
| Real-time (< 1s) | 30 seconds | 2 minutes | 5 minutes |
| Hourly | 65 minutes | 3 hours | 6 hours |
| Daily | 25 hours | 3 days | 5 days |
| Weekly | 8 days | 14 days | 30 days |
| Monthly | 35 days | 60 days | 90 days |

### 11.4 Conflicting KPIs

| Conflicting Pair | Why They Conflict | Mitigation |
|-----------------|-------------------|------------|
| Throughput vs Accuracy | Speed reduces quality | Composite includes both; accuracy red overrides throughput |
| Cost vs Quality | Cheap models = lower quality | Minimum quality floor enforced |
| Utilization vs Idle | Over-utilization causes burnout | Target range 80-90% with error rate penalty |
| Fresh Rate vs Verification | More content overwhelms verification | 1:1.5 verifier:generator ratio |
| Response Time vs Accuracy | Rushing reduces quality | Response target only when accuracy >= 95% |
| Revenue vs Quality | Monetization pressure | Revenue gated: if quality < 99%, bonuses reduced |

### 11.5 Anti-Gaming Safeguards

1. **Random Audits:** 5% of tasks randomly selected for deep audit
2. **Blind Evaluation Sets:** KPIs based on held-out test data agents never see
3. **Cross-Validation:** Key metrics validated from multiple independent sources
4. **Statistical Process Control:** Automated monitoring for distribution shifts
5. **Human-in-the-Loop:** Random samples reviewed by human QA
6. **KPI Rotation:** 20% of KPIs reviewed and potentially replaced every 90 days
7. **Adversarial Evaluation:** Dedicated "Red Team" agent system detects gaming

### 11.6 Missing Data Imputation

| Gap | Imputation Method | Confidence Adjustment |
|-----|-------------------|----------------------|
| Isolated point (1 of 100) | Linear interpolation | None |
| Short gap (< 1 hr) | Carry forward last value | Reduced by 10% |
| Medium gap (1-24 hr) | Same day from previous week, seasonally adjusted | Reduced by 25% |
| Long gap (> 24 hr) | Rolling average of last 7 days | Reduced by 50% |

---

## 12. KPI System Architecture

### 12.1 Data Collection

**Event-Based Architecture (No Polling):**
- All KPI data is collected via event-driven architecture
- Agents emit events on task start, task complete, error, handover, etc.
- System components emit events on deployment, incident, scaling, etc.
- User actions emit events on signup, session, content consumption, etc.
- Event schema: { event_id, event_type, source_id, timestamp, payload, version }

**Event Types by Source:**
| Source | Event Examples | Volume (per day) |
|--------|---------------|-----------------|
| Agent Task Events | task_started, task_completed, task_failed, task_escalated | 50 million |
| Agent Health Events | health_check, anomaly_detected, model_degraded | 10 million |
| User Events | session_start, session_end, content_viewed, quiz_taken | 100 million |
| System Events | deployment, scaling, incident, recovery | 100,000 |
| Business Events | subscription_created, payment_received, churn | 1 million |

### 12.2 Storage Architecture

**Tiered Time-Series Storage:**

| Tier | Retention | Storage Engine | Query Priority | Data Volume |
|------|-----------|---------------|---------------|-------------|
| Hot | 30 days | In-memory + Redis + VictoriaMetrics | Real-time queries (< 10ms) | 50 TB |
| Warm | 90 days | ClickHouse + Parquet files | Interactive queries (< 1s) | 150 TB |
| Cold | 7 years | S3/GCS + Parquet (compressed) | Batch queries (< 1 min) | 12 PB |

**Database Components:**
- **VictoriaMetrics:** Real-time metrics storage (agent KPIs, latency, throughput)
- **ClickHouse:** Analytical queries (department/company KPIs, trends, forecasts)
- **Redis:** Cached KPI values for dashboards (hot cache, TTL: 30 seconds)
- **S3/GCS:** Long-term archival with Parquet format, partitioned by date and KPI ID
- **PostgreSQL:** KPI metadata (definitions, targets, thresholds, change log)

### 12.3 Calculation Engine

**Stream Processing (Real-time KPIs):**
- Apache Flink with SQL-based processing
- Sliding windows for rolling calculations (1 min, 5 min, 1 hour)
- Immediate updates on event arrival
- Average throughput: 200,000 events/second per Flink cluster

**Batch Processing (Historical KPIs):**
- Apache Spark scheduled via Apache Airflow (DAGs)
- Hourly, daily, weekly, monthly computation jobs
- Idempotent re-computation for data correction
- Average job runtime: 10 min (daily), 2 hours (monthly)

**KPI Computation Pipeline:**
Event Ingestion (Kafka) -> Stream Processing (Flink, real-time KPIs) -> Hot Storage (Redis+VictoriaMetrics)
                                                                        -> Batch Processing (Spark, historical KPIs) -> Warm Storage (ClickHouse) -> Cold Storage (S3 Parquet)

### 12.4 KPI API

The KPI system exposes a RESTful API for querying KPIs at any level of the hierarchy.

**Endpoints:**

| Method | Endpoint | Description | Parameters |
|--------|----------|-------------|------------|
| GET | /api/v1/kpis | List all KPI definitions | level, department_id, team_id |
| GET | /api/v1/kpis/{kpi_id} | Get single KPI definition + current value | include_history, time_range |
| GET | /api/v1/kpis/{kpi_id}/values | Get KPI time-series values | start, end, interval, aggregation |
| GET | /api/v1/company/kpis | Company-level KPI dashboard | time_range |
| GET | /api/v1/departments/{id}/kpis | Department-level KPIs | time_range, include_teams |
| GET | /api/v1/teams/{id}/kpis | Team-level KPIs | time_range, include_agents |
| GET | /api/v1/agents/{id}/kpis | Agent-level KPIs | time_range |
| GET | /api/v1/alerts | Active alerts | level, status, source_id |
| POST | /api/v1/kpis/{kpi_id}/adjust | Propose KPI adjustment | new_target, reason, proposed_by |

**Query Parameters for All Endpoints:**
- time_range: 1h, 24h, 7d, 30d, 90d, custom (start_ts, end_ts)
- interval: raw, 1m, 5m, 1h, 1d
- aggregation: avg, sum, min, max, p50, p95, p99, count
- filters: status (green/yellow/red), department_id, team_id, agent_id, role_type
- format: json, csv, parquet

### 12.5 Integration with Monitoring, Logging, and Alerting

**Monitoring Integration:**
- KPI data exported as Prometheus metrics for infrastructure monitoring
- Grafana dashboards consume KPI API alongside infrastructure metrics
- Alertmanager receives KPI alerts alongside system alerts
- PagerDuty/OpsGenie for P0/P1 on-call notifications

**Logging Integration:**
- All KPI calculations logged with input parameters and result
- KPI change log (adjustments) stored in PostgreSQL
- Audit log of all KPI API access (who queried what, when)
- Integration with ELK stack for log search and correlation

**Alerting Integration:**
- KPI alerts flow through the centralized Alert Manager
- Alert deduplication, correlation, and suppression at the Alert Manager level
- Escalation policies defined in the Alert Manager (on-call schedules)
- Status page updates automatically when company-level KPIs hit Red

### 12.6 System Resilience and SLAs

| Component | Redundancy | Failover Time | Recovery Point Objective (RPO) |
|-----------|-----------|---------------|-------------------------------|
| Kafka (event stream) | Multi-AZ, replicated | < 30 seconds | 0 (at least once delivery) |
| Flink (stream processing) | Active-passive with checkpointing | < 60 seconds | < 1 second |
| VictoriaMetrics (hot storage) | Multi-node cluster, replication factor 2 | < 10 seconds | < 5 seconds |
| ClickHouse (warm storage) | Multi-shard, replicated | < 30 seconds | < 1 minute |
| PostgreSQL (metadata) | Primary-standby with synchronous replication | < 10 seconds | 0 |
| API Gateway | Load-balanced, multi-AZ | < 5 seconds | N/A (stateless) |

**KPI System SLAs:**
- Availability: 99.99% (4.38 min downtime/month)
- API P99 latency: < 200ms
- Dashboard load time (P95): < 2 seconds
- Alert notification delivery (P0): < 30 seconds
- Data freshness (real-time KPIs): < 5 seconds

---

## Appendix A: KPI ID Naming Convention

| Prefix | Department/Level |
|--------|-----------------|
| KPI-COMP- | Company-level |
| KPI-CA- | Content Acquisition |
| KPI-CG- | Content Generation |
| KPI-IV- | Islamic Verification |
| KPI-AT- | AI Teacher |
| KPI-ENG- | Engineering |
| KPI-MKT- | Marketing |
| KPI-SAL- | Sales |
| KPI-SUP- | Support |
| KPI-ANL- | Analytics |
| KPI-HR- | HR/AgentOps |
| KPI-FIN- | Finance |
| KPI-SEC- | Security |
| KPI-KG- | Knowledge Graph Team |
| KPI-SG- | Story Generation Team |
| KPI-QZ- | Quiz Generation Team |
| KPI-FC- | Flashcard Team |
| KPI-PC- | Podcast Team |
| KPI-OCR- | OCR/Digitization Team |
| KPI-GM- | Gamification Team |
| KPI-SM- | Social Media Team |
| KPI-MOB- | Mobile Apps Team |
| KPI-SRC- | Search Team |
| KPI-AG-PR- | Agent Productivity |
| KPI-AG-QL- | Agent Quality |
| KPI-AG-EF- | Agent Efficiency |
| KPI-AG-CL- | Agent Collaboration |
| KPI-AG-LN- | Agent Learning |
| KPI-AG-CP- | Agent Compliance |
| KPI-AG-HL- | Agent Health |

## Appendix B: KPI System Change Log v1.0

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2026-07-09 | KPI Governance Board | Initial release |
