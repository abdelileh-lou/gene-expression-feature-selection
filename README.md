# Leukemia Gene Expression Analysis: Feature Extraction, Selection, and Smart Combinations

A Machine Learning study of leukemia gene-expression classification, investigating dimensionality reduction, feature selection strategies, and intelligent feature combinations to improve classification performance while reducing feature dimensionality.

## Overview

High-dimensional gene-expression data presents a major challenge for Machine Learning because datasets often contain thousands of features but relatively few samples. This creates difficulties in identifying informative genes, training reliable classifiers, and avoiding overfitting.

This project investigates different approaches to analyzing leukemia gene-expression data through a four-stage experimental pipeline:

1. **Feature Extraction:** Dimensionality reduction using Principal Component Analysis (PCA) and Linear Discriminant Analysis (LDA).
2. **Cross-Dataset Validation:** Evaluating generalization by training on the PONE dataset and testing on the Golub dataset.
3. **Feature Selection:** Comparing Filter, Wrapper, and Embedded feature selection techniques.
4. **Smart Feature Combinations:** Combining selected features through intersection, union, and voting strategies.

Five classification algorithms are evaluated using stratified 5-fold cross-validation and balanced accuracy.

The experiments demonstrate that combining feature selection strategies can improve classification performance while substantially reducing the number of features required.

## Project Objectives

- Analyze high-dimensional leukemia gene-expression data.
- Investigate dimensionality reduction techniques.
- Compare different feature selection strategies.
- Evaluate multiple Machine Learning classifiers.
- Investigate cross-dataset generalization.
- Combine feature selection results to identify informative gene subsets.
- Find a balance between classification performance and feature reduction.

## Datasets

The experiments use two leukemia gene-expression datasets.

| Dataset | Description | Usage |
|---|---|---|
| PONE | 94 samples: 47 normal and 47 tumour | Primary training and evaluation dataset |
| Golub | Benchmark leukemia gene-expression dataset | Cross-dataset validation |

The Golub dataset is used to investigate how well a model trained on PONE generalizes to another dataset.

**Note:** The datasets are not included in this repository. Users must obtain the data from their respective sources and configure the dataset paths in the Python implementation.

Golub dataset source: https://www.broadinstitute.org/cgi-bin/cancer/datasets.cgi

## Methodology

The project is organized into four experimental stages.

### 1. Feature Extraction

Feature extraction transforms the original feature space into a lower-dimensional representation.

Two techniques are investigated:

**Principal Component Analysis (PCA)**

- Unsupervised dimensionality reduction.
- Retains components explaining 95% of the variance.
- Projects the original features into a smaller set of principal components.

**Linear Discriminant Analysis (LDA)**

- Supervised dimensionality reduction.
- Uses class information to find discriminative projections.
- Maximizes between-class variance relative to within-class variance.

Both techniques are compared with a baseline using the original features.

### 2. Cross-Dataset Validation

The best extraction approach is investigated through cross-dataset evaluation.

- Training dataset: PONE.
- Testing dataset: Golub.
- Feature alignment: Common gene probes between datasets.
- Objective: Evaluate generalization across different gene-expression datasets.

### 3. Feature Selection

Three families of feature selection methods are investigated.

**Filter Methods**

Evaluate features using statistical criteria independently of a specific classifier.

- Chi-Square (Chi2)
- Mutual Information
- ANOVA F-test

**Wrapper Methods**

Evaluate feature subsets using the performance of a learning algorithm.

- Recursive Feature Elimination (RFE) with Linear SVM.

**Embedded Methods**

Perform feature selection as part of the model training process.

- Random Forest Feature Importance.
- Lasso (L1 Regularization).

The selected feature subsets are evaluated using multiple classifiers and compared according to their average balanced accuracy.

### 4. Smart Feature Combinations

The selected features from different methods are combined to investigate whether combining selection strategies provides better results.

Three combination strategies are investigated:

**Intersection**

Selects features shared by the different feature selection methods.

**Union**

Combines the features selected by at least one method, preserving complementary information from different approaches.

**Voting**

Selects features supported by at least three feature selection methods, producing a compact consensus-based feature subset.

The objective is to identify feature subsets that provide a good balance between classification performance and dimensionality reduction.

## Machine Learning Algorithms

Five classifiers are used throughout the experiments.

| Algorithm | Description |
|---|---|
| Linear SVM | Linear Support Vector Machine |
| KNN | k-Nearest Neighbors |
| Decision Tree | Tree-based classification |
| Random Forest | Ensemble of decision trees |
| Naive Bayes | Probabilistic classification |

### Main Hyperparameters

| Classifier | Parameter | Value |
|---|---|---|
| Linear SVM | C | 1.0 |
| KNN | n_neighbors | 5 |
| Decision Tree | max_depth | None |
| Random Forest | n_estimators | 100 |
| Naive Bayes | var_smoothing | 1e-9 |

## Experimental Setup

The experiments use the following evaluation configuration:

- Cross-validation: Stratified 5-Fold.
- Performance metric: Balanced Accuracy.
- Primary dataset: PONE.
- Number of classifiers: 5.
- Feature extraction: PCA and LDA.
- Feature selection: Filter, Wrapper, and Embedded methods.

**Balanced Accuracy**

Balanced accuracy is the average recall obtained across all classes.

For binary classification:

\[
BA = \frac{Sensitivity + Specificity}{2}
\]

It is particularly useful when evaluating classification performance across different classes.

## Results and Performance

### Step 1: Feature Extraction

PCA and LDA are compared against the original feature space.

