# Disparate Impact Simulation — Future Directions

## Next Up: "Interview Top N" Framing

Replace the fixed score cutoff with a slider that asks "How many people do you want to interview?" The system finds the threshold that admits ~N people, and shows the composition by neighborhood.

### Why this fixes the current issues

The current model has a floor effect: at reasonable cutoffs (50+), Lakeview's interview rate is so low that removing non-proxy factors sends it to 0%, creating confusing behavior where the gap appears to shrink when you turn off a fair factor. The "Interview top N" framing eliminates this because the total interviewed stays constant — the question becomes purely about composition.

### Key behavior students would discover

- Turning off a proxy → gap shrinks (algorithm leans less on unfair factors)
- Turning off a non-proxy → gap grows (algorithm is forced to lean harder on proxies)

Both directions are informative, consistent, and neither hits a floor.

### Implementation plan

1. **Change parameters:**
   - 200 applicants (100 per neighborhood)
   - Rebalance point values: Experience up to 35 pts (3.5 × years), Assessment up to 35 pts (raw score × 0.35), Referral 15 pts, Internship 15 pts
   - Keep proxy correlations at current levels (referral: 40%/10%, internship: 35%/12%)

2. **Change the Algorithm tab slider:**
   - Label: "How many applicants should get interviews?" (range: 20–160)
   - Show the effective cutoff score on the histogram as a red line (computed, not directly set)
   - Summary: "Interviewing X out of 200 applicants (top Y%)"

3. **Change the Investigate tab slider:**
   - Same "How many interviews?" framing
   - When features are toggled, the system recalculates: finds new threshold to maintain ~same N interviews, then shows the new composition
   - Gap meter shows: "Of your N interviews: X go to Greenfield, Y go to Lakeview"

4. **Consider showing composition as a stacked bar** rather than (or in addition to) separate percentage rates. "Of your 60 interviews: [===Greenfield 43===|==Lakeview 17==]" makes the disparity very concrete.

5. **Tested parameters at "Interview ~60":**
   - All factors: 43 G + 21 L (gap 22 pts)
   - Without Referral: 34 G + 28 L (gap 6 pts)
   - Without Internship: 36 G + 28 L (gap 8 pts)
   - Without Experience: 45 G + 14 L (gap 31 pts)

---

## Advanced Version: Selection Effects & Feedback Loops

Build a more advanced simulation that shows how performance data itself can become correlated with neighborhood through selection effects. The setup:

- Students work with a historical dataset where NovaTech has already been using the algorithm for several years
- Past hiring decisions favored Greenfield applicants (due to proxy variables), so the "successful employees" in the training data are disproportionately from Greenfield
- Students set up something like a train/test split — use historical performance data along with the four factors to build a model
- Students select a cutoff that minimizes "bad hires" and maximizes "good ones" based on this historical data
- They then discover that the historical performance data was itself shaped by the selection: because Lakeview applicants were rarely hired in the past, there's little data about how they perform, and the few who were hired may have been exceptional (survivorship bias)
- This creates a feedback loop: biased selection → biased training data → biased model → biased selection

This would help students understand:
1. Why "just use the data" doesn't solve the problem
2. How selection bias in historical data perpetuates disparate impact
3. Why a model can appear to be "accurate" on historical data while being systematically unfair
4. The difference between "the model fits the data" and "the data is representative"

### Implementation Notes
- Could frame as a "Version 2.0" of the hiring algorithm where NovaTech decides to "improve" by training on outcomes
- Needs careful scaffolding — the concept of feedback loops is abstract
- Might work better as a guided walkthrough rather than open sandbox
- Consider whether this should be a separate page or an additional tab
