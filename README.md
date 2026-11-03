# FraudGuard AI

Credit card fraud detection web app built with Streamlit, scikit-learn and SQLite.

Run from the project root:

```bash
pip install streamlit pandas numpy scikit-learn plotly joblib werkzeug pytest
streamlit run app/app.py
pytest tests -q
```

## Features

- Register / sign in (hashed passwords, login lock-out)
- Fraud Detection page: pick a model, enter a transaction and analyze it
- Light / dark theme, dashboard, analytics, model comparison, "How it works"
- Prediction History per user with KPI cards, filters, search, CSV export and delete

### Clear fraud verdicts (new)

- **Big verdict banner** on the result: SAFE / SUSPICIOUS / FRAUD with a large icon
- **"What should I do now?"** action line under the banner
- **"Why was this flagged?"**: the top 3 unusual patterns in plain language
  (compared with the normal transactions of the dataset; weighted by feature
  importance when the model provides it). These are *unusual patterns*, not a
  proven cause of fraud
- **Plain-language confidence**: "About 73 out of 100 transactions like this turn out to be fraud."
- **Alert behaviour**: the whole result area is tinted red / orange / green and a
  toast appears once per analysis. The gauge is supporting detail below the reasons

### Review workflow (new)

- Every prediction stores its reasons, a review status
  (`PENDING`, `CONFIRMED_FRAUD`, `FALSE_ALARM`, `APPROVED`) and an optional note (max 300 characters)
- Older databases are migrated automatically (columns are added only when missing, no data is lost)
- Prediction History: alert banner with the number of flagged transactions waiting for review,
  row highlighting, Verdict + Action columns, probability bars, review-status badges,
  quick filter (All / Needs review / Fraud only / Suspicious only / Safe), sorting,
  date and amount filters, "Review this transaction" section, extra KPI cards
  (Needs review, Fraud amount blocked, Fraud rate), optional "last 7 days" chart
- CSV export includes `verdict`, `action`, `reasons`, `review_status` and `note`
- Dashboard: same verdict wording and row tint, plus a notice with the number of
  unreviewed flagged transactions that links to the History page

### Language (new)

All new user-facing text is in `UI_TEXT` in `app/config.py` in two languages:
English (`en`, default) and Roman Urdu (`ur`). Use the **Language** selector in the sidebar.

### Single KNN model, MerchantID and date/time (new)

- The app uses one model only: **KNN** (no model selector).
- Inputs: Amount, Transaction Type, Merchant ID, Location, Transaction date and time.
  Locations are read from `data/creditcard.csv`.
- To (re)train the model on the dataset run `python train_knn.py` from the project root.
  It rewrites `models/knn.joblib` and `models/preprocessor.joblib`.
