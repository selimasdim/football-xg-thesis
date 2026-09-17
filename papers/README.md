* **Αλγόριθμοι (5):** Logistic Regression, Decision Tree, Random Forest, XGBoost, LightGBM (αφήνω προς το παρόν εκτος τα νευρωνικά δίκτυα, αλλα μήπως μπει adaboost ή catboost)
* **Παράμετροι (2):** Default parameters, Hyperparameters
* **Imbalance (4):** None, Class Weight, Undersampling, Oversampling
* **Calibration (2):** None (δεν χρειαζεται να εκπαιδεύσουμε μοντέλο για το None), Platt Scaling, Isotonic Regression 

#### Feature Tiers: 
* **Baseline** (Features: x, y)
* **Baseline+** (Features: x, y, extra shot features)
* **Enhanced** (Features: x, y, Football Manager Features)
* **Enhanced+** (Features: x, y, extra shot features, Football Manager Features)


#### Datasets:
* Combined Dataset (660k shots): Combine και τα 2 dataset σε 1 (normalized και limited features σε ολα τα shots) => Train on all feature tiers (πλήθος: 4)
* StatsBomb Dataset (~60k shots): Χρησιμοποίηση του rich dataset στα tiers **Baseline+** και **Enhanced+** για να κάνουμε και σύγκριση στα datasets (πόσο % καλύτερο είναι το rich από το common-limited) (πλήθος: 2)

---

## Προσέγγιση 1: Factorial

δεν χρειαζεται να δουμε το None, είναι το αποτέλεσμα του προηγούμενου βήματος

$$\text{Παραλλαγές ανά Αλγόριθμο} = 2 \text{ (Params)} \times 4 \text{ (Imbalance)} \times 2 \text{ (Calibration)} = \mathbf{16 \text{ παραλλαγές}}$$
$$\text{Μοντέλα ανά Feature Tier} = 5 \text{ (Αλγόριθμοι)} \times 16 = \mathbf{80 \text{ μοντέλα}}$$


#### Συνολικό Πλήθος Εκπαιδεύσεων:
* **Combined Dataset:** $4 \times 80 = \mathbf{320 \text{ μοντέλα}}$
* **StatsBomb-only Dataset:** $2 \times 80 = \mathbf{160 \text{ μοντέλα}}$
* **Σύνολο Full Factorial:** $320 + 160 = \mathbf{480 \text{ μοντέλα}}$

---

### 1.1 Model Naming


$$\mathbf{[Tier][Algorithm].[Parameters].[Imbalance].[Calibration]}$$

#### Encoding Scheme:
* **Parameters:** `.1` = Default, `.2` = Tuned
* **Imbalance:** `.1` = None, `.2` = Class Weight, `.3` = Undersampling, `.4` = Oversampling
* **Calibration:** `.1` = None, `.2` = Platt, `.3` = Isotonic

πχ. 
#### BL: Baseline (Features: x, y)
* **BL1: Logistic Regression (24 εκδοχές)**
  * *Default Parameters (.1):*
    * `BL1.1.1.1`: Default + None Imbalance + None Calibration
    * `BL1.1.1.2`: Default + None Imbalance + Platt Calibration
    * `BL1.1.1.3`: Default + None Imbalance + Isotonic Calibration
    * `BL1.1.2.1`: Default + Class Weight + None Calibration
    * `BL1.1.2.2`: Default + Class Weight + Platt Calibration
    * `BL1.1.2.3`: Default + Class Weight + Isotonic Calibration
    * `BL1.1.3.1`: Default + Undersampling + None Calibration
    * `BL1.1.3.2`: Default + Undersampling + Platt Calibration
    * `BL1.1.3.3`: Default + Undersampling + Isotonic Calibration
    * `BL1.1.4.1`: Default + Oversampling + None Calibration
    * `BL1.1.4.2`: Default + Oversampling + Platt Calibration
    * `BL1.1.4.3`: Default + Oversampling + Isotonic Calibration
  * *Tuned Parameters (.2):*
    * `BL1.2.1.1`: Tuned + None Imbalance + None Calibration
    * `BL1.2.1.2`: Tuned + None Imbalance + Platt Calibration
    * `BL1.2.1.3`: Tuned + None Imbalance + Isotonic Calibration
    * `BL1.2.2.1`: Tuned + Class Weight + None Calibration
    * `BL1.2.2.2`: Tuned + Class Weight + Platt Calibration
    * `BL1.2.2.3`: Tuned + Class Weight + Isotonic Calibration
    * `BL1.2.3.1`: Tuned + Undersampling + None Calibration
    * `BL1.2.3.2`: Tuned + Undersampling + Platt Calibration
    * `BL1.2.3.3`: Tuned + Undersampling + Isotonic Calibration
    * `BL1.2.4.1`: Tuned + Oversampling + None Calibration
    * `BL1.2.4.2`: Tuned + Oversampling + Platt Calibration
    * `BL1.2.4.3`: Tuned + Oversampling + Isotonic Calibration


