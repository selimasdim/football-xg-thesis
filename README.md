# Thesis - Football Expected Goals (xG)

### Μοντέλα προς Σύγκριση (Κατηγορίες)

#### 1. Provider
/Provider models/
* **PV1**: Statsbomb xG
* **PV2**: Understat xG

#### 2. Baseline 
/Baseline Models/

(μόνο x, y)
* **BL1**: Logistic Regression
* **BL2**: Decision Trees
* **BL3**: Random Forests
* Gradient Boosting Trees (πχ XGBoost, LightGBM)
* Neural Networks (απλά Multi-Layer Perceptrons)

#### 3. Baseline+ 
(x, y + Features)
* Logistic Regression
* Decision Trees
* Random Forests
* Gradient Boosting Trees (πχ XGBoost, LightGBM)
* Neural Networks (απλά Multi-Layer Perceptrons)

#### 4. Enhanced 
Μοντέλα με Feature Engineering (x, y + FM Player Attributes)
* Logistic Regression
* Decision Trees
* Random Forests
* Gradient Boosting Trees (πχ XGBoost, LightGBM)
* Deep Learning (πιο βαθιά Neural Networks)

#### 5. Enhanced+ 
Μοντέλα με Feature Engineering (x, y + Features + FM Player Attributes)
* Logistic Regression
* Decision Trees
* Random Forests
* Gradient Boosting Trees (πχ XGBoost, LightGBM)
* Deep Learning (πιο βαθιά Neural Networks)

#### 6. Provider models + FM Attributes
* Πώς θα τα κάνω enhance;

#### 7. Unsupervised / Προηγμένα Μοντέλα
* **Graph Neural Networks (GNNs):** Μοντελοποίηση χωρικών σχέσεων στο γήπεδο
* **K-Means Clustering:** Ομαδοποίηση παικτών με βάση τα χαρακτηριστικά τους (μπορεί να χρησιμοποιηθεί;)
* **Principal Component Analysis (PCA):** Για μείωση των πολλών attributes του FM

---

### Εξτρά Μοντέλα (Χρειάζεται κάτι από αυτά;)
* Generalized Additive Model, Support Vector Machines (SVM), KNN
* Calibrated models (Platt/Isotonic) *(αυτά τα χρησιμοποιώ ως addon για τα trees)*

---

### Μετρικές Αξιολόγησης (για κάθε μοντέλο)

#### Βασικές Μετρικές
* Brier Score
* Log-Loss (Cross-Entropy Loss)
* ROC-AUC (Area Under the Curve)

#### Έξτρα
* Calibration Curve (Reliability Diagram)
* Normalized Brier Score
* RMSE / MAE (Root Mean Square Error / Mean Absolute Error)
* Accuracy, Precision, Recall, F1-Score *(Μάλλον δεν κάνουν λόγω imbalanced classes — ~10% των σουτ μετατρέπονται σε γκολ)*