The reported experiment shows that extraction performance depends on the classifier. PCA does not consistently improve performance, while LDA provides a supervised representation that can preserve class-discriminative information.

### Step 2: Cross-Dataset Validation

The best extraction approach is investigated using PONE for training and Golub for testing.

This experiment evaluates whether the learned representation generalizes across datasets with different gene-probe spaces.

### Step 3: Feature Selection

The following average balanced accuracy results are reported in the feature selection comparison:

| Feature Selection Method | Category | Balanced Accuracy |
|---|---|---:|
| Mutual Information | Filter | 0.9900 |
| LDA | Extraction | 0.9411 |
| Random Forest Importance | Embedded | 0.9356 |
| Chi-Square | Filter | 0.9289 |
| ANOVA | Filter | 0.9244 |
| RFE (Linear SVM) | Wrapper | 0.9156 |
| No Selection | Baseline | 0.9111 |
| Lasso | Embedded | 0.9011 |

Mutual Information obtains the highest reported score in this comparison, highlighting the importance of evaluating individual genes according to their statistical relationship with the target classes.

### Step 4: Smart Feature Combinations

The final experiment evaluates combinations of features selected by different methods.

| Strategy | Number of Features | KNN | Random Forest |
|---|---:|---:|---:|
| Baseline | ~7,070 | 0.9289 | 0.9689 |
| Voting | 7 | 0.9467 | — |
| Intersection | 42 | — | — |
| Union | 40 | 0.9900 | 0.9789 |

The union strategy achieves the strongest reported classification results:

- KNN: 0.9900 balanced accuracy.
- Random Forest: 0.9789 balanced accuracy.

The voting strategy offers a compact alternative, using only seven features and achieving 0.9467 balanced accuracy with KNN.

These findings demonstrate that combining different feature selection strategies can identify complementary information and provide an effective trade-off between model performance and feature dimensionality.

## Results Visualizations

### Feature Extraction

![Feature Extraction Results](img/feature_extraction_results.png)

### Cross-Dataset Validation

![Cross Dataset Validation](img/cross_dataset_validation.png)

### Feature Selection

![Feature Selection Results](img/feature_selection_results.png)

### Smart Feature Combinations

![Smart Feature Combinations](img/smart_combinations_results.png)

## Technologies and Libraries

- **Python** – Main programming language.
- **NumPy** – Numerical computations.
- **Pandas** – Data manipulation and analysis.
- **Scikit-learn** – Feature extraction, feature selection, classification, and evaluation.
- **Matplotlib** – Data visualization.
- **Seaborn** – Statistical visualization.
- **SciPy** – Scientific computing.

## Project Structure

```text
leukemia-gene-expression-analysis/
│
├── README.md
│
├── src/
│   └── feature_selection.py
│
├── img/
│   ├── feature_extraction_results.png
│   ├── cross_dataset_validation.png
│   ├── feature_selection_results.png
│   └── smart_combinations_results.png
│
├── requirements.txt
├── .gitignore
└── LICENSE
```

## Installation and Execution

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/leukemia-gene-expression-analysis.git

cd leukemia-gene-expression-analysis
```

### 2. Create a virtual environment

```bash
python -m venv venv
```

Activate it:

**Linux / macOS**

```bash
source venv/bin/activate
```

**Windows**

```bash
venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure the datasets

Download the required datasets and update the dataset paths inside the Python script.

### 5. Run the project

```bash
python src/feature_selection.py
```

## Academic Information

**Institution:** Université des Frères Mentouri – Constantine 1 / Constantine 2

**Faculty:** Faculté des NTIC

**Department:** IFA

**Program:** Master 2 – Data Analysis and Machine Intelligence (DAMI2)

**Module:** Data Analysis and Machine Intelligence

**Academic Year:** 2024–2025

**Project Date:** May 8, 2026

### Authors

- **Abdelileh Louhichi**
- **Abderaouf Boukhchem**

## Conclusion

This project presents a comparative study of feature extraction, feature selection, and intelligent feature combination strategies for leukemia gene-expression classification.

The experiments highlight three important findings:

1. Mutual Information achieves the highest reported score among the individual feature selection approaches.
2. Combining selected features through a union strategy improves classification performance, reaching 0.9900 balanced accuracy with KNN.
3. A voting-based strategy provides a compact alternative, achieving 0.9467 balanced accuracy using only seven features.

Overall, the study demonstrates the value of combining statistical feature selection and Machine Learning classification techniques when working with high-dimensional biomedical datasets.

## Future Work

Potential directions for further investigation include:

- Exploring deep learning approaches such as autoencoders.
- Investigating feature selection stability across multiple random splits.
- Improving cross-dataset generalization.
- Evaluating selected genes on independent external datasets.
- Incorporating biological pathway and gene ontology information.

## References

1. Golub, T. R., Slonim, D. K., Tamayo, P., et al. (1999). Molecular Classification of Cancer: Class Discovery and Class Prediction by Gene Expression Monitoring. *Science*, 286(5439), 531–537.
2. Li, Y., Chen, C. Y. (2021). Deep Feature Learning for Support Vector Machine-Based Leukemia Classification. *Computational and Mathematical Methods in Medicine*.
3. Zhang, X., Liu, Q., Wang, J. (2022). Mutual Information-Based Feature Selection for Leukemia Gene Expression Analysis. *Bioinformatics*, 38(4), 1120–1128.

---

**Disclaimer:** This repository presents an academic research project. The experimental findings do not constitute clinical validation or a diagnostic tool.
