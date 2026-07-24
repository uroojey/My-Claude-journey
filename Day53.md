# KEY-LEARNINGS.md

## Day 53 Key Learnings

1. **Missingness can be a signal, not just noise.**
   Instead of defaulting to automatic imputation, evaluating whether a missing value carries meaning (e.g., in risk-related datasets) leads to better-informed decisions per column.

2. **Check cardinality before one-hot encoding.**
   Running `.nunique()` on categorical columns first prevents unexpected column explosion and keeps the dataset manageable.

3. **Data leakage checks are a small step with big downstream impact.**
   Reviewing documentation for any column that wouldn't realistically be known at prediction time avoids building a model on false confidence.

4. **Protecting raw data is a foundational architectural decision.**
   Keeping raw and cleaned datasets in separate folders (rather than overwriting in place) makes the entire pipeline reproducible, auditable, and safer to iterate on.

5. **Plan for cross-platform tooling gaps early.**
   Power BI Desktop's Windows-only limitation meant identifying the Power BI Service as a fallback for Mac users before it became a blocker.

6. **Documentation as you go beats documentation after the fact.**
   Recording the "why" behind each decision in `data_notes.md` immediately, rather than retroactively, made the reasoning clearer and easier to hand off.
