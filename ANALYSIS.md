# GPTZero A/B Test Analysis

## Overview

This analysis evaluates an A/B test comparing two monetization strategies:

- **Control group** (`in_experiment_group_freemium`): Users on a freemium model with limited credits
- **Treatment group** (`in_experiment_group_paid_only`): Users on a paid-only model

**Scope:** Users with `created_at >= 2025-09-01`  
**Conversion metric:** Users who fired the `started_paid_plan` event  
**Data:** 12,289 experiment users (6,400 control / 5,889 treatment), Aug–Nov 2025

---

## Q1 — Pre-experiment Balance Check

**Finding: ⚠ Groups are statistically imbalanced**

| Segment | Test | chi² | p-value | Result |
|---|---|---|---|---|
| `user_type` | Chi-square | 60.6 | < 0.0001 | Imbalanced |
| `hdyhau` | Chi-square | 72.6 | < 0.0001 | Imbalanced |

Notable imbalances:
- Teachers: 23.3% in control vs 17.7% in treatment
- Friend referrals: 48.1% in control vs 41.4% in treatment
- Search: 15.4% in control vs 19.9% in treatment

**Implication:** The two groups are not directly comparable as-is. Conversion results should be interpreted with caution, and segmented or regression-adjusted analysis is recommended.

---

## Q2 — Key Metric: Conversion Rate

**Finding: ✓ Statistically significant lift in treatment**

| Group | Converted | Rate |
|---|---|---|
| Control (freemium) | 255 / 6,400 | 3.98% |
| Treatment (paid-only) | 294 / 5,889 | 4.99% |

- Δ = **+1.01 percentage points**
- z = −2.70, **p = 0.007**

However, this result must be contextualized alongside the severe engagement dropout in the treatment group (see Q — Engagement below).

---

## Q3 — Plan Distribution (free vs. premium)

**Finding: ✗ No significant difference**

| Plan | Control % | Treatment % |
|---|---|---|
| Free | 96.0% | 95.5% |
| Premium | 4.0% | 4.5% |

chi² = 1.89, p = 0.17 — no meaningful difference in current plan snapshot between groups.

---

## Q4 — AI Scan Usage (`ran_ai_scan`)

**Finding: ✓ Highly significant — by design**

| Group | Rate |
|---|---|
| Control | 95.3% |
| Treatment | 46.9% |

Δ = −48.4 pp, p ≈ 0. The paid-only gate restricts scan access, so far fewer treatment users fire this event. This is expected and confirms the experiment is working as designed.

---

## Q5 — Writing Feedback Scan (`ran_writing_feedback_scan`)

**Finding: ✓ Highly significant — by design**

| Group | Rate |
|---|---|
| Control | 14.1% |
| Treatment | 3.2% |

Δ = −10.8 pp, p ≈ 0. Same mechanism as Q4 — restricted access reduces usage.

---

## Q6 — Out-of-Credits Modal (`viewed_out_of_credits_modal`)

**Finding: Control-only friction point**

| Group | Rate |
|---|---|
| Control | 8.8% (566 users) |
| Treatment | 0.0% |

8.8% of freemium users hit the credits paywall. This is a key conversion driver to monitor — these users are most likely candidates to upgrade.

---

## Q7 — Friend Referrals (`successfully_referred_friend`)

**Finding: ✓ Significant — freemium incentive effect**

| Group | Rate |
|---|---|
| Control | 4.4% |
| Treatment | 1.4% |

Δ = −2.9 pp, p ≈ 0. Freemium users refer friends at 3× the rate, likely motivated by earning free credits. Removing freemium significantly weakens the referral flywheel.

---

## Engagement Funnel Analysis

A critical finding beyond the primary metrics: **53% of treatment users never fired any event after being assigned to the experiment**, compared to only 4.7% in control.

| | Control | Treatment |
|---|---|---|
| Total users | 6,400 | 5,889 |
| First event: `ran_ai_scan` | 5,911 (92.4%) | 1,319 (22.4%) |
| First event: `started_free_trial` | 0 (0.0%) | 1,267 (21.5%) |
| First event: `ran_writing_feedback_scan` | 134 (2.1%) | 0 (0.0%) |
| First event: `started_paid_plan` (direct) | 53 (0.8%) | 175 (3.0%) |
| **No events after assignment** | **302 (4.7%)** | **3,128 (53.1%)** |
| Ever `started_paid_plan` | 255 (3.98%) | 294 (4.99%) |

The paid-only gate causes massive early dropout. The treatment group's conversion denominator is inflated by users who never engaged, making the +1pp lift misleading at face value.

### By user_type

| user_type | Control total | Treatment total | Control no-events | Treatment no-events | Control ever-paid | Treatment ever-paid |
|---|---|---|---|---|---|---|
| professional | 1,554 | 1,490 | 4.9% | 54.1% | 4.1% | 3.9% |
| student | 3,355 | 3,359 | 5.2% | 52.0% | 4.4% | 5.2% |
| teacher | 1,491 | 1,040 | 3.6% | 55.2% | 3.0% | 5.8% |

### By hdyhau

| hdyhau | Control total | Treatment total | Control no-events | Treatment no-events | Control ever-paid | Treatment ever-paid |
|---|---|---|---|---|---|---|
| chatgpt | 470 | 509 | 4.9% | 56.0% | 5.3% | 5.9% |
| friend | 3,079 | 2,439 | 4.2% | 54.1% | 3.1% | 5.2% |
| search | 987 | 1,170 | 5.3% | 49.9% | 4.7% | 4.4% |
| social_media | 1,864 | 1,771 | 5.2% | 53.1% | 4.8% | 4.8% |

---

## Summary & Recommendations

| Question | Finding |
|---|---|
| Q1 Balance | ⚠ Randomization failed — groups differ on user_type and hdyhau |
| Q2 Conversion | ✓ Treatment +1pp higher (4.99% vs 3.98%), p = 0.007 |
| Q3 Plan mix | ✗ No difference |
| Q4 AI scans | ✓ Control 2× more (by design) |
| Q5 Writing scans | ✓ Control 4× more (by design) |
| Q6 Credits modal | Control-only: 8.8% hit the paywall |
| Q7 Referrals | ✓ Control refers 3× more (freemium incentive effect) |

**Key concerns before shipping treatment:**

1. **Randomization failure** — user_type and hdyhau are imbalanced. Re-run with proper stratified randomization or adjust for covariates before drawing conclusions.
2. **53% dropout in treatment** — the paid-only gate causes over half of users to immediately churn with zero engagement. The conversion lift may disappear or reverse when computed only over engaged users.
3. **Referral flywheel damage** — removing freemium reduces referrals by 3×, which could hurt top-of-funnel growth significantly.
4. **Segment heterogeneity** — teachers show the largest conversion lift (+2.8pp) while professionals show a slight decline. Social media users show no treatment effect at all.
