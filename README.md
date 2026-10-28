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
* **BL4**: LightBGM
* **BL5**: XGBoost
* **BL6**: MLP
* Neural Networks (αλλα εκτός Multi-Layer Perceptrons)

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
###### (input: x, y coordinates of shots)

##### BL1 (Logistic Regression)

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


##### BL2 (Decision Tree)


| # | Model | Description | Log Loss | ROC AUC | Brier Score | Actual Goals | Expected Goals |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| 1 | BL2.1.1.1 | Default Params + No Weights + No Calibration | 1.4618 | 0.6782 | 0.0921 | 60584 | 60739.5 |
| 2 | BL2.1.1.2 | Default Params + No Weights + Platt Scaling | 0.3008 | 0.6778 | 0.0842 | 60584 | 60587.2 |
| 3 | BL2.1.1.3 | Default Params + No Weights + Isotonic Regression | 0.2969 | 0.6778 | 0.0829 | 60584 | 60583.3 |
| 4 | BL2.1.2.1 | Default Params + Class Weight + No Calibration | 1.7091 | 0.6787 | 0.1993 | 60584 | 182124.9 |
| 5 | BL2.1.2.2 | Default Params + Class Weight + Platt Scaling | 0.2995 | 0.6779 | 0.0837 | 60584 | 60583.1 |
| 6 | BL2.1.2.3 | Default Params + Class Weight + Isotonic Regression | 0.2968 | 0.6781 | 0.0828 | 60584 | 60593.4 |
| 7 | BL2.1.3.1 | Default Params + Undersampling + No Calibration | 7.8222 | 0.6414 | 0.3185 | 60584 | 260062.1 |
| 8 | BL2.1.3.2 | Default Params + Undersampling + Platt Scaling | 0.3075 | 0.6407 | 0.0860 | 60584 | 60619.6 |
| 9 | BL2.1.3.3 | Default Params + Undersampling + Isotonic Regression | 0.3054 | 0.6479 | 0.0855 | 60584 | 60584.6 |
| 10 | BL2.1.4.1 | Default Params + Oversampling + No Calibration | 2.1087 | 0.6758 | 0.1493 | 60584 | 123406.2 |
| 11 | BL2.1.4.2 | Default Params + Oversampling + Platt Scaling | 0.3010 | 0.6751 | 0.0843 | 60584 | 60543.3 |
| 12 | BL2.1.4.3 | Default Params + Oversampling + Isotonic Regression | 0.2996 | 0.6755 | 0.0839 | 60584 | 60583.5 |
| 13 | BL2.2.1.1 | Tuned Params + No Weights + No Calibration | 0.2800 | 0.7432 | 0.0790 | 60584 | 60576.8 |
| 14 | BL2.2.1.2 | Tuned Params + No Weights + Platt Scaling | 0.2852 | 0.7432 | 0.0799 | 60584 | 60579.6 |
| 15 | BL2.2.1.3 | Tuned Params + No Weights + Isotonic Regression | 0.2801 | 0.7427 | 0.0790 | 60584 | 60544.7 |
| 16 | BL2.2.2.1 | Tuned Params + Class Weight + No Calibration | 0.5892 | 0.7433 | 0.2028 | 60584 | 263928.9 |
| 17 | BL2.2.2.2 | Tuned Params + Class Weight + Platt Scaling | 0.2811 | 0.7433 | 0.0794 | 60584 | 60541.0 |
| 18 | BL2.2.2.3 | Tuned Params + Class Weight + Isotonic Regression | 0.2801 | 0.7429 | 0.0790 | 60584 | 60575.4 |
| 19 | BL2.2.3.1 | Tuned Params + Undersampling + No Calibration | 0.5914 | 0.7422 | 0.2036 | 60584 | 264621.3 |
| 20 | BL2.2.3.2 | Tuned Params + Undersampling + Platt Scaling | 0.2814 | 0.7422 | 0.0795 | 60584 | 60542.1 |
| 21 | BL2.2.3.3 | Tuned Params + Undersampling + Isotonic Regression | 0.2805 | 0.7416 | 0.0791 | 60584 | 60475.7 |
| 22 | BL2.2.4.1 | Tuned Params + Oversampling + No Calibration | 0.5991 | 0.7424 | 0.2102 | 60584 | 265241.1 |
| 23 | BL2.2.4.2 | Tuned Params + Oversampling + Platt Scaling | 0.2831 | 0.7424 | 0.0802 | 60584 | 60591.2 |
| 24 | BL2.2.4.3 | Tuned Params + Oversampling + Isotonic Regression | 0.2812 | 0.7419 | 0.0795 | 60584 | 60621.9 |


##### BL3 (Random Forest)


