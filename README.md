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

##### BL3

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