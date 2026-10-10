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


### Results

#### 1. Provider

| # | Model        | Description      | Log Loss | ROC AUC | Brier Score | Actual Goals | Expected Goals |
|:-:|:-------------|:-----------------|:--------:|:-------:|:-----------:|:------------:|:--------------:|
| 1 | PV1          | Statsbomb xGoals |  0.2561  | 0.8080  |   0.0722    |     5465     |     5225.7     |
| 2 | PV2          | UnderStat        |  0.2491  | 0.8130  |   0.0702    |    58957     |    62412.0     |

#### 2. Baseline 

| # | Model | Description | Log Loss | ROC AUC | Brier Score | Actual Goals | Expected Goals |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| 1 | BL1.1.1.1 | Default Params + No Weights + No Calibration | 0.2818 | 0.7411 | 0.0792 | 60584 | 60575.0 |
| 2 | BL1.1.1.2 | Default Params + No Weights + Platt Scaling | 0.2861 | 0.7411 | 0.0799 | 60584 | 60614.5 |
| 3 | BL1.1.1.3 | Default Params + No Weights + Isotonic Regression | 0.2807 | 0.7405 | 0.0790 | 60584 | 60584.9 |
| 4 | BL1.1.2.1 | Default Params + Class Weight + No Calibration | 0.5947 | 0.7408 | 0.2029 | 60584 | 267265.3 |
| 5 | BL1.1.2.2 | Default Params + Class Weight + Platt Scaling | 0.2831 | 0.7408 | 0.0798 | 60584 | 60573.1 |
| 6 | BL1.1.2.3 | Default Params + Class Weight + Isotonic Regression | 0.2808 | 0.7402 | 0.0790 | 60584 | 60584.6 |
| 7 | BL1.1.3.1 | Default Params + Undersampling + No Calibration | 0.5950 | 0.7408 | 0.2030 | 60584 | 267283.3 |
| 8 | BL1.1.3.2 | Default Params + Undersampling + Platt Scaling | 0.2831 | 0.7408 | 0.0798 | 60584 | 60572.6 |
| 9 | BL1.1.3.3 | Default Params + Undersampling + Isotonic Regression | 0.2808 | 0.7403 | 0.0791 | 60584 | 60584.9 |
| 10 | BL1.1.4.1 | Default Params + Oversampling + No Calibration | 0.6061 | 0.7395 | 0.2096 | 60584 | 270332.6 |
| 11 | BL1.1.4.2 | Default Params + Oversampling + Platt Scaling | 0.2838 | 0.7395 | 0.0801 | 60584 | 60625.6 |
| 12 | BL1.1.4.3 | Default Params + Oversampling + Isotonic Regression | 0.2812 | 0.7389 | 0.0791 | 60584 | 60584.3 |
| 13 | BL1.2.1.1 | Tuned Params + No Weights + No Calibration | 0.2818 | 0.7411 | 0.0792 | 60584 | 60575.0 |
| 14 | BL1.2.1.2 | Tuned Params + No Weights + Platt Scaling | 0.2861 | 0.7411 | 0.0799 | 60584 | 60614.5 |
| 15 | BL1.2.1.3 | Tuned Params + No Weights + Isotonic Regression | 0.2807 | 0.7405 | 0.0790 | 60584 | 60584.8 |
| 16 | BL1.2.2.1 | Tuned Params + Class Weight + No Calibration | 0.5947 | 0.7409 | 0.2028 | 60584 | 267628.3 |
| 17 | BL1.2.2.2 | Tuned Params + Class Weight + Platt Scaling | 0.2831 | 0.7408 | 0.0798 | 60584 | 60568.4 |
| 18 | BL1.2.2.3 | Tuned Params + Class Weight + Isotonic Regression | 0.2808 | 0.7402 | 0.0790 | 60584 | 60583.7 |
| 19 | BL1.2.3.1 | Tuned Params + Undersampling + No Calibration | 0.5949 | 0.7410 | 0.2028 | 60584 | 268171.9 |
| 20 | BL1.2.3.2 | Tuned Params + Undersampling + Platt Scaling | 0.2830 | 0.7410 | 0.0798 | 60584 | 60540.4 |
| 21 | BL1.2.3.3 | Tuned Params + Undersampling + Isotonic Regression | 0.2808 | 0.7403 | 0.0790 | 60584 | 60584.8 |
| 22 | BL1.2.4.1 | Tuned Params + Oversampling + No Calibration | 0.6061 | 0.7395 | 0.2096 | 60584 | 270553.4 |
| 23 | BL1.2.4.2 | Tuned Params + Oversampling + Platt Scaling | 0.2837 | 0.7395 | 0.0801 | 60584 | 60591.6 |
| 24 | BL1.2.4.3 | Tuned Params + Oversampling + Isotonic Regression | 0.2808 | 0.7403 | 0.0790 | 60584 | 60584.8 |