# Lab 5: Hierarchical and DBSCAN Clustering on the Wine Dataset

## Purpose

This lab explores two unsupervised clustering techniques - **Agglomerative Hierarchical Clustering** and
**DBSCAN** - applied to the Wine dataset from `sklearn.datasets`. The goal is to build hands-on experience
with clustering workflows: preparing and standardizing data, tuning algorithm-specific hyperparameters,
visualizing results, and evaluating cluster quality using both internal metrics (Silhouette Score) and
external metrics (Homogeneity and Completeness, which compare clusters against the known wine cultivar
labels).

## How to Run

1.**Clone the repository**

```bash
   git clone https://github.com/Hanuman42109/ABD_and_DM_Lab_5
   cd ABD_and_DM_Lab_5
```

2.**Create and activate a virtual environment**

```bash
   python -m venv venv
   venv\Scripts\activate        # Windows
   source venv/bin/activate     # Mac/Linux
```

3.**Install dependencies**

```bash
   pip install pandas numpy matplotlib seaborn scikit-learn scipy ipykernel
```

4.**Open in VS Code**

- Open the project folder in VS Code
- Press `Ctrl+Shift+P` → **Python: Select Interpreter** → choose `venv`
- Open `ABD_and_DM_Lab_5.ipynb`
- Click **Run All**

## Key Insights from Clustering Results

- **Standardization mattered.** The Wine dataset's 13 chemical features are on very different scales (e.g.,
  `proline` is in the hundreds while `hue` is close to 1). Standardizing with `StandardScaler` was essential
  before applying any distance-based clustering algorithm.
- **Hierarchical Clustering recovered the true structure well.** With `n_clusters=3` (matching the 3 known
  wine cultivars), Agglomerative Clustering (Ward linkage) produced clusters that aligned closely with the
  ground-truth labels, confirmed by a high Adjusted Rand Index and solid Homogeneity/Completeness scores. The
  dendrogram showed a clear, tall separation near the top of the tree consistent with 3 natural groups.
- **DBSCAN was much more sensitive to its hyperparameters.** Small `eps` values produced excessive noise
  points, while larger `eps` values caused distinct cultivars to merge into a single large cluster. Only a
  narrow range of `eps`/`min_samples` combinations produced a reasonable 3-cluster structure, and even then
  DBSCAN didn't separate the cultivars quite as cleanly as Hierarchical Clustering did.
- **DBSCAN's noise-detection is a double-edged sword for this dataset.** Since the Wine dataset's classes are
  fairly compact and don't contain many true outliers, DBSCAN's ability to flag noise points wasn't a big
  advantage here - it mostly just made the algorithm harder to tune. DBSCAN would likely shine more on
  datasets with irregularly shaped clusters or genuine outliers.
- **Overall**, for this particular dataset, Hierarchical Clustering was the more reliable of the two methods,
  while DBSCAN required more careful parameter search and produced less consistent alignment with the known
  cultivar labels.

## Challenges Faced / Decisions Made

- **Choosing DBSCAN parameters:** Finding a good `eps`/`min_samples` combination took some experimentation.
  A small grid search over several `eps` values (1.0–2.5) and `min_samples` values (3, 5) was used to
  understand the trade-off between number of clusters and amount of noise before settling on a representative
  configuration (`eps=2.0`, `min_samples=5`) for detailed evaluation.
- **Visualizing 13-dimensional data:** Since the Wine dataset has 13 features, PCA was used to project the
  standardized data down to 2 principal components purely for visualization. Clustering itself was performed
  on the full standardized feature space, not the PCA-reduced space, so the 2D scatter plots are a
  simplified view rather than the exact clustering geometry.
- **Handling noise in evaluation:** Silhouette Score is undefined when DBSCAN produces fewer than 2 clusters
  or when noise points are included in a way that leaves only one real cluster, so noise points were excluded
  before computing the Silhouette Score for DBSCAN, while Homogeneity and Completeness were computed on the
  full label set (treating noise as its own "cluster") for a fair comparison against Hierarchical Clustering.