| # | Model | Description | Log Loss | ROC AUC | Brier Score | Actual Goals | Expected Goals |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| 1 | BL3.1.1.1 | Default Params + No Weights + No Calibration | 1.0268 | 0.6729 | 0.0916 | 60584 | 60966.6 |
| 2 | BL3.1.1.2 | Default Params + No Weights + Platt Scaling | 0.3000 | 0.6724 | 0.0834 | 60584 | 60619.2 |
| 3 | BL3.1.1.3 | Default Params + No Weights + Isotonic Regression | 0.2983 | 0.6721 | 0.0830 | 60584 | 60583.0 |
| 4 | BL3.1.2.1 | Default Params + Class Weight + No Calibration | 1.1075 | 0.6778 | 0.1896 | 60584 | 170772.3 |
| 5 | BL3.1.2.2 | Default Params + Class Weight + Platt Scaling | 0.3012 | 0.6777 | 0.0842 | 60584 | 60581.6 |
| 6 | BL3.1.2.3 | Default Params + Class Weight + Isotonic Regression | 0.2993 | 0.6774 | 0.0837 | 60584 | 60582.7 |
| 7 | BL3.1.3.1 | Default Params + Undersampling + No Calibration | 1.1262 | 0.6823 | 0.2522 | 60584 | 263208.2 |
| 8 | BL3.1.3.2 | Default Params + Undersampling + Platt Scaling | 0.3007 | 0.6823 | 0.0842 | 60584 | 60590.3 |
| 9 | BL3.1.3.3 | Default Params + Undersampling + Isotonic Regression | 0.2984 | 0.6819 | 0.0834 | 60584 | 60585.5 |
| 10 | BL3.1.4.1 | Default Params + Oversampling + No Calibration | 1.0820 | 0.6643 | 0.1287 | 60584 | 108269.0 |
| 11 | BL3.1.4.2 | Default Params + Oversampling + Platt Scaling | 0.3025 | 0.6640 | 0.0843 | 60584 | 60584.7 |
| 12 | BL3.1.4.3 | Default Params + Oversampling + Isotonic Regression | 0.3014 | 0.6639 | 0.0840 | 60584 | 60583.7 |
| 13 | BL3.2.1.1 | Tuned Params + No Weights + No Calibration | 0.2792 | 0.7465 | 0.0788 | 60584 | 60570.9 |
| 14 | BL3.2.1.2 | Tuned Params + No Weights + Platt Scaling | 0.2845 | 0.7465 | 0.0797 | 60584 | 60580.9 |
| 15 | BL3.2.1.3 | Tuned Params + No Weights + Isotonic Regression | 0.2793 | 0.7460 | 0.0788 | 60584 | 60587.2 |
| 16 | BL3.2.2.1 | Tuned Params + Class Weight + No Calibration | 0.5839 | 0.7455 | 0.2007 | 60584 | 261787.9 |
| 17 | BL3.2.2.2 | Tuned Params + Class Weight + Platt Scaling | 0.2806 | 0.7455 | 0.0793 | 60584 | 60578.8 |
| 18 | BL3.2.2.3 | Tuned Params + Class Weight + Isotonic Regression | 0.2796 | 0.7450 | 0.0789 | 60584 | 60591.9 |
| 19 | BL3.2.3.1 | Tuned Params + Undersampling + No Calibration | 0.5899 | 0.7449 | 0.2030 | 60584 | 264110.3 |
| 20 | BL3.2.3.2 | Tuned Params + Undersampling + Platt Scaling | 0.2807 | 0.7449 | 0.0793 | 60584 | 60559.3 |
| 21 | BL3.2.3.3 | Tuned Params + Undersampling + Isotonic Regression | 0.2801 | 0.7443 | 0.0790 | 60584 | 60575.8 |
| 22 | BL3.2.4.1 | Tuned Params + Oversampling + No Calibration | 0.5818 | 0.7439 | 0.2024 | 60584 | 257641.2 |
| 23 | BL3.2.4.2 | Tuned Params + Oversampling + Platt Scaling | 0.2828 | 0.7439 | 0.0801 | 60584 | 60587.4 |
| 24 | BL3.2.4.3 | Tuned Params + Oversampling + Isotonic Regression | 0.2813 | 0.7432 | 0.0794 | 60584 | 60793.4 |


##### BL4 (LightBGM)

| # | Model | Description | Log Loss | ROC AUC | Brier Score | Actual Goals | Expected Goals |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| 1 | BL4.1.1.1 | Default Params + No Weights + No Calibration | 0.2792 | 0.7467 | 0.0788 | 60584 | 60584.3 |
| 2 | BL4.1.1.2 | Default Params + No Weights + Platt Scaling | 0.2845 | 0.7467 | 0.0797 | 60584 | 60579.6 |
| 3 | BL4.1.1.3 | Default Params + No Weights + Isotonic Regression | 0.2793 | 0.7462 | 0.0788 | 60584 | 60586.8 |
| 4 | BL4.1.2.1 | Default Params + Class Weight + No Calibration | 0.5853 | 0.7466 | 0.2012 | 60584 | 262783.2 |
| 5 | BL4.1.2.2 | Default Params + Class Weight + Platt Scaling | 0.2803 | 0.7466 | 0.0792 | 60584 | 60546.7 |
| 6 | BL4.1.2.3 | Default Params + Class Weight + Isotonic Regression | 0.2794 | 0.7461 | 0.0788 | 60584 | 60585.2 |
| 7 | BL4.1.3.1 | Default Params + Undersampling + No Calibration | 0.5883 | 0.7470 | 0.2022 | 60584 | 263934.8 |
| 8 | BL4.1.3.2 | Default Params + Undersampling + Platt Scaling | 0.2801 | 0.7470 | 0.0791 | 60584 | 60547.7 |
| 9 | BL4.1.3.3 | Default Params + Undersampling + Isotonic Regression | 0.2796 | 0.7465 | 0.0788 | 60584 | 60583.1 |
| 10 | BL4.1.4.1 | Default Params + Oversampling + No Calibration | 0.5791 | 0.7433 | 0.2007 | 60584 | 257185.6 |
| 11 | BL4.1.4.2 | Default Params + Oversampling + Platt Scaling | 0.2829 | 0.7433 | 0.0802 | 60584 | 60589.9 |
| 12 | BL4.1.4.3 | Default Params + Oversampling + Isotonic Regression | 0.2812 | 0.7427 | 0.0795 | 60584 | 60587.1 |
| 13 | BL4.2.1.1 | Tuned Params + No Weights + No Calibration | 0.2789 | 0.7481 | 0.0787 | 60584 | 60582.4 |
| 14 | BL4.2.1.2 | Tuned Params + No Weights + Platt Scaling | 0.2842 | 0.7481 | 0.0796 | 60584 | 60582.9 |
| 15 | BL4.2.1.3 | Tuned Params + No Weights + Isotonic Regression | 0.2790 | 0.7476 | 0.0787 | 60584 | 60585.8 |
| 16 | BL4.2.2.1 | Tuned Params + Class Weight + No Calibration | 0.5379 | 0.7169 | 0.1869 | 60584 | 217466.9 |
| 17 | BL4.2.2.2 | Tuned Params + Class Weight + Platt Scaling | 0.2913 | 0.7169 | 0.0818 | 60584 | 60601.5 |
| 18 | BL4.2.2.3 | Tuned Params + Class Weight + Isotonic Regression | 0.2873 | 0.7164 | 0.0804 | 60584 | 60583.1 |
| 19 | BL4.2.3.1 | Tuned Params + Undersampling + No Calibration | 0.5869 | 0.7488 | 0.2015 | 60584 | 263229.3 |
| 20 | BL4.2.3.2 | Tuned Params + Undersampling + Platt Scaling | 0.2796 | 0.7488 | 0.0790 | 60584 | 60595.6 |
| 21 | BL4.2.3.3 | Tuned Params + Undersampling + Isotonic Regression | 0.2788 | 0.7483 | 0.0786 | 60584 | 60584.0 |
| 22 | BL4.2.4.1 | Tuned Params + Oversampling + No Calibration | 0.5128 | 0.6974 | 0.1724 | 60584 | 201801.6 |
| 23 | BL4.2.4.2 | Tuned Params + Oversampling + Platt Scaling | 0.2969 | 0.6974 | 0.0833 | 60584 | 60567.8 |
| 24 | BL4.2.4.3 | Tuned Params + Oversampling + Isotonic Regression | 0.2957 | 0.6968 | 0.0830 | 60584 | 60580.8 |


