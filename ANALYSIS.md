# GPTZero A/B Test Analysis

**Notebook:** [AB test analysis - free token and paid user.ipynb](https://github.com/Amber-Sun-0911/gptzero_ds_assessment_candidate/blob/main/AB%20test%20analysis%20-%20free%20token%20and%20paid%20user.ipynb)

## Experiment Setup

- **Control group** (`in_experiment_group_freemium`): Freemium model with limited credits
- **Treatment group** (`in_experiment_group_paid_only`): Paid-only model
- **Scope:** Users created on or after 2025-09-01
- **Primary conversion metric:** `started_paid_plan` event
- **Sample:** 12,289 users — 6,400 control / 5,889 treatment

---


## TL;DR

1. **Paid-only converts better, but unevenly.** Conversion rate is 4.99% vs. 3.98% in freemium (+1.0 pp, p = 0.007). Teachers and friend-referred users show the strongest uplift.

2. **Paid-only kills early engagement.** 53.1% of paid-only users never fired any event after assignment, vs. 4.7% in freemium. Most users churn before experiencing the product.

3. **20 free tokens may be too generous.** Only 8.8% of freemium users hit the out-of-credits modal, meaning the credit limit creates little conversion pressure for the vast majority.

---

## Next Steps

1. **Find the optimal free token amount.** Run experiments on the freemium group with lower token limits (e.g., 5, 10, 15) to identify the threshold that maximizes conversion.

2. **Test paid-only in high-signal segments.** Pilot the paid-only model for teachers and friend-referred users before broader rollout.

---

## Engagement Funnel

| event | control_n | treatment_n | control_pct | treatment_pct |
|---|---|---|---|---|
| Total users | 6,400 | 5,889 | — | — |
| First event: ran_ai_scan | 5,911 | 1,319 | 92.36% | 22.40% |
| First event: started_free_trial | 0 | 1,267 | 0.00% | 21.51% |
| First event: ran_writing_feedback_scan | 134 | 0 | 2.09% | 0.00% |
| First event: started_paid_plan | 53 | 175 | 0.83% | 2.97% |
| No events after assignment | 302 | 3,128 | 4.72% | 53.12% |
| Ever started_paid_plan | 255 | 294 | 3.98% | 4.99% |
| Ever viewed_out_of_credits_modal | 566 | 0 | 8.84% | 0.00% |

---

## Segmentation by user_type

| user_type | event | control_n | treatment_n | control_pct | treatment_pct |
|---|---|---|---|---|---|
| professional | Total users | 1,554 | 1,490 | — | — |
| | ever_started_paid_plan | 64 | 58 | 4.12% | 3.89% |
| | no_events_after_assignment | 76 | 806 | 4.89% | 54.09% |
| | ran_ai_scan | 1,441 | 332 | 92.73% | 22.28% |
| | started_free_trial | 0 | 311 | 0.00% | 20.87% |
| | started_paid_plan (first) | 13 | 41 | 0.84% | 2.75% |
| student | Total users | 3,355 | 3,359 | — | — |
| | ever_started_paid_plan | 146 | 176 | 4.35% | 5.24% |
| | no_events_after_assignment | 173 | 1,748 | 5.16% | 52.04% |
| | ran_ai_scan | 3,049 | 764 | 90.88% | 22.74% |
| | started_free_trial | 0 | 749 | 0.00% | 22.30% |
| | started_paid_plan (first) | 32 | 98 | 0.95% | 2.92% |
| teacher | Total users | 1,491 | 1,040 | — | — |
| | ever_started_paid_plan | 45 | 60 | 3.02% | 5.77% |
| | no_events_after_assignment | 53 | 574 | 3.55% | 55.19% |
| | ran_ai_scan | 1,421 | 223 | 95.31% | 21.44% |
| | started_free_trial | 0 | 207 | 0.00% | 19.90% |
| | started_paid_plan (first) | 8 | 36 | 0.54% | 3.46% |

Teachers show the strongest conversion lift (+2.75 pp, z = −3.41, **p = 0.0006**).

---

## Segmentation by hdyhau

| hdyhau | event | control_n | treatment_n | control_pct | treatment_pct |
|---|---|---|---|---|---|
| chatgpt | Total users | 470 | 509 | — | — |
| | ever_started_paid_plan | 25 | 30 | 5.32% | 5.89% |
| | no_events_after_assignment | 23 | 285 | 4.89% | 55.99% |
| | ran_ai_scan | 437 | 106 | 92.98% | 20.83% |
| | started_free_trial | 0 | 101 | 0.00% | 19.84% |
| | started_paid_plan (first) | 4 | 17 | 0.85% | 3.34% |
| friend | Total users | 3,079 | 2,439 | — | — |
| | ever_started_paid_plan | 94 | 127 | 3.05% | 5.21% |
| | no_events_after_assignment | 130 | 1,319 | 4.22% | 54.08% |
| | ran_ai_scan | 2,895 | 545 | 94.02% | 22.35% |
| | started_free_trial | 0 | 497 | 0.00% | 20.38% |
| | started_paid_plan (first) | 19 | 78 | 0.62% | 3.20% |
| search | Total users | 987 | 1,170 | — | — |
| | ever_started_paid_plan | 46 | 52 | 4.66% | 4.44% |
| | no_events_after_assignment | 52 | 584 | 5.27% | 49.91% |
| | ran_ai_scan | 911 | 234 | 92.30% | 20.00% |
| | started_free_trial | 0 | 319 | 0.00% | 27.26% |
| | started_paid_plan (first) | 9 | 33 | 0.91% | 2.82% |
| social_media | Total users | 1,864 | 1,771 | — | — |
| | ever_started_paid_plan | 90 | 85 | 4.83% | 4.80% |
| | no_events_after_assignment | 97 | 940 | 5.20% | 53.08% |
| | ran_ai_scan | 1,668 | 434 | 89.48% | 24.51% |
| | started_free_trial | 0 | 350 | 0.00% | 19.76% |
| | started_paid_plan (first) | 21 | 47 | 1.13% | 2.65% |

Friend-referred users show the strongest and most significant conversion lift (+2.16 pp, z = −4.05, **p < 0.0001**). Social media users show virtually no treatment effect (4.83% vs. 4.80%).

---

## Submission Notes

- **Code:** All SQL queries and Python analysis are included in the notebook linked at the top of this document. Code is not fully cleaned but is reproducible.
- **Time spent:** ~2 hours, excluding breaks.
- **Recommendations:** See Next Steps section above.
- **AI assistance:** Used ChatGPT to enhance the TL;DR language and debug SQL syntax errors.
- **Other notes:** No unclear instructions or problems encountered with the provided data.
