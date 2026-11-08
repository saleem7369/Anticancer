# Explainable Machine Learning-Assisted Quantitative Prediction of Anticancer Activity in Azo-Schiff Base Metal Complexes

## Overview

This research presents an explainable machine-learning framework for quantitative prediction of the anticancer activity of Azo-Schiff base metal complexes. The prediction target is the experimentally reported half-maximal inhibitory concentration (**IC₅₀**, in µM). The framework combines chemical composition, quantum-chemical descriptors, and molecular/physicochemical descriptors with regression models and explainable AI (XAI). The study evaluates five regression algorithms---Random Forest, XGBoost, CatBoost, LightGBM, and Support Vector Regression (SVR)---using a 70:30 training/testing split. CatBoost achieved the strongest reported test-set performance, with an R² of 0.8933, MAE of 5.3840, RMSE of 6.5548, and MAPE of 7.0627%. SHAP and LIME are used to interpret model behaviour globally and for individual predictions. The explanations identify influential model features and should be understood as model associations, not proof of causal or biological mechanisms.

## Research Objectives

-   Predict continuous IC₅₀ values for Azo-Schiff base metal complexes.
-   Integrate quantum-chemical and molecular descriptors into a
    machine-learning regression framework.
-   Compare the predictive performance of five regression algorithms.
-   Use SHAP to examine global feature contributions.
-   Use LIME and local SHAP explanations to interpret individual
    predictions.
-   Provide a preliminary computational approach for prioritizing
    compounds for further experimental evaluation.

## Dataset

The paper describes a dataset of **350 records** and **15 attributes**,
representing:

-   **8 metal categories:** Ni, Cu, Pd, Zn, Pt, Ru, Co, and Fe.
-   **6 ligand categories.**
-   **7 cancer cell lines.**
-   **IC₅₀ range:** 39.06--135.80 µM.
-   **Mean IC₅₀:** 86.62 µM.

### Dataset fields

  -----------------------------------------------------------------------
  Feature                             Description
  ----------------------------------- -----------------------------------
  Compound ID                         Identifier assigned to each dataset
                                      record

  Ligand                              Azo-Schiff base ligand category

  Metal                               Metal centre (Ni, Cu, Pd, Zn, Pt,
                                      Ru, Co, Fe)

  Cell line                           Cancer cell line used to
                                      characterize activity

  IC₅₀ (µM)                           Target variable for anticancer
                                      activity prediction

  DOI                                 Source-reference field; the paper
                                      notes that provenance requires
                                      verification

  HOMO                                Highest occupied molecular orbital
                                      energy

  LUMO                                Lowest unoccupied molecular orbital
                                      energy

  Gap                                 HOMO--LUMO energy gap

  Dipole                              Molecular dipole moment

  Hardness                            Chemical hardness

  Softness                            Chemical softness

  LogP                                Lipophilicity descriptor

  TPSA                                Topological polar surface area

  MW                                  Molecular weight
  -----------------------------------------------------------------------

## Quantum-Chemical and Molecular Descriptors

The model uses chemical composition and electronic/physicochemical
information, including:

-   Ligand type and metal centre.
-   Cancer cell line.
-   HOMO and LUMO energies.
-   HOMO--LUMO energy gap.
-   Dipole moment.
-   Chemical hardness and softness.
-   LogP.
-   Topological polar surface area (TPSA).
-   Molecular weight (MW).
-   Derived conceptual-DFT descriptors discussed in the article,
    including ionization potential, electron affinity,
    electronegativity, chemical potential, and electrophilicity index.

The paper notes substantial relationships among some derived
descriptors. For example, HOMO and ionization potential, LUMO and
electron affinity, and electronegativity and chemical potential are
mathematically related. This multicollinearity should be considered when
interpreting feature importance.

## Methodology

The workflow described in the article consists of the following stages:

1.  **Data collection:** Compile Azo-Schiff base metal-complex records
    and their experimentally reported IC₅₀ values from published
    studies.
2.  **Data preprocessing:** Identify missing values, duplicate
    observations, inconsistent IC₅₀ units, and abnormal records. Encode
    categorical variables and standardize numerical features where
    required.
3.  **Exploratory analysis:** Examine descriptor distributions,
    correlations, and relationships with IC₅₀.
4.  **Data split:** Divide the processed dataset into 70% training and
    30% testing subsets.
5.  **Model training:** Train and compare five regression algorithms.
6.  **Performance evaluation:** Assess predictions using R², MAE, MSE,
    RMSE, and MAPE.
7.  **Explainability:** Apply SHAP and LIME to interpret global feature
    influence and individual predictions.
8.  **Potential deployment:** The proposed framework discusses cloud
    storage and real-time prediction as a deployment direction. The
    supplied article does not provide a deployable service or its
    implementation files.

### Training and testing split

The paper reports **245 training samples** and **105 testing samples**
from the 350-record dataset. The test partition is an internal
evaluation set. The article acknowledges that a single random split may
not fully remove similarities among related complexes and that
independent validation and cross-validation are needed to assess
generalizability more thoroughly.

## Machine-Learning Models

The following regression algorithms are evaluated:

-   Random Forest
-   XGBoost
-   CatBoost
-   LightGBM
-   Support Vector Regression (SVR)

