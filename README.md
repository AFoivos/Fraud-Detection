Πρότζεκτ ανίχνευσης ύποπτων τραπεζικών συναλλαγών με συνδυασμό:
- καθαρισμού/προεπεξεργασίας,
- exploratory data analysis (EDA),
- στατιστικών flags (π.χ. z-score),
- unsupervised ανίχνευσης ανωμαλιών (Isolation Forest),
- supervised μοντελοποίησης (Random Forest).

Το repository είναι οργανωμένο σε 5 notebooks που εκτελούν πλήρες pipeline από raw δεδομένα μέχρι τελικό αρχείο με ανωμαλίες.

---

## 1) Τι περιέχει το dataset

Το αρχικό dataset βρίσκεται στο `data/bank_transactions_data.csv` και περιλαμβάνει **2512 εγγραφές** με βασικά πεδία συναλλαγών (ημερομηνίες, ποσά, merchant, συσκευή, προσπάθειες login, υπόλοιπο λογαριασμού κ.ά.).

Στη συνέχεια δημιουργούνται εμπλουτισμένες εκδόσεις του dataset με νέα χαρακτηριστικά/flags.

---

## 2) Ροή εργασίας (notebook-by-notebook)

### `notebooks/01_data_understanding_cleaning.ipynb`
Βήματα:
1. Φόρτωση αρχικού dataset.
2. Έλεγχος ποιότητας (`shape`, `describe`, `isnull`, `nunique`).
3. Αφαίρεση διπλότυπων.
4. Μετατροπές σε ημερομηνίες (`TransactionDate`, `PreviousTransactionDate`).
5. Label encoding σε κατηγορικές μεταβλητές.
6. MinMax scaling σε αριθμητικά πεδία.
7. Δημιουργία χαρακτηριστικών όπως:
   - `time_diff`
   - `location_change`
8. Εξαγωγή στο: `data/clean_transactions_data.csv`.

### `notebooks/02_exploratory_data_analysis.ipynb`
Βήματα:
1. EDA σε καθαρισμένα δεδομένα.
2. Ανάλυση κατανομών και outliers.
3. Οπτικοποιήσεις (scatterplots, boxplots, hourly activity, correlations).
4. Δημιουργία κανόνων/flags ύποπτης συμπεριφοράς, όπως:
   - `high_amount_flag`
   - `many_login_attempts_flag`
   - `suspicious_merchant_flag`
   - `reactivation_suspect_flag`
   - `amount_exceeds_balance`
5. Εξαγωγή στο: `data/clean_transactions_with_flags.csv`.

### `notebooks/03_statistical_methods.ipynb`
Βήματα:
1. Υπολογισμός z-scores σε επιλεγμένα αριθμητικά χαρακτηριστικά.
2. Δημιουργία δυαδικών flags με κανόνα `|z| > 3`.
3. Συνένωση με υπάρχον dataset.
4. Εξαγωγή στο: `data/transactions_with_zscores.csv`.

### `notebooks/04_isolation_forest_model.ipynb`
Βήματα:
1. Εκπαίδευση Isolation Forest για unsupervised εντοπισμό ανωμαλιών.
2. Δημιουργία πεδίου `isolation_outlier_label`.
3. Συνδυασμός πολλαπλών flags σε τελική ένδειξη:
   - `final_anomaly_flag`
4. Εξαγωγή στο: `data/transactions_with_all_flags_final.csv.csv`.

### `notebooks/05_supervised_anomaly_model.ipynb`
Βήματα:
1. Train/test split με stratification.
2. Εκπαίδευση RandomForestClassifier.
3. Αξιολόγηση με classification report και confusion matrix.
4. Ανάλυση feature importance.
5. Δημιουργία τελικού συνόλου μόνο με anomalies.
6. Εξαγωγή στο: `transactions_anomalies_only.csv`.

---

## 3) Data artifacts (παραγόμενα αρχεία)

- `data/bank_transactions_data.csv` → αρχικό dataset (**2512 γραμμές, 16 στήλες**)
- `data/clean_transactions_data.csv` → μετά τον καθαρισμό/feature engineering (**2512, 18**)
- `data/clean_transactions_with_flags.csv` → μετά από EDA flags (**2512, 31**)
- `data/transactions_with_zscores.csv` → μετά από στατιστικές μεθόδους (**2512, 47**)
- `data/transactions_with_all_flags_final.csv.csv` → τελικό dataset με συνδυασμένα flags (**2512, 50**)
- `transactions_anomalies_only.csv` → τελικό extract ύποπτων συναλλαγών (**947, 28**)

Κατανομή τελικής ετικέτας (`final_anomaly_flag`) στο πλήρες τελικό dataset:
- `0` (μη ανωμαλία): **1565**
- `1` (ανωμαλία): **947**

---

## 4) Προτεινόμενο περιβάλλον

Ελάχιστες βιβλιοθήκες Python:
- `pandas`
- `numpy`
- `matplotlib`
- `seaborn`
- `scipy`
- `scikit-learn`
- `jupyter`

Ενδεικτική εγκατάσταση:

```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn jupyter
```

---

## 5) Πώς να τρέξεις το pipeline

1. Άνοιξε τα notebooks με τη σειρά:
   1. `01_data_understanding_cleaning.ipynb`
   2. `02_exploratory_data_analysis.ipynb`
   3. `03_statistical_methods.ipynb`
   4. `04_isolation_forest_model.ipynb`
   5. `05_supervised_anomaly_model.ipynb`
2. Βεβαιώσου ότι οι σχετικές διαδρομές (`../data/...`) παραμένουν ίδιες.
3. Έλεγξε ότι κάθε notebook ολοκληρώνεται πριν προχωρήσεις στο επόμενο.

---

## 6) Σημειώσεις / Παρατηρήσεις

- Το αρχείο `transactions_with_all_flags_final.csv.csv` έχει διπλή κατάληξη (`.csv.csv`). Λειτουργεί κανονικά, αλλά προαιρετικά μπορεί να μετονομαστεί για καθαρότητα.
- Το project είναι ιδανικό ως βάση για:
  - threshold tuning,
  - cost-sensitive learning,
  - explainability πάνω σε flagged συναλλαγές,
  - σύγκριση με πιο εξειδικευμένα anomaly detection μοντέλα.

---

## 7) Σύντομη σύνοψη

Το repository υλοποιεί end-to-end fraud detection workflow σε συναλλαγές: από data cleaning και κανόνες συμπεριφοράς μέχρι μοντέλα ανίχνευσης ανωμαλιών και παραγωγή τελικού αρχείου με τις ύποπτες εγγραφές.
