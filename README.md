---

```markdown
# Himalayan Expedition Success Prediction

This notebook walks through a full pipeline—from data loading and EDA to model tuning and final deployment artifacts—for predicting whether a Himalayan expedition will summit at least one member.

---

## 📁 Project Structure

```

├── data/
│   ├── exped.csv
│   ├── peaks.csv
│   ├── members.csv
│   ├── refer.csv
│   └── himalayan\_data\_dictionary.csv
├── models/
│   └── himalayan\_final\_rf.pkl
├── submission.csv
├── Himalayan.ipynb
└── README.md

````

---

## 🛠️ Requirements

- Python 3.7+
- pandas  
- numpy  
- scikit-learn  
- matplotlib, seaborn  
- joblib  

You can install via:

```bash
pip install pandas numpy scikit-learn matplotlib seaborn joblib
````

---

## 🚀 How to run

1. **Mount your Google Drive** (if on Colab) or place the `data/` folder next to the notebook.
2. Update the file paths in the very first cell, or rely on the default `low_memory=False` & `encoding='latin1'` parameters to read messy CSVs.
3. Execute cells in order:

   1. **Environment setup** (imports, warnings suppression)
   2. **Data loading & cleaning**
   3. **Exploratory Data Analysis**
   4. **Feature selection & engineering**
   5. **Baseline modeling (LogisticRegression)**
   6. **Advanced modeling & tuning (RandomForest + RandomizedSearchCV)**
   7. **Aggregation of member-level stats**
   8. **Final model training & evaluation**
   9. **Save** `himalayan_final_rf.pkl` and **export** `submission.csv`

---

## 📊 Key Results

| Model                                        |   ROC AUC |  Accuracy |  F1-score |
| :------------------------------------------- | --------: | --------: | --------: |
| Logistic Regression (baseline)               |     0.824 |     0.787 |     0.820 |
| Random Forest (default params)               |     0.974 |     0.919 |     0.926 |
| **Random Forest (tuned + members-features)** | **0.988** | **0.950** | **0.955** |

**Confusion Matrix (final model on test set):**

|                | Pred = 0 | Pred = 1 |
| :------------: | :------: | :------: |
| **Actual = 0** |    964   |    63    |
| **Actual = 1** |    51    |   1207   |

**Top 3 Features by Importance**

1. `highpoint` (≈82 %)
2. `totmembers` (≈5 %)
3. engineered member features (mean age, % attempted summits), plus region/host

---

## ⚠️ Known Issues & Resolutions

1. **UnicodeDecodeError & DtypeWarning**

   * *Cause:* CSVs contained mixed encodings and mixed-type columns.
   * *Fix:* Use `low_memory=False`, `encoding='latin1'` and explicit `dtype={…: str}` or suppress via `warnings.filterwarnings`.

2. **Missing data in route/ascent columns**

   * > 80 % values were null in `route3–4`, `ascent2–4`.
   * *Action:* Dropped these features for baseline; revisit only if you engineer route-specific stats.

3. **KeyError in members aggregation**

   * *Cause:* original code assumed `role` & `msuccess` cols, but actual names were `leader`, `deputy`, `msmtbid`.
   * *Fix:* Adjusted aggregation to use `leader`, `deputy`, `msmtbid`, `sex`, `yob`.

4. **Highly correlated features**

   * `smtdays` vs. `totdays` had r≈0.9 → dropped `smtdays` to avoid redundancy.