The best-performing model on the selected test partition---CatBoost---is
used for subsequent SHAP and LIME interpretation.

## Evaluation Metrics

  -----------------------------------------------------------------------
  Metric                              Purpose
  ----------------------------------- -----------------------------------
  R² (coefficient of determination)   Measures the proportion of target
                                      variance explained by the model

  MAE (mean absolute error)           Mean absolute difference between
                                      measured and predicted IC₅₀

  MSE (mean squared error)            Mean squared prediction error

  RMSE (root mean squared error)      Square root of MSE, expressed in
                                      the target variable's units

  MAPE (mean absolute percentage      Mean absolute percentage prediction
  error)                              error
  -----------------------------------------------------------------------

For IC₅₀ prediction, lower error values indicate closer predictions; a
higher R² indicates a better fit on the evaluated data.

## Results Reported in the Paper

### Best-performing model

  Test-set metric     CatBoost result
  ----------------- -----------------
  R²                           0.8933
  MAE                          5.3840
  MSE                         42.9651
  RMSE                         6.5548
  MAPE                        7.0627%

CatBoost achieved the highest reported test R² and the lowest reported
test errors among the five evaluated models. XGBoost showed very strong
training performance but a lower test R² of 0.8790, which the article
discusses as indicating a comparatively greater overfitting risk.
LightGBM was also competitive.

These results describe performance on the paper's selected test
partition; they are not evidence of independent external validation.

### Feature interpretation

The global SHAP analysis identifies the following as influential
features:

-   LogP
-   Dipole moment
-   Energy gap
-   Softness
-   Hardness

The article reports that higher energy-gap and hardness values generally
shifted CatBoost predictions toward lower IC₅₀ values, while higher
softness and LogP were associated with increased predicted IC₅₀ in the
global explanation. These are learned model associations, not
established causal effects.

### Individual explanations

The article provides local SHAP and LIME examples:

-   **Local SHAP example:** The model output moves from a baseline of
    87.118 µM to a predicted IC₅₀ of 56.342 µM. Energy gap, softness,
    hardness, and LogP contribute negative shifts of 7.34, 6.27, 5.32,
    and 3.67 µM, respectively; dipole moment contributes a positive
    shift of 0.82 µM.
-   **LIME example 1:** Predicted IC₅₀ is 88.38 µM. Gap, softness, and
    hardness have notable negative contributions, while LogP and dipole
    moment have positive contributions.
-   **LIME example 2:** Predicted IC₅₀ is 93.05 µM. LogP has the
    strongest negative contribution, while dipole moment and several
    electronic descriptors contribute positively.

Because lower IC₅₀ indicates stronger measured or predicted potency, a
negative contribution to predicted IC₅₀ lowers the model estimate.
Individual explanations apply to the specific examples described in the
paper and should not be generalized to every complex.

## Software and Computational Environment

The article reports using Python and the following libraries:

-   Pandas --- data handling.
-   NumPy --- numerical and statistical calculations.
-   Matplotlib and Seaborn --- visualization.
-   Scikit-learn --- machine-learning implementation.
-   SHAP --- model explanation.
-   LIME --- local model explanation.

The article does not provide a complete dependency lockfile, exact
package versions, source-code entry point, full preprocessing script, or
complete model configuration. Consequently, this README does not invent
installation commands or claim that the experiments can be reproduced
directly from this repository.

## Reproducing the Study

The supplied article describes the experimental workflow but does not
include the full dataset or executable source code. A faithful
computational reproduction therefore requires additional materials.

At a high level, reproduction would require:

1.  Obtain the 350-record dataset and verify each record's experimental
    source and DOI.
2.  Confirm IC₅₀ units, cell-line labels, metal and ligand categories,
    and descriptor calculation procedures.
3.  Reproduce the stated preprocessing and categorical encoding.
4.  Recreate the 245/105 training/testing split, including the original
    random seed if available.
5.  Train Random Forest, XGBoost, CatBoost, LightGBM, and SVR using the
    original configurations.
6.  Calculate R², MAE, MSE, RMSE, and MAPE on the same test partition.
7.  Reproduce the SHAP and LIME explanations using the same fitted
    CatBoost model and samples.

**Not supplied in the article files:** the record-level dataset,
complete source code, exact random seed, full hyperparameter settings,
package versions, and all DFT computational settings. These are needed
for exact reproduction.

## Limitations and Future Work

The article identifies several limitations and future directions:

-   DFT computational settings are not documented in sufficient detail
    for full reproducibility.
-   Detailed coordination-specific descriptors are not included.
-   Coordination geometry, ligand denticity, donor-atom environment,
    formal charge, oxidation state, and spin configuration require more
    explicit treatment.
-   The source-reference field (DOI) requires verification to establish
    experimental provenance.
-   Independent external validation, applicability-domain assessment,
    and prediction-uncertainty analysis are needed.
-   Correlated and mathematically dependent descriptors require careful
    feature selection and interpretation.

Future work proposed in the paper includes standardized quantum-chemical
calculations with clearly specified computational methods, basis sets,
solvent effects, molecular geometries, oxidation states, and spin
configurations, together with coordination-specific descriptors.