και ούτω καθ' έξης για τα επόμενα

---

### Αξιολόγηση Full Factorial
* **Πλεονεκτήματα:**
  * Βέλτιστο αποτέλεσμα
* **Μειονεκτήματα:**
  *  "Άπειρος" χρόνος

---
## Προσέγγιση Β

Μπορούμε να κάνουμε calibration ΜΟΝΟ στα βελτιστα 2 μοντελα ανά feature tier και όχι σε ολα

$$\text{Παραλλαγές ανά Αλγόριθμο} = 2 \text{ (Params)} \times 4 \text{ (Imbalance)} = \mathbf{8 \text{ παραλλαγές}}$$
$$\text{Μοντέλα ανά Feature Tier} = 5 \text{ (Αλγόριθμοι)} \times 8 = \mathbf{40 \text{ μοντέλα}}$$
$$\text{Μοντέλα μετά το Calibration στα top 2} = 2 \text{ (Μοντέλα)} \times 2 \text{ (Calibration)} = \mathbf{4 \text{ μοντέλα}}$$

$$ \text{Σύνολο ανά feature tier} = 5 \text{ (Αλγόριθμοι)} \times 2 \text{ (Params)} \times 4 \text{ (Imbalance)}  +  2 \text{ (Μοντέλα)} \times 2 \text{ (Calibration)} = \mathbf{44 \text{ μοντέλα}}$$


#### Συνολικό Πλήθος Εκπαιδεύσεων:
* **Combined Dataset:** $4 \times 44 = \mathbf{176 \text{ μοντέλα}}$
* **StatsBomb-only Dataset:** $2 \times 44 = \mathbf{88 \text{ μοντέλα}}$
* **Σύνολο:** $176 + 88 = \mathbf{264\text{ μοντέλα}}$
---
### Αξιολόγηση 
* **Πλεονεκτήματα:**
  * Σχεδόν ίδιο αποτέλεσμα, μισή δουλειά
* **Μειονεκτήματα:**
  *  Ακόμα ο χρόνος παραμένει υπερβολικος ;
---
## Προσέγγιση 3

Για κάθε feature tier, να κόψουμε τα:
* Default parameters
---

$$\text{Παραλλαγές ανά Αλγόριθμο} = 1 \text{ (Params)} \times 4 \text{ (Imbalance)} = \mathbf{4 \text{ παραλλαγές}}$$
$$\text{Μοντέλα ανά Feature Tier} = 5 \text{ (Αλγόριθμοι)} \times 4 = \mathbf{20 \text{ μοντέλα}}$$
$$\text{Μοντέλα μετά το Calibration στα top 2} = 2 \text{ (Μοντέλα)} \times 2 \text{ (Calibration)} = \mathbf{4 \text{ μοντέλα}}$$

$$ \text{Σύνολο ανά feature tier} = 5 \text{ (Αλγόριθμοι)} \times 1 \text{ (Params)} \times 4 \text{ (Imbalance)}  +  2 \text{ (Μοντέλα)} \times 2 \text{ (Calibration)} = \mathbf{24 \text{ μοντέλα}}$$

#### Συνολικό Πλήθος Εκπαιδεύσεων:
* **Combined Dataset:** $4 \times 24 = \mathbf{96 \text{ μοντέλα}}$
* **StatsBomb-only Dataset:** $2 \times 24 = \mathbf{48 \text{ μοντέλα}}$
* **Σύνολο:** $96+ 48 = \mathbf{144\text{ μοντέλα}}$

## Προσέγγιση 4: Decoupled Stage-Gated

Για κάθε feature tier, να κόψουμε τα:
* Default parameters
* Πλήρες grid στο Imbalance (δοκιμή όλων ΜΟΝΟ στο Baseline feature tier και επιλογή της βέλτιστης για τα επόμενα feature tiers)
---