##### BL5 (XGBoost)

| # | Model | Description | Log Loss | ROC AUC | Brier Score | Actual Goals | Expected Goals |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| 1 | BL5.1.1.1 | Default Params + No Weights + No Calibration | 0.2793 | 0.7468 | 0.0787 | 60584 | 60595.2 |
| 2 | BL5.1.1.2 | Default Params + No Weights + Platt Scaling | 0.2842 | 0.7468 | 0.0795 | 60584 | 60588.2 |
| 3 | BL5.1.1.3 | Default Params + No Weights + Isotonic Regression | 0.2793 | 0.7462 | 0.0787 | 60584 | 60582.2 |
| 4 | BL5.1.2.1 | Default Params + Class Weight + No Calibration | 0.5784 | 0.7464 | 0.1988 | 60584 | 258528.6 |
| 5 | BL5.1.2.2 | Default Params + Class Weight + Platt Scaling | 0.2804 | 0.7464 | 0.0791 | 60584 | 60590.5 |
| 6 | BL5.1.2.3 | Default Params + Class Weight + Isotonic Regression | 0.2794 | 0.7458 | 0.0787 | 60584 | 60584.0 |
| 7 | BL5.1.3.1 | Default Params + Undersampling + No Calibration | 0.5901 | 0.7456 | 0.2029 | 60584 | 262866.9 |
| 8 | BL5.1.3.2 | Default Params + Undersampling + Platt Scaling | 0.2808 | 0.7456 | 0.0793 | 60584 | 60592.5 |
| 9 | BL5.1.3.3 | Default Params + Undersampling + Isotonic Regression | 0.2799 | 0.7451 | 0.0789 | 60584 | 60583.2 |
| 10 | BL5.1.4.1 | Default Params + Oversampling + No Calibration | 0.5702 | 0.7412 | 0.1972 | 60584 | 251566.5 |
| 11 | BL5.1.4.2 | Default Params + Oversampling + Platt Scaling | 0.2837 | 0.7412 | 0.0803 | 60584 | 60558.3 |
| 12 | BL5.1.4.3 | Default Params + Oversampling + Isotonic Regression | 0.2823 | 0.7407 | 0.0797 | 60584 | 60580.7 |
| 13 | BL5.2.1.1 | Tuned Params + No Weights + No Calibration | 0.2783 | 0.7498 | 0.0785 | 60584 | 60581.4 |
| 14 | BL5.2.1.2 | Tuned Params + No Weights + Platt Scaling | 0.2837 | 0.7498 | 0.0794 | 60584 | 60584.6 |
| 15 | BL5.2.1.3 | Tuned Params + No Weights + Isotonic Regression | 0.2784 | 0.7492 | 0.0786 | 60584 | 60585.6 |
| 16 | BL5.2.2.1 | Tuned Params + Class Weight + No Calibration | 0.5236 | 0.7158 | 0.1801 | 60584 | 209144.4 |
| 17 | BL5.2.2.2 | Tuned Params + Class Weight + Platt Scaling | 0.2919 | 0.7158 | 0.0819 | 60584 | 60597.2 |
| 18 | BL5.2.2.3 | Tuned Params + Class Weight + Isotonic Regression | 0.2878 | 0.7152 | 0.0806 | 60584 | 60583.5 |
| 19 | BL5.2.3.1 | Tuned Params + Undersampling + No Calibration | 0.5901 | 0.7456 | 0.2029 | 60584 | 262866.9 |
| 20 | BL5.2.3.2 | Tuned Params + Undersampling + Platt Scaling | 0.2808 | 0.7456 | 0.0793 | 60584 | 60592.5 |
| 21 | BL5.2.3.3 | Tuned Params + Undersampling + Isotonic Regression | 0.2799 | 0.7451 | 0.0789 | 60584 | 60583.2 |
| 22 | BL5.2.4.1 | Tuned Params + Oversampling + No Calibration | 0.5702 | 0.7412 | 0.1972 | 60584 | 251566.5 |
| 23 | BL5.2.4.2 | Tuned Params + Oversampling + Platt Scaling | 0.2837 | 0.7412 | 0.0803 | 60584 | 60558.3 |
| 24 | BL5.2.4.3 | Tuned Params + Oversampling + Isotonic Regression | 0.2823 | 0.7407 | 0.0797 | 60584 | 60580.7 |


##### MultiLayer Perceptron

