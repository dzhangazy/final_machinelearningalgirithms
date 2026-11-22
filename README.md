# Astana Apartment Price Prediction — Final Exam Project

- **Course:** Machine Learning Algorithms
- **Format:** Group presentation
- **Deadline:** 23 November 2026
- **Team:** Nurkhan Kuanyshov and Bakytzhan Jangazy, Group SE-2431

## Project question and objective

Can the asking price of an Astana apartment be predicted from its area, layout,
location, floor, building characteristics, and advertised amenities?

This is a **regression** task. The original target is `price` in KZT; the
supervised models learn `log1p(price)`. The practical objective is to produce a
reproducible estimate for decision support, not an official appraisal or a
causal estimate. The primary model-selection metric is validation RMSE on the
log-price scale; MAE, R², KZT MAE, and median absolute percentage error
(MedAPE) provide complementary evaluation.

## Dataset and provenance

- **Dataset:** [Astana Real Estate Dataset by turarr on Kaggle](https://www.kaggle.com/datasets/turarr/astana-real-estate-dataset)
- **Source and acquisition:** a Kaggle-hosted collection of Krisha.kz
  advertisements, loaded from an attached Kaggle input, an optional local
  `astana_real_estate_dataset.csv`, or KaggleHub. No synthetic data is used.
- **Raw size:** 5,003 listings and 23 columns, including the `price` target.
  Predictors cover area, rooms, current and total floors, district/residential
  complex, building and amenity categories, ceiling height, construction year,
  and available coordinates.
- **Cleaning:** 834 exact duplicates and 83 further records failing range or
  target checks were removed, leaving 4,086 listings. The model target is
  restricted to the raw sample's 1st–99th price percentiles. The original
  recorded CSV had no blank cells; invalid values can become missing during
  range validation and are imputed inside training-fitted pipelines.
- **Licence and caveats:** Kaggle's API record lists CC0, but this is a
  third-party collection of Krisha.kz content, not an official government
  dataset. The Kaggle label does not necessarily resolve upstream collection
  or redistribution terms. Listings are seller-entered asking prices, not
  verified transactions; duplicates, errors, incomplete fields, and
  time-specific coverage are known limitations. See [DATA.md](DATA.md) and
  [data/README.md](data/README.md) before redistribution.

## Team contributions and defense participation

| Member | Identifiable technical contribution |
| --- | --- |
| Nurkhan Kuanyshov | Dataset preparation, EDA, cleaning, feature engineering, leakage-safe split and preprocessing, midterm baselines and KNN/tree tuning, and K-Means. |
| Bakytzhan Jangazy | Random Forest and Gradient Boosting tuning, PCA, ANN, final evaluation, error analysis, model comparison, and report/presentation preparation. |

Both members speak during the defense and must be prepared to explain and
answer questions about the entire project. The deck's speaker plan is:
Nurkhan — slides 2–4, 7–8, and 12; Bakytzhan — slides 5–6 and 9–11; both —
slides 1 and 13–15. Adjust timing to fit the 12-minute talk.

## Final deliverables

- [Endterm executable notebook](notebooks/astana_housing_endterm.ipynb)
- [Written report and research proposal](Final_Project_Report.docx)
- [15-slide presentation](Astana_Housing_Endterm_Presentation.pptx)
- [Final comparison of all supervised models](reports/final_model_comparison.csv)
- [Five-fold tuning results and selected parameters](reports/model_tuning_cv.csv)
- [Final selected-model test metrics](reports/final_test_metrics.csv)
- [Defense questions and result-specific prompts](reports/defense_questions.md)
- [Dataset acquisition and licence notes](DATA.md)

The original midterm notebook, [astana_housing_midterm.ipynb](astana_housing_midterm.ipynb),
is preserved. The older `Astana Apartment Price Prediction — Endterm.pdf` is a
draft with outdated results and clusters; use the current PPTX and DOCX above.

## Coverage of the final-project criteria

<<<<<<< HEAD
```text
astana_housing_midterm.ipynb              # preserved original work
notebooks/astana_housing_ final.ipynb     # final-exam notebook
scripts/build_endterm_notebook.py         # version-controlled notebook source
visualizations/{eda,clustering,pca,ann,evaluation}/
models/                                   # created by a successful run
reports/endterm_analysis.md
reports/defense_questions.md
data/README.md
```
=======
### Data preparation and exploratory analysis
>>>>>>> 795e882 (updated README)

- EDA includes five plots: raw/log price distributions, area versus price,
  district price distributions, feature correlations, and missingness. The
  report interprets patterns and notes limitations for each.
- Cleaning documents types, duplicates, ranges, target trimming, floors, dates
  and invalid values. Engineered predictors include house age, relative floor,
  first/top-floor indicators, and area per room; location-derived values are
  used only when coordinates are available.
- A fixed-seed 70/15/15 train/validation/test split is stratified by log-price
  quantile bins. Imputation, scaling, and encoding are fitted inside
  training-only pipelines.

### Supervised learning and model evaluation

- The median baseline is retained and compared with Linear Regression,
  Decision Tree, KNN, Random Forest, Gradient Boosting, and a feedforward ANN.
- Five-fold training-only cross-validation is used for model tuning.
  Grid search is used for selected tree/KNN models and bounded randomized
  search for Random Forest and Gradient Boosting. The tuning table records
  CV scores, fold variation, and selected parameters.
- Validation RMSE(log) selects the model. After the choice is locked, every
  supervised model is refit on train plus validation and evaluated on the same
  held-out test set. Test results do not change the selection.

### Clustering, PCA, and neural network

- K-Means is additional exploratory analysis, not a substitute for supervised
  prediction. Features are scaled; inertia and silhouette evidence support
  **k = 5** (silhouette **0.277**). Price is used only for exploratory market
  segmentation, never as a prediction input. The report interprets the groups.
- PCA is fit to standardized training numerical features. The first two
  components explain **33.8%** of variance; **9** and **11** components retain
  90% and 95%, respectively.
- The tabular ANN uses 128–64–32 ReLU hidden layers, a linear output, Adam and
  MSE, with early stopping and learning-rate reduction. Its training and
  validation history is plotted and discussed; its test result is included in
  the same final comparison.

### Research, reporting, and defense

- The DOCX contains the study, related work, methods, results, the common
  comparison table, error analysis, limitations, practical and ethical
  considerations, a research proposal, contribution record, and AI-use
  disclosure.
- The bibliography contains 30 APA-style entries. In-text citations and
  bibliography entries were cross-checked; dataset, method, and software
  sources are identified.
- The report is intended to meet the required minimum of four study pages
  (excluding title page, abstract, conclusion, and bibliography) and a
  separate 2–3 page proposal. **Before submission, render/open the DOCX and
  confirm the actual page counts and that tables/figures are not clipped.**
- The presentation has 15 slides for a 12-minute talk, with results,
  limitations, contributions, and Q&A. Every member should be ready to
  discuss the whole study.

## Results

Gradient Boosting was selected using validation RMSE(log) **0.1644**. On the
held-out test set it achieved RMSE(log) **0.1678**, MAE(log) **0.1263**, R²
**0.8917**, MAE **6.18 million KZT**, and MedAPE **9.45%**. The validation
MedAPE was **9.58%**, compared with **33.93%** for the median baseline.

Random Forest had a slightly lower test RMSE(log) (**0.1677**) and test MAE,
but Gradient Boosting remains selected because the predeclared rule uses
validation RMSE; test results are descriptive and were not used to select the
winner. The ANN achieved validation RMSE(log) **0.1697** and test RMSE(log)
**0.1867** (test MedAPE **11.39%**). K-Means selected five clusters, and the
first two PCA components explain 33.8% of numerical-feature variance. Full
per-model results are in [final_model_comparison.csv](reports/final_model_comparison.csv).

## Setup and execution order

The recorded successful run used Python **3.14.10**, pandas **3.1.1**, NumPy
**2.5.4**, scikit-learn **1.9.1**, Matplotlib **3.11.2**, seaborn **0.13.2**,
joblib **1.6.0**, TensorFlow **2.22.0-rc0**, python-docx **1.2.0**, and
python-pptx **1.0.2**. The notebook prints analysis-library versions at
runtime. `requirements.txt` contains minimum versions, not a complete lockfile,
so a fresh install may resolve differently.

From the project root:

```bash
python3.14 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python scripts/build_endterm_notebook.py
python scripts/verify_endterm_notebook.py
python scripts/build_final_report.py
python -m scripts.build_endterm_presentation
```

The notebook builder creates the notebook; the verifier executes it from top
to bottom and validates the notebook. A successful run downloads the public
dataset if necessary and creates the result CSVs, figures, and selected model
artifact. The first download outside Kaggle requires internet access.
KaggleHub caches downloaded data outside the repository. The report and slide
builders should be run after the notebook so they use the latest outputs.

## Limitations, ethics, and AI disclosure

<<<<<<< HEAD
The endterm notebook completed successfully on the supplied CSV. Gradient
Boosting was selected on validation RMSE(log) and achieved held-out test
RMSE(log) **0.1672**, MAE(log) **0.1254**, R² **0.8924**, MAE **6.10M KZT**,
and MedAPE **9.18%**. The validation MedAPE was 9.42%, compared with 33.93% for
the median baseline. The ANN was evaluated on the same validation split but did
not improve on Gradient Boosting (RMSE(log) 0.1708 versus 0.1624). Historical
midterm metrics remain in the original notebook and are not substituted for
endterm results.

| Validation model | RMSE (log) | R² | MAE (KZT) | MedAPE |
| --- | ---: | ---: | ---: | ---: |
| Gradient Boosting | 0.162 | 0.893 | 5.80M | 9.42% |
| Random Forest | 0.170 | 0.884 | 6.01M | 9.78% |
| Artificial Neural Network | 0.171 | 0.882 | 6.23M | 9.70% |
| Linear Regression | 0.209 | 0.823 | 7.68M | 11.93% |
| Decision Tree (tuned) | 0.219 | 0.806 | 8.34M | 13.33% |
| KNN (tuned) | 0.240 | 0.766 | 8.32M | 13.11% |
| Median baseline | 0.500 | -0.013 | 17.92M | 33.93% |

K-Means selected **k = 5** with silhouette **0.277**. PCA retained **33.8%**
of variance in two components, and required **9** and **11** components for
about 90% and 95% variance, respectively. The ANN uses training-only scaling
for both processed inputs and the log-price target, then inverse-transforms
predictions before common metric calculation. It is competitive with Random
Forest but does not justify replacing Gradient Boosting.

## Limitations and ethics

Listing prices are not verified transactions. Advertisements can be duplicated, inaccurate, incomplete, and time-specific; premium homes can depend on unrecorded renovation, view or building details. Accuracy can vary by district and price segment. Feature importance is association, not causation, and outputs must not be treated as official appraisals.
=======
The sample represents asking prices in one time-specific collection, not
completed transactions. Target trimming excludes some valid low- and
high-priced listings. Errors can differ by district and price segment.
Feature importance and model associations are not causal effects. Do not use
these predictions as official appraisals, credit decisions, or eligibility
decisions; verify upstream data terms and avoid publishing identifiable
listing details.


>>>>>>> 795e882 (updated README)
