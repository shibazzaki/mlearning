
# Himalayan Expedition Success Prediction

**Competition:**  
Build a binary classification model that, given expedition metadata (peak height, team size, season, host country, participant statistics, etc.), predicts whether at least one member will summit.

- **Data:**  
  - `exped.csv` — expedition records (dates, routes, team counts, summit attempts, deaths, etc.)  
  - `peaks.csv` — peak characteristics (height, region, permit requirements)  
  - `members.csv` — participant details (year of birth, sex, role-flags, summit bids)  

- **Task:**  
  - Predict `success_flag = 1` if ≥1 summit; else 0.

- **Metrics:**  
  - **Primary:** ROC AUC  
  - **Secondary:** Accuracy, F1-score

- **Approach:**  
  1. **EDA & Cleaning** → evaluate missingness & correlations  
  2. **Baseline:** LogisticRegression (ROC AUC 0.824)  
  3. **Advanced:** RandomForest + RandomizedSearchCV → ROC AUC 0.988, Accuracy 0.95, F1 0.955  
  4. **Feature Engineering:** aggregate member‐level stats (mean age, % female, % summit bids)

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

Install with:

```bash
pip install pandas numpy scikit-learn matplotlib seaborn joblib
````

---

## 🚀 How to Run

1. **Mount Google Drive** (if on Colab) or place the `data/` folder next to the notebook.
2. Adjust file paths in the first cell or rely on `low_memory=False` & `encoding='latin1'`.
3. Run cells in order:

   1. **Environment setup** (imports, warnings suppression)
   2. **Data loading & cleaning**
   3. **Exploratory Data Analysis**
   4. **Feature selection & engineering**
   5. **Baseline modeling** (LogisticRegression)
   6. **Advanced modeling & tuning** (RandomForest + RandomizedSearchCV)
   7. **Aggregate member‐level stats**
   8. **Final model training & evaluation**
   9. **Save** `himalayan_final_rf.pkl` & **export** `submission.csv`

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
3. Engineered member stats (mean\_age, pct\_summiters), plus region/host

---

## ⚠️ Known Issues & Resolutions

1. **UnicodeDecodeError & DtypeWarning**

   * *Cause:* Mixed encodings & mixed‐type columns in CSVs
   * *Fix:* Use `low_memory=False`, `encoding='latin1'`, explicit `dtype={…: str}`, or suppress warnings

2. **Missing data in route/ascent columns**

   * > 80 % null in `route3–4`, `ascent2–4`
   * *Action:* Dropped for baseline; revisit if engineering route features

3. **KeyError in members aggregation**

   * *Cause:* Wrong column names (`role`/`msuccess` assumed vs actual `leader`/`deputy`/`msmtbid`)
   * *Fix:* Updated aggregation to use correct fields

4. **Highly correlated features**

   * `smtdays` vs `totdays` (r≈0.9) → dropped `smtdays` to avoid redundancy

---

## 📝 Next Steps / GitHub Issues

* **\[DATA-CLEANUP]** Finalize `read_csv` calls with correct encoding & dtypes
* **\[EDA]** Review and document any additional outliers or class imbalance
* **\[FEATURE-ENGINEERING]** Explore more member & peak‐level stats (e.g. rope usage, campsites)
* **\[MODEL-TUNING]** Optionally compare with GBM (XGBoost, LightGBM)
* **\[CLEANUP]** Remove debug cells & obsolete imports from notebook
* **\[DOCS]** Polish README and add usage examples
* **\[DEPLOY]** Wrap model in Flask/FastAPI endpoint & Dockerize
* **\[TESTS]** Write pytest tests for pipeline & API

Feel free to open these as issues in your repository and check them off as you go. Good luck! 🚀