| # | Model | Description | Log Loss | ROC AUC | Brier Score | Actual Goals | Expected Goals |
| :---: | :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| 1 | BL6.1.1.1 | Default Params + No Weights + No Calibration | 0.2805 | 0.7437 | 0.0791 | 60584 | 56020.4 |
| 2 | BL6.1.1.2 | Default Params + No Weights + Platt Scaling | 0.2846 | 0.7437 | 0.0798 | 60584 | 60589.1 |
| 3 | BL6.1.1.3 | Default Params + No Weights + Isotonic Regression | 0.2800 | 0.7432 | 0.0790 | 60584 | 60589.9 |
| 4 | BL6.1.2.1 | Default Params + Class Weight + No Calibration | 0.5529 | 0.7441 | 0.1883 | 60584 | 249339.1 |
| 5 | BL6.1.2.2 | Default Params + Class Weight + Platt Scaling | 0.2810 | 0.7440 | 0.0793 | 60584 | 60589.3 |
| 6 | BL6.1.2.3 | Default Params + Class Weight + Isotonic Regression | 0.2799 | 0.7434 | 0.0790 | 60584 | 60587.0 |
| 7 | BL6.1.3.1 | Default Params + Undersampling + No Calibration | 0.6603 | 0.7429 | 0.2298 | 60584 | 296409.0 |
| 8 | BL6.1.3.2 | Default Params + Undersampling + Platt Scaling | 0.2810 | 0.7428 | 0.0793 | 60584 | 60591.7 |
| 9 | BL6.1.3.3 | Default Params + Undersampling + Isotonic Regression | 0.2803 | 0.7421 | 0.0790 | 60584 | 60511.0 |
| 10 | BL6.1.4.1 | Default Params + Oversampling + No Calibration | 7.4103 | 0.6307 | 0.8992 | 60584 | 621398.2 |
| 11 | BL6.1.4.2 | Default Params + Oversampling + Platt Scaling | 0.2831 | 0.7424 | 0.0800 | 60584 | 60583.2 |
| 12 | BL6.1.4.3 | Default Params + Oversampling + Isotonic Regression | 0.2808 | 0.7416 | 0.0792 | 60584 | 60587.0 |
| 13 | BL6.2.1.1 | Tuned Params + No Weights + No Calibration | 0.2804 | 0.7439 | 0.0791 | 60584 | 56386.9 |
| 14 | BL6.2.1.2 | Tuned Params + No Weights + Platt Scaling | 0.2844 | 0.7439 | 0.0797 | 60584 | 60586.3 |
| 15 | BL6.2.1.3 | Tuned Params + No Weights + Isotonic Regression | 0.2803 | 0.7421 | 0.0790 | 60584 | 60511.0 |
| 16 | BL6.2.2.1 | Tuned Params + Class Weight + No Calibration | 0.5424 | 0.7437 | 0.1829 | 60584 | 245683.9 |
| 17 | BL6.2.2.2 | Tuned Params + Class Weight + Platt Scaling | 0.2813 | 0.7437 | 0.0794 | 60584 | 60549.1 |
| 18 | BL6.2.2.3 | Tuned Params + Class Weight + Isotonic Regression | 0.2800 | 0.7430 | 0.0790 | 60584 | 60592.1 |
| 19 | BL6.2.3.1 | Tuned Params + Undersampling + No Calibration | 0.6117 | 0.7436 | 0.2120 | 60584 | 276242.0 |
| 20 | BL6.2.3.2 | Tuned Params + Undersampling + Platt Scaling | 0.2807 | 0.7436 | 0.0793 | 60584 | 60594.5 |
| 21 | BL6.2.3.3 | Tuned Params + Undersampling + Isotonic Regression | 0.2799 | 0.7430 | 0.0790 | 60584 | 60581.0 |
| 22 | BL6.2.4.1 | Tuned Params + Oversampling + No Calibration | 0.8388 | 0.7311 | 0.3038 | 60584 | 348690.3 |
| 23 | BL6.2.4.2 | Tuned Params + Oversampling + Platt Scaling | 0.2855 | 0.7294 | 0.0804 | 60584 | 60845.4 |
| 24 | BL6.2.4.3 | Tuned Params + Oversampling + Isotonic Regression | 0.2847 | 0.7262 | 0.0799 | 60584 | 61250.8 |

#### Συμπεράσματα για BaseLine

* Tuned Parameters > Default Parameters (και ο ήλιος βγαίνει από την ανατολή χαχαχα)
* Isotonic Regression > Platt Scaling
* Imbalance (+Isotonic): None > Class Weight > Undersampling > Oversampling


### Model Ranking

### Πλήρης Κατάταξη Όλων των Μοντέλων (Αύξουσα Σειρά Log Loss)

