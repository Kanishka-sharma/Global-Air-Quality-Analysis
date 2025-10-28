# Global Air Quality Analysis — Unsupervised Learning on Atmospheric Data

### Project Overview
This project explores atmospheric gas concentration data collected from the **Mauna Loa Observatory**.
The dataset contains multiple greenhouse gases (CO₂, CO, Methane, Nitrous Oxide, and CFC-11) recorded over time.

Our goal is to:
- Handle **missing data** using statistical imputation,
- Apply **dimensionality reduction** (PCA),
- Perform **unsupervised clustering** (K-Means, K-Medoids, Hierarchical, and Model-Based),
- Evaluate and visualize the resulting **air quality groupings**.

This analysis demonstrates a complete **end-to-end data science workflow** — from raw data to meaningful environmental insights.

---

### Key Objectives
1. **Explore** data characteristics and detect missingness/outliers.
2. **Identify** the missing data mechanism (MCAR/MAR/MNAR).
3. **Impute** missing values using **MICE (Predictive Mean Matching)**.
4. **Reduce** dimensionality with **Principal Component Analysis (PCA)**.
5. **Cluster** observations using:
   - K-Means
   - K-Medoids (PAM)
   - Hierarchical (Ward’s method)
   - Model-Based Clustering (via BIC)
6. **Interpret** environmental emission patterns revealed by clusters.

---

### Dataset
- **Source**: Simulated/derived Mauna Loa Observatory dataset
- **Shape**: 183 observations × 5 continuous variables
- **Variables**:
  - `CO` - Carbon Monoxide
  - `CO2` - Carbon Dioxide
  - `Methane`
  - `NitrousOx` - Nitrous Oxide
  - `CFC11` - Chlorofluorocarbon-11

Missing data ranged between **5%-20% per variable**.

---

### Methodology Overview

#### 1️. Exploratory Data Analysis
- Checked missing values, outliers, and correlations
- Visualized variable distributions and missingness patterns using:
  - `corrplot`
  - `VIM::aggr()`
  - `ggplot2` boxplots

#### 2️. Missing Data Mechanism Analysis
- Applied **Little’s MCAR test**
- Used **logistic regression** to test for MAR dependency
- Performed **Wilcoxon tests** for potential MNAR behavior

Findings:
- `CO2` missingness ≈ MCAR
- `CO` missingness ≈ MAR (dependent on `CFC11`)

#### 3️. Imputation
- `CO2` → Imputed via **MICE (PMM)**
- `CO` → Imputed using **CFC11** as predictor under MAR assumption

#### 4️. Dimensionality Reduction (PCA)
- Standardized data before PCA
- Extracted eigenvalues, scree plot, and variable contributions
- First two PCs explained **~97.6% of total variance**
- Stability checked using **bootstrap-based eigenvalue distribution**

#### 5️. Clustering
| Method | Optimal K | Evaluation Metric | Avg. Silhouette |
|--------|-------------|------------------|-----------------|
| K-Means | 2 | Silhouette | 0.472 |
| K-Medoids (PAM) | 2 | Silhouette | 0.472 |
| Hierarchical (Ward.D2) | 2 | Silhouette | 0.509 |
| Model-Based (BIC) | 3 | Highest BIC | 0.468 |

The **Hierarchical** approach provided the most stable clustering.

#### 6️. Visualization
- **Scree Plot:** Explained variance by PCs
- **PCA Biplot:** Variable correlations
- **Cluster plots:** Samples grouped in PCA space
- **Silhouette plots:** Cluster quality comparison

---

### Key Insights
- Two main **emission regimes** were identified:
  - **Cluster 1:** Higher CO₂, Methane, and N₂O → likely industrial/urban influence
  - **Cluster 2:** Lower overall emissions → background or natural variability
- PCA revealed **CO₂ and Methane** as dominant contributors to total variance.
- Imputation and PCA confirmed strong inter-variable correlation, validating environmental dependence among gases.

---

### Tools & Technologies
| Category | Tools/Packages |
|-----------|----------------|
| Language | R |
| Data Handling | `dplyr`, `naniar`, `VIM` |
| Visualization | `ggplot2`, `corrplot`, `factoextra` |
| Imputation | `mice` |
| Dimensionality Reduction | `FactoMineR` |
| Clustering | `cluster`, `mclust` |

---

### Project Learnings
- Handling **non-random missingness** through diagnostics and custom imputation
- Applying **unsupervised learning** to discover latent structure in real-world environmental data
- Evaluating clustering stability and **model interpretability** in PCA-reduced space
- Building reproducible and visual **data science pipelines in R**