### Βήμα 1: Baseline Tier (Imbalance Screening)
$$\text{Παραλλαγές ανά Αλγόριθμο} = 1 \text{ (Params)} \times 4 \text{ (Imbalance)} = \mathbf{4 \text{ παραλλαγές}}$$
$$\text{Μοντέλα στο Baseline Tier} = 5 \text{ (Αλγόριθμοι)} \times 4 = \mathbf{20 \text{ μοντέλα}}$$
$$\text{Μοντέλα μετά το Calibration στα top 2} = 2 \text{ (Μοντέλα)} \times 2 \text{ (Calibration)} = \mathbf{4 \text{ μοντέλα}}$$

$$ \text{Σύνολο για το Baseline Tier} = 5 \text{ (Αλγόριθμοι)} \times 1 \text{ (Params)} \times 4 \text{ (Imbalance)}  +  2 \text{ (Μοντέλα)} \times 2 \text{ (Calibration)} = \mathbf{24 \text{ μοντέλα}}$$

### Βήμα 2: Υπόλοιπα Feature Tiers (με την 1 βέλτιστη μέθοδο Imbalance)
$$\text{Παραλλαγές ανά Αλγόριθμο} = 1 \text{ (Params)} \times 1 \text{ (Best Imbalance)} = \mathbf{1 \text{ παραλλαγή}}$$
$$\text{Μοντέλα ανά Feature Tier} = 5 \text{ (Αλγόριθμοι)} \times 1 = \mathbf{5 \text{ μοντέλα}}$$
$$\text{Μοντέλα μετά το Calibration στα top 2} = 2 \text{ (Μοντέλα)} \times 2 \text{ (Calibration)} = \mathbf{4 \text{ μοντέλα}}$$

$$ \text{Σύνολο ανά υπόλοιπο feature tier} = 5 \text{ (Αλγόριθμοι)} \times 1 \text{ (Params)} \times 1 \text{ (Imbalance)}  +  2 \text{ (Μοντέλα)} \times 2 \text{ (Calibration)} = \mathbf{9 \text{ μοντέλα}}$$

#### Συνολικό Πλήθος Εκπαιδεύσεων:
* **Combined Dataset:** $24 \text{ (Baseline)} + (3 \text{ Tiers} \times 9) = 24 + 27 = \mathbf{51 \text{ μοντέλα}}$
* **StatsBomb-only Dataset:** $2 \text{ Tiers} \times 9 = \mathbf{18 \text{ μοντέλα}}$
* **Σύνολο:** $51 + 18 = \mathbf{69\text{ μοντέλα}}$

---
### Αξιολόγηση 
* **Πλεονεκτήματα:**
  * Ακριβώς ίδιο αποτέλεσμα με την προσέγγιση 2 (τα default parameters, δεν θα είναι ποτέ τα βέλτιστα), μισή δουλεια από την Β
* **Μειονεκτήματα:**
  *  Χάνουμε στο ablation study ;;
---

## Συγκριτικός Πίνακας Προσεγγίσεων
| Προσέγγιση | Ανά Tier | Combined Dataset (4 Tiers) | StatsBomb Dataset (2 Tiers) | Υπολογισμός Συνόλου Datasets | Γενικό Σύνολο Εκπαιδεύσεων |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Α** | 80 | 320 | 160 | 320 + 160 | **480** |
| **Β** | 44 | 176 | 88 | 176 + 88 | **264** |
| **Γ** | 24 | 96 | 48 | 96 + 48 | **144** |
| **Δ** | 24 (Tier 1) / 9 (Tiers 2-4) | 51 | 18 | 51 + 18 | **69** |


### Σημειώσεις

Ψάχνοντας κάποια σχετικά papers είδα πως:
* για oversampling κυριαρχούν 2 τεχνικες (Random Over-Sampling Examples και Synthetic Minority Over-sampling Technique)
* Το undersampling που θεωρητικά φαίνεται ευκολότερο, πιθανότατα θα πρέπει cluster-based ώστε να διατηρηθεί η ίδια αναλογία των αποστάσεων στα οχι-γκολ σουτ
* Calibration είναι must μετά απο over-under sampling
* Υπάρχει η τάση σε κάποια από αυτά, να μην διαχειρίζονται κάπως το imbalance, αλλά να χρησιμοποιούν Stratified Splitting & Stratified K-Fold Cross-Validation, ώστε να διατηρηθεί η αναλογία 1:9 σε όλα τα splits
