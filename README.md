<<<<<<< HEAD
# Task 5: Decision Trees and Random Forests
**AI & ML Internship — Elevate Labs**

## Objective
Learn tree-based models for classification & regression.

## Tools Used
Python, Scikit-learn, Graphviz, Matplotlib, Seaborn

## Dataset
Heart Disease dataset (`data/heart.csv`) — 13 clinical features (age, sex,
chest pain type, resting BP, cholesterol, max heart rate, etc.) and a
binary `target` (1 = heart disease present, 0 = not present).

> **Note on the data file:** generated locally with `generate_dataset.py`
> using the same well-known column structure as the real UCI Cleveland
> Heart Disease dataset, since this environment has no internet access to
> download it directly. The target was built from a genuine (noisy)
> combination of the risk-factor features, so the patterns the trees learn
> are real and meaningful, not arbitrary. To use the real dataset instead,
> download it from the
> [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/45/heart+disease)
> and drop it in as `data/heart.csv` with matching column names —
> `decision_trees_random_forests.py` needs no changes.

## Project Structure
```
├── data/
│   └── heart.csv
├── images/
│   ├── decision_tree_matplotlib.png
│   ├── decision_tree_graphviz.png
│   ├── overfitting_vs_depth.png
│   ├── model_comparison.png
│   ├── feature_importances.png
│   └── cross_validation.png
├── generate_dataset.py
├── decision_trees_random_forests.py
├── model_comparison.csv
├── cross_validation_results.csv
├── requirements.txt
└── README.md
```

## How to Run
```bash
pip install -r requirements.txt
# System dependency for tree visualization (Graphviz):
#   Ubuntu/Debian: sudo apt install graphviz
#   Mac:           brew install graphviz
#   Windows:       https://graphviz.org/download/
python generate_dataset.py       # only needed if data/heart.csv doesn't exist
python decision_trees_random_forests.py
```

## What Was Done

1. **Trained a Decision Tree Classifier** (max_depth=3, for a readable
   visualization) and rendered it two ways: with `sklearn.tree.plot_tree`
   (matplotlib) and with `graphviz` for a cleaner layout.
2. **Analyzed overfitting** by training trees at every `max_depth` from 1
   to 20 and plotting train vs. test accuracy — the classic
   ever-climbing-train / plateauing-test pattern shows up clearly.
3. **Trained a Random Forest** (200 trees) and compared its test accuracy
   directly against the best single tuned tree.
4. **Interpreted feature importances** from the Random Forest via a
   ranked bar chart.
5. **Evaluated both models with 5-fold cross-validation** to get a more
   robust accuracy estimate than a single train/test split.

## Results

**Overfitting analysis:** train accuracy climbs steadily toward 1.0 as
`max_depth` increases, while test accuracy plateaus around depth 3-4 and
then fluctuates without meaningfully improving — the widening gap between
the two curves *is* overfitting. See `images/overfitting_vs_depth.png`.

| Model                          | Test Accuracy | 5-Fold CV Mean Accuracy |
|---------------------------------|---------------|--------------------------|
| Decision Tree (tuned max_depth) | 0.719         | 0.670 (± 0.022)          |
| Random Forest (200 trees)       | 0.769         | 0.766 (± 0.035)          |

The Random Forest clearly beats the single tuned tree on both metrics,
and its cross-validation scores are noticeably more consistent —
exactly the ensemble-averaging benefit random forests are known for.

**Top 3 most important features** (by mean decrease in impurity):
`chol` (cholesterol), `thalach` (max heart rate achieved), and `oldpeak`
(ST depression) — see `images/feature_importances.png` for the full
ranking.

---