| # | Model | Description | Log Loss | ROC AUC | Brier Score | Actual Goals | Expected Goals |
|:-:|:------|:------------|:--------:|:-------:|:-----------:|:------------:|:--------------:|
| 1 | **PV2** | (Provider) UnderStat | **0.2491** | 0.8130 | 0.0702 | 58957 | 62412.0 |
| 2 | **PV1** | (Provider) Statsbomb xGoals | **0.2561** | 0.8080 | 0.0722 | 5465 | 5225.7 |
| 3 | **BL5.2.1.1** | (XGBoost) Tuned Params + No Weights + No Calibration | **0.2783** | 0.7498 | 0.0785 | 60584 | 60581.4 |
| 4 | **BL5.2.1.3** | (XGBoost) Tuned Params + No Weights + Isotonic Regression | **0.2784** | 0.7492 | 0.0786 | 60584 | 60585.6 |
| 5 | **BL4.2.3.3** | (LightBGM) Tuned Params + Undersampling + Isotonic Regression | **0.2788** | 0.7483 | 0.0786 | 60584 | 60584.0 |
| 6 | **BL4.2.1.1** | (LightBGM) Tuned Params + No Weights + No Calibration | **0.2789** | 0.7481 | 0.0787 | 60584 | 60582.4 |
| 7 | **BL4.2.1.3** | (LightBGM) Tuned Params + No Weights + Isotonic Regression | **0.2790** | 0.7476 | 0.0787 | 60584 | 60585.8 |
| 8 | **BL4.1.1.1** | (LightBGM) Default Params + No Weights + No Calibration | **0.2792** | 0.7467 | 0.0788 | 60584 | 60584.3 |
| 9 | **BL3.2.1.1** | (Random Forest) Tuned Params + No Weights + No Calibration | **0.2792** | 0.7465 | 0.0788 | 60584 | 60570.9 |
| 10 | **BL5.1.1.1** | (XGBoost) Default Params + No Weights + No Calibration | **0.2793** | 0.7468 | 0.0787 | 60584 | 60595.2 |
| 11 | **BL5.1.1.3** | (XGBoost) Default Params + No Weights + Isotonic Regression | **0.2793** | 0.7462 | 0.0787 | 60584 | 60582.2 |
| 12 | **BL4.1.1.3** | (LightBGM) Default Params + No Weights + Isotonic Regression | **0.2793** | 0.7462 | 0.0788 | 60584 | 60586.8 |
| 13 | **BL3.2.1.3** | (Random Forest) Tuned Params + No Weights + Isotonic Regression | **0.2793** | 0.7460 | 0.0788 | 60584 | 60587.2 |
| 14 | **BL4.1.2.3** | (LightBGM) Default Params + Class Weight + Isotonic Regression | **0.2794** | 0.7461 | 0.0788 | 60584 | 60585.2 |
| 15 | **BL5.1.2.3** | (XGBoost) Default Params + Class Weight + Isotonic Regression | **0.2794** | 0.7458 | 0.0787 | 60584 | 60584.0 |
| 16 | **BL4.2.3.2** | (LightBGM) Tuned Params + Undersampling + Platt Scaling | **0.2796** | 0.7488 | 0.0790 | 60584 | 60595.6 |
| 17 | **BL4.1.3.3** | (LightBGM) Default Params + Undersampling + Isotonic Regression | **0.2796** | 0.7465 | 0.0788 | 60584 | 60583.1 |
| 18 | **BL3.2.2.3** | (Random Forest) Tuned Params + Class Weight + Isotonic Regression | **0.2796** | 0.7450 | 0.0789 | 60584 | 60591.9 |
| 19 | **BL5.1.3.3** | (XGBoost) Default Params + Undersampling + Isotonic Regression | **0.2799** | 0.7451 | 0.0789 | 60584 | 60583.2 |
| 20 | **BL5.2.3.3** | (XGBoost) Tuned Params + Undersampling + Isotonic Regression | **0.2799** | 0.7451 | 0.0789 | 60584 | 60583.2 |
| 21 | **BL6.1.2.3** | (MLP) Default Params + Class Weight + Isotonic Regression | **0.2799** | 0.7434 | 0.0790 | 60584 | 60587.0 |
| 22 | **BL6.2.3.3** | (MLP) Tuned Params + Undersampling + Isotonic Regression | **0.2799** | 0.7430 | 0.0790 | 60584 | 60581.0 |
| 23 | **BL2.2.1.1** | (Decision Tree) Tuned Params + No Weights + No Calibration | **0.2800** | 0.7432 | 0.0790 | 60584 | 60576.8 |
| 24 | **BL6.1.1.3** | (MLP) Default Params + No Weights + Isotonic Regression | **0.2800** | 0.7432 | 0.0790 | 60584 | 60589.9 |
| 25 | **BL6.2.2.3** | (MLP) Tuned Params + Class Weight + Isotonic Regression | **0.2800** | 0.7430 | 0.0790 | 60584 | 60592.1 |
| 26 | **BL4.1.3.2** | (LightBGM) Default Params + Undersampling + Platt Scaling | **0.2801** | 0.7470 | 0.0791 | 60584 | 60547.7 |
| 27 | **BL3.2.3.3** | (Random Forest) Tuned Params + Undersampling + Isotonic Regression | **0.2801** | 0.7443 | 0.0790 | 60584 | 60575.8 |
| 28 | **BL2.2.2.3** | (Decision Tree) Tuned Params + Class Weight + Isotonic Regression | **0.2801** | 0.7429 | 0.0790 | 60584 | 60575.4 |
| 29 | **BL2.2.1.3** | (Decision Tree) Tuned Params + No Weights + Isotonic Regression | **0.2801** | 0.7427 | 0.0790 | 60584 | 60544.7 |
| 30 | **BL4.1.2.2** | (LightBGM) Default Params + Class Weight + Platt Scaling | **0.2803** | 0.7466 | 0.0792 | 60584 | 60546.7 |
| 31 | **BL6.1.3.3** | (MLP) Default Params + Undersampling + Isotonic Regression | **0.2803** | 0.7421 | 0.0790 | 60584 | 60511.0 |
| 32 | **BL6.2.1.3** | (MLP) Tuned Params + No Weights + Isotonic Regression | **0.2803** | 0.7421 | 0.0790 | 60584 | 60511.0 |
| 33 | **BL5.1.2.2** | (XGBoost) Default Params + Class Weight + Platt Scaling | **0.2804** | 0.7464 | 0.0791 | 60584 | 60590.5 |
| 34 | **BL6.2.1.1** | (MLP) Tuned Params + No Weights + No Calibration | **0.2804** | 0.7439 | 0.0791 | 60584 | 56386.9 |
| 35 | **BL6.1.1.1** | (MLP) Default Params + No Weights + No Calibration | **0.2805** | 0.7437 | 0.0791 | 60584 | 56020.4 |
| 36 | **BL2.2.3.3** | (Decision Tree) Tuned Params + Undersampling + Isotonic Regression | **0.2805** | 0.7416 | 0.0791 | 60584 | 60475.7 |
| 37 | **BL3.2.2.2** | (Random Forest) Tuned Params + Class Weight + Platt Scaling | **0.2806** | 0.7455 | 0.0793 | 60584 | 60578.8 |
| 38 | **BL3.2.3.2** | (Random Forest) Tuned Params + Undersampling + Platt Scaling | **0.2807** | 0.7449 | 0.0793 | 60584 | 60559.3 |
| 39 | **BL6.2.3.2** | (MLP) Tuned Params + Undersampling + Platt Scaling | **0.2807** | 0.7436 | 0.0793 | 60584 | 60594.5 |
| 40 | **BL1.1.1.3** | (LogRegr) Default Params + No Weights + Isotonic Regression | **0.2807** | 0.7405 | 0.0790 | 60584 | 60584.9 |
| 41 | **BL1.2.1.3** | (LogRegr) Tuned Params + No Weights + Isotonic Regression | **0.2807** | 0.7405 | 0.0790 | 60584 | 60584.8 |
| 42 | **BL5.1.3.2** | (XGBoost) Default Params + Undersampling + Platt Scaling | **0.2808** | 0.7456 | 0.0793 | 60584 | 60592.5 |
| 43 | **BL5.2.3.2** | (XGBoost) Tuned Params + Undersampling + Platt Scaling | **0.2808** | 0.7456 | 0.0793 | 60584 | 60592.5 |
| 44 | **BL6.1.4.3** | (MLP) Default Params + Oversampling + Isotonic Regression | **0.2808** | 0.7416 | 0.0792 | 60584 | 60587.0 |
| 45 | **BL1.2.3.3** | (LogRegr) Tuned Params + Undersampling + Isotonic Regression | **0.2808** | 0.7403 | 0.0790 | 60584 | 60584.8 |
| 46 | **BL1.2.4.3** | (LogRegr) Tuned Params + Oversampling + Isotonic Regression | **0.2808** | 0.7403 | 0.0790 | 60584 | 60584.8 |
| 47 | **BL1.1.3.3** | (LogRegr) Default Params + Undersampling + Isotonic Regression | **0.2808** | 0.7403 | 0.0791 | 60584 | 60584.9 |
| 48 | **BL1.1.2.3** | (LogRegr) Default Params + Class Weight + Isotonic Regression | **0.2808** | 0.7402 | 0.0790 | 60584 | 60584.6 |
| 49 | **BL1.2.2.3** | (LogRegr) Tuned Params + Class Weight + Isotonic Regression | **0.2808** | 0.7402 | 0.0790 | 60584 | 60583.7 |
| 50 | **BL6.1.2.2** | (MLP) Default Params + Class Weight + Platt Scaling | **0.2810** | 0.7440 | 0.0793 | 60584 | 60589.3 |
| 51 | **BL6.1.3.2** | (MLP) Default Params + Undersampling + Platt Scaling | **0.2810** | 0.7428 | 0.0793 | 60584 | 60591.7 |
| 52 | **BL2.2.2.2** | (Decision Tree) Tuned Params + Class Weight + Platt Scaling | **0.2811** | 0.7433 | 0.0794 | 60584 | 60541.0 |
| 53 | **BL4.1.4.3** | (LightBGM) Default Params + Oversampling + Isotonic Regression | **0.2812** | 0.7427 | 0.0795 | 60584 | 60587.1 |
| 54 | **BL2.2.4.3** | (Decision Tree) Tuned Params + Oversampling + Isotonic Regression | **0.2812** | 0.7419 | 0.0795 | 60584 | 60621.9 |
| 55 | **BL1.1.4.3** | (LogRegr) Default Params + Oversampling + Isotonic Regression | **0.2812** | 0.7389 | 0.0791 | 60584 | 60584.3 |
| 56 | **BL6.2.2.2** | (MLP) Tuned Params + Class Weight + Platt Scaling | **0.2813** | 0.7437 | 0.0794 | 60584 | 60549.1 |
| 57 | **BL3.2.4.3** | (Random Forest) Tuned Params + Oversampling + Isotonic Regression | **0.2813** | 0.7432 | 0.0794 | 60584 | 60793.4 |
| 58 | **BL2.2.3.2** | (Decision Tree) Tuned Params + Undersampling + Platt Scaling | **0.2814** | 0.7422 | 0.0795 | 60584 | 60542.1 |
| 59 | **BL1.1.1.1** | (LogRegr) Default Params + No Weights + No Calibration | **0.2818** | 0.7411 | 0.0792 | 60584 | 60575.0 |
| 60 | **BL1.2.1.1** | (LogRegr) Tuned Params + No Weights + No Calibration | **0.2818** | 0.7411 | 0.0792 | 60584 | 60575.0 |
| 61 | **BL5.1.4.3** | (XGBoost) Default Params + Oversampling + Isotonic Regression | **0.2823** | 0.7407 | 0.0797 | 60584 | 60580.7 |
| 62 | **BL5.2.4.3** | (XGBoost) Tuned Params + Oversampling + Isotonic Regression | **0.2823** | 0.7407 | 0.0797 | 60584 | 60580.7 |
| 63 | **BL3.2.4.2** | (Random Forest) Tuned Params + Oversampling + Platt Scaling | **0.2828** | 0.7439 | 0.0801 | 60584 | 60587.4 |
| 64 | **BL4.1.4.2** | (LightBGM) Default Params + Oversampling + Platt Scaling | **0.2829** | 0.7433 | 0.0802 | 60584 | 60589.9 |
| 65 | **BL1.2.3.2** | (LogRegr) Tuned Params + Undersampling + Platt Scaling | **0.2830** | 0.7410 | 0.0798 | 60584 | 60540.4 |
| 66 | **BL6.1.4.2** | (MLP) Default Params + Oversampling + Platt Scaling | **0.2831** | 0.7424 | 0.0800 | 60584 | 60583.2 |
| 67 | **BL2.2.4.2** | (Decision Tree) Tuned Params + Oversampling + Platt Scaling | **0.2831** | 0.7424 | 0.0802 | 60584 | 60591.2 |
| 68 | **BL1.1.2.2** | (LogRegr) Default Params + Class Weight + Platt Scaling | **0.2831** | 0.7408 | 0.0798 | 60584 | 60573.1 |
| 69 | **BL1.1.3.2** | (LogRegr) Default Params + Undersampling + Platt Scaling | **0.2831** | 0.7408 | 0.0798 | 60584 | 60572.6 |
| 70 | **BL1.2.2.2** | (LogRegr) Tuned Params + Class Weight + Platt Scaling | **0.2831** | 0.7408 | 0.0798 | 60584 | 60568.4 |
| 71 | **BL5.2.1.2** | (XGBoost) Tuned Params + No Weights + Platt Scaling | **0.2837** | 0.7498 | 0.0794 | 60584 | 60584.6 |
| 72 | **BL5.1.4.2** | (XGBoost) Default Params + Oversampling + Platt Scaling | **0.2837** | 0.7412 | 0.0803 | 60584 | 60558.3 |
| 73 | **BL5.2.4.2** | (XGBoost) Tuned Params + Oversampling + Platt Scaling | **0.2837** | 0.7412 | 0.0803 | 60584 | 60558.3 |
| 74 | **BL1.2.4.2** | (LogRegr) Tuned Params + Oversampling + Platt Scaling | **0.2837** | 0.7395 | 0.0801 | 60584 | 60591.6 |
| 75 | **BL1.1.4.2** | (LogRegr) Default Params + Oversampling + Platt Scaling | **0.2838** | 0.7395 | 0.0801 | 60584 | 60625.6 |
| 76 | **BL4.2.1.2** | (LightBGM) Tuned Params + No Weights + Platt Scaling | **0.2842** | 0.7481 | 0.0796 | 60584 | 60582.9 |
| 77 | **BL5.1.1.2** | (XGBoost) Default Params + No Weights + Platt Scaling | **0.2842** | 0.7468 | 0.0795 | 60584 | 60588.2 |
| 78 | **BL6.2.1.2** | (MLP) Tuned Params + No Weights + Platt Scaling | **0.2844** | 0.7439 | 0.0797 | 60584 | 60586.3 |
| 79 | **BL4.1.1.2** | (LightBGM) Default Params + No Weights + Platt Scaling | **0.2845** | 0.7467 | 0.0797 | 60584 | 60579.6 |
| 80 | **BL3.2.1.2** | (Random Forest) Tuned Params + No Weights + Platt Scaling | **0.2845** | 0.7465 | 0.0797 | 60584 | 60580.9 |
| 81 | **BL6.1.1.2** | (MLP) Default Params + No Weights + Platt Scaling | **0.2846** | 0.7437 | 0.0798 | 60584 | 60589.1 |
| 82 | **BL6.2.4.3** | (MLP) Tuned Params + Oversampling + Isotonic Regression | **0.2847** | 0.7262 | 0.0799 | 60584 | 61250.8 |
| 83 | **BL2.2.1.2** | (Decision Tree) Tuned Params + No Weights + Platt Scaling | **0.2852** | 0.7432 | 0.0799 | 60584 | 60579.6 |
| 84 | **BL6.2.4.2** | (MLP) Tuned Params + Oversampling + Platt Scaling | **0.2855** | 0.7294 | 0.0804 | 60584 | 60845.4 |
| 85 | **BL1.1.1.2** | (LogRegr) Default Params + No Weights + Platt Scaling | **0.2861** | 0.7411 | 0.0799 | 60584 | 60614.5 |
| 86 | **BL1.2.1.2** | (LogRegr) Tuned Params + No Weights + Platt Scaling | **0.2861** | 0.7411 | 0.0799 | 60584 | 60614.5 |
| 87 | **BL4.2.2.3** | (LightBGM) Tuned Params + Class Weight + Isotonic Regression | **0.2873** | 0.7164 | 0.0804 | 60584 | 60583.1 |
| 88 | **BL5.2.2.3** | (XGBoost) Tuned Params + Class Weight + Isotonic Regression | **0.2878** | 0.7152 | 0.0806 | 60584 | 60583.5 |
| 89 | **BL4.2.2.2** | (LightBGM) Tuned Params + Class Weight + Platt Scaling | **0.2913** | 0.7169 | 0.0818 | 60584 | 60601.5 |
| 90 | **BL5.2.2.2** | (XGBoost) Tuned Params + Class Weight + Platt Scaling | **0.2919** | 0.7158 | 0.0819 | 60584 | 60597.2 |
| 91 | **BL4.2.4.3** | (LightBGM) Tuned Params + Oversampling + Isotonic Regression | **0.2957** | 0.6968 | 0.0830 | 60584 | 60580.8 |
| 92 | **BL2.1.2.3** | (Decision Tree) Default Params + Class Weight + Isotonic Regression | **0.2968** | 0.6781 | 0.0828 | 60584 | 60593.4 |
| 93 | **BL4.2.4.2** | (LightBGM) Tuned Params + Oversampling + Platt Scaling | **0.2969** | 0.6974 | 0.0833 | 60584 | 60567.8 |
| 94 | **BL2.1.1.3** | (Decision Tree) Default Params + No Weights + Isotonic Regression | **0.2969** | 0.6778 | 0.0829 | 60584 | 60583.3 |
| 95 | **BL3.1.1.3** | (Random Forest) Default Params + No Weights + Isotonic Regression | **0.2983** | 0.6721 | 0.0830 | 60584 | 60583.0 |
| 96 | **BL3.1.3.3** | (Random Forest) Default Params + Undersampling + Isotonic Regression | **0.2984** | 0.6819 | 0.0834 | 60584 | 60585.5 |
| 97 | **BL3.1.2.3** | (Random Forest) Default Params + Class Weight + Isotonic Regression | **0.2993** | 0.6774 | 0.0837 | 60584 | 60582.7 |
| 98 | **BL2.1.2.2** | (Decision Tree) Default Params + Class Weight + Platt Scaling | **0.2995** | 0.6779 | 0.0837 | 60584 | 60583.1 |
| 99 | **BL2.1.4.3** | (Decision Tree) Default Params + Oversampling + Isotonic Regression | **0.2996** | 0.6755 | 0.0839 | 60584 | 60583.5 |
| 100 | **BL3.1.1.2** | (Random Forest) Default Params + No Weights + Platt Scaling | **0.3000** | 0.6724 | 0.0834 | 60584 | 60619.2 |
| 101 | **BL3.1.3.2** | (Random Forest) Default Params + Undersampling + Platt Scaling | **0.3007** | 0.6823 | 0.0842 | 60584 | 60590.3 |
| 102 | **BL2.1.1.2** | (Decision Tree) Default Params + No Weights + Platt Scaling | **0.3008** | 0.6778 | 0.0842 | 60584 | 60587.2 |
| 103 | **BL2.1.4.2** | (Decision Tree) Default Params + Oversampling + Platt Scaling | **0.3010** | 0.6751 | 0.0843 | 60584 | 60543.3 |
| 104 | **BL3.1.2.2** | (Random Forest) Default Params + Class Weight + Platt Scaling | **0.3012** | 0.6777 | 0.0842 | 60584 | 60581.6 |
| 105 | **BL3.1.4.3** | (Random Forest) Default Params + Oversampling + Isotonic Regression | **0.3014** | 0.6639 | 0.0840 | 60584 | 60583.7 |
| 106 | **BL3.1.4.2** | (Random Forest) Default Params + Oversampling + Platt Scaling | **0.3025** | 0.6640 | 0.0843 | 60584 | 60584.7 |
| 107 | **BL2.1.3.3** | (Decision Tree) Default Params + Undersampling + Isotonic Regression | **0.3054** | 0.6479 | 0.0855 | 60584 | 60584.6 |
| 108 | **BL2.1.3.2** | (Decision Tree) Default Params + Undersampling + Platt Scaling | **0.3075** | 0.6407 | 0.0860 | 60584 | 60619.6 |
| 109 | **BL4.2.4.1** | (LightBGM) Tuned Params + Oversampling + No Calibration | **0.5128** | 0.6974 | 0.1724 | 60584 | 201801.6 |
| 110 | **BL5.2.2.1** | (XGBoost) Tuned Params + Class Weight + No Calibration | **0.5236** | 0.7158 | 0.1801 | 60584 | 209144.4 |
| 111 | **BL4.2.2.1** | (LightBGM) Tuned Params + Class Weight + No Calibration | **0.5379** | 0.7169 | 0.1869 | 60584 | 217466.9 |
| 112 | **BL6.2.2.1** | (MLP) Tuned Params + Class Weight + No Calibration | **0.5424** | 0.7437 | 0.1829 | 60584 | 245683.9 |
| 113 | **BL6.1.2.1** | (MLP) Default Params + Class Weight + No Calibration | **0.5529** | 0.7441 | 0.1883 | 60584 | 249339.1 |
| 114 | **BL5.1.4.1** | (XGBoost) Default Params + Oversampling + No Calibration | **0.5702** | 0.7412 | 0.1972 | 60584 | 251566.5 |
| 115 | **BL5.2.4.1** | (XGBoost) Tuned Params + Oversampling + No Calibration | **0.5702** | 0.7412 | 0.1972 | 60584 | 251566.5 |
| 116 | **BL5.1.2.1** | (XGBoost) Default Params + Class Weight + No Calibration | **0.5784** | 0.7464 | 0.1988 | 60584 | 258528.6 |
| 117 | **BL4.1.4.1** | (LightBGM) Default Params + Oversampling + No Calibration | **0.5791** | 0.7433 | 0.2007 | 60584 | 257185.6 |
| 118 | **BL3.2.4.1** | (Random Forest) Tuned Params + Oversampling + No Calibration | **0.5818** | 0.7439 | 0.2024 | 60584 | 257641.2 |
| 119 | **BL3.2.2.1** | (Random Forest) Tuned Params + Class Weight + No Calibration | **0.5839** | 0.7455 | 0.2007 | 60584 | 261787.9 |
| 120 | **BL4.1.2.1** | (LightBGM) Default Params + Class Weight + No Calibration | **0.5853** | 0.7466 | 0.2012 | 60584 | 262783.2 |
| 121 | **BL4.2.3.1** | (LightBGM) Tuned Params + Undersampling + No Calibration | **0.5869** | 0.7488 | 0.2015 | 60584 | 263229.3 |
| 122 | **BL4.1.3.1** | (LightBGM) Default Params + Undersampling + No Calibration | **0.5883** | 0.7470 | 0.2022 | 60584 | 263934.8 |
| 123 | **BL2.2.2.1** | (Decision Tree) Tuned Params + Class Weight + No Calibration | **0.5892** | 0.7433 | 0.2028 | 60584 | 263928.9 |
| 124 | **BL3.2.3.1** | (Random Forest) Tuned Params + Undersampling + No Calibration | **0.5899** | 0.7449 | 0.2030 | 60584 | 264110.3 |
| 125 | **BL5.1.3.1** | (XGBoost) Default Params + Undersampling + No Calibration | **0.5901** | 0.7456 | 0.2029 | 60584 | 262866.9 |
| 126 | **BL5.2.3.1** | (XGBoost) Tuned Params + Undersampling + No Calibration | **0.5901** | 0.7456 | 0.2029 | 60584 | 262866.9 |
| 127 | **BL2.2.3.1** | (Decision Tree) Tuned Params + Undersampling + No Calibration | **0.5914** | 0.7422 | 0.2036 | 60584 | 264621.3 |
| 128 | **BL1.2.2.1** | (LogRegr) Tuned Params + Class Weight + No Calibration | **0.5947** | 0.7409 | 0.2028 | 60584 | 267628.3 |
| 129 | **BL1.1.2.1** | (LogRegr) Default Params + Class Weight + No Calibration | **0.5947** | 0.7408 | 0.2029 | 60584 | 267265.3 |
| 130 | **BL1.2.3.1** | (LogRegr) Tuned Params + Undersampling + No Calibration | **0.5949** | 0.7410 | 0.2028 | 60584 | 268171.9 |
| 131 | **BL1.1.3.1** | (LogRegr) Default Params + Undersampling + No Calibration | **0.5950** | 0.7408 | 0.2030 | 60584 | 267283.3 |
| 132 | **BL2.2.4.1** | (Decision Tree) Tuned Params + Oversampling + No Calibration | **0.5991** | 0.7424 | 0.2102 | 60584 | 265241.1 |
| 133 | **BL1.1.4.1** | (LogRegr) Default Params + Oversampling + No Calibration | **0.6061** | 0.7395 | 0.2096 | 60584 | 270332.6 |
| 134 | **BL1.2.4.1** | (LogRegr) Tuned Params + Oversampling + No Calibration | **0.6061** | 0.7395 | 0.2096 | 60584 | 270553.4 |
| 135 | **BL6.2.3.1** | (MLP) Tuned Params + Undersampling + No Calibration | **0.6117** | 0.7436 | 0.2120 | 60584 | 276242.0 |
| 136 | **BL6.1.3.1** | (MLP) Default Params + Undersampling + No Calibration | **0.6603** | 0.7429 | 0.2298 | 60584 | 296409.0 |
| 137 | **BL6.2.4.1** | (MLP) Tuned Params + Oversampling + No Calibration | **0.8388** | 0.7311 | 0.3038 | 60584 | 348690.3 |
| 138 | **BL3.1.1.1** | (Random Forest) Default Params + No Weights + No Calibration | **1.0268** | 0.6729 | 0.0916 | 60584 | 60966.6 |
| 139 | **BL3.1.4.1** | (Random Forest) Default Params + Oversampling + No Calibration | **1.0820** | 0.6643 | 0.1287 | 60584 | 108269.0 |
| 140 | **BL3.1.2.1** | (Random Forest) Default Params + Class Weight + No Calibration | **1.1075** | 0.6778 | 0.1896 | 60584 | 170772.3 |
| 141 | **BL3.1.3.1** | (Random Forest) Default Params + Undersampling + No Calibration | **1.1262** | 0.6823 | 0.2522 | 60584 | 263208.2 |
| 142 | **BL2.1.1.1** | (Decision Tree) Default Params + No Weights + No Calibration | **1.4618** | 0.6782 | 0.0921 | 60584 | 60739.5 |
| 143 | **BL2.1.2.1** | (Decision Tree) Default Params + Class Weight + No Calibration | **1.7091** | 0.6787 | 0.1993 | 60584 | 182124.9 |
| 144 | **BL2.1.4.1** | (Decision Tree) Default Params + Oversampling + No Calibration | **2.1087** | 0.6758 | 0.1493 | 60584 | 123406.2 |
| 145 | **BL6.1.4.1** | (MLP) Default Params + Oversampling + No Calibration | **7.4103** | 0.6307 | 0.8992 | 60584 | 621398.2 |
| 146 | **BL2.1.3.1** | (Decision Tree) Default Params + Undersampling + No Calibration | **7.8222** | 0.6414 | 0.3185 | 60584 | 260062.1 |