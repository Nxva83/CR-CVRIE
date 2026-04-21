# CR-CVRIE

Python data analysis project focused on two main areas:

1. Unsupervised clustering of patient testimonials (NLP)
2. Preparation and baseline classification on a medical image dataset (Kaggle)

The repository mainly contains two notebooks:

- `unsupervised_clustering.ipynb`
- `kaggle_dataset.ipynb`

## Goals

- Build an end-to-end unsupervised pipeline for unlabeled text data
- Compare multiple clustering algorithms with suitable metrics
- Visualize and interpret the resulting groups
- Download/clean an image dataset and establish a classification baseline

## Project structure

```text
CR-CVRIE/
|- README.md
|- Student_Dataset.csv
|- unsupervised_clustering.ipynb
`- kaggle_dataset.ipynb
```

## Data

### 1) Text data

- File: `Student_Dataset.csv`
- Content: patient testimonials (heterogeneous format depending on the row)
- Usage: unsupervised clustering in `unsupervised_clustering.ipynb`

### 2) Image data

- Source: Kaggle via `kagglehub`
- Target dataset in the notebook: `bmadushanirodrigo/fracture-multi-region-x-ray-data`
- Usage: preparation, cleaning, train/val/test split, RandomForest baseline in `kaggle_dataset.ipynb`

## Methodology

### Notebook 1 - `unsupervised_clustering.ipynb`

Main pipeline:

1. CSV parsing (handling `id,testimony` and `id,color,testimony` formats)
2. Text cleaning (lowercase, punctuation, spaces)
3. TF-IDF vectorization (unigrams + bigrams)
4. Dimensionality reduction (SVD + normalization)
5. Model comparison:
	- KMeans
	- Agglomerative Clustering
	- DBSCAN
6. Label-free evaluation:
	- Silhouette (higher = better)
	- Calinski-Harabasz (higher = better)
	- Davies-Bouldin (lower = better)
7. Automatic best-model selection
8. PCA visualizations and keyword extraction by cluster

Notebook bonus:

- Before/After comparison section to measure the impact of model/hyperparameter changes

### Notebook 2 - `kaggle_dataset.ipynb`

Main pipeline:

1. Downloading the Kaggle dataset with `kagglehub`
2. File exploration
3. Cleaning:
	- duplicate removal using pixel hashes
	- corrupted image detection/removal
4. Building a DataFrame (`filepath`, `label`)
5. Stratified train/validation/test split (70/15/15)
6. Image preprocessing (resize, normalization, flatten)
7. Training a `RandomForestClassifier` baseline
8. Evaluation (accuracy + classification report)
9. Diagnostic block (leak check, random baseline, feature importance)

## Prerequisites

- Python 3.10+
- Jupyter Notebook or VS Code with the Notebook extension

Packages used (depending on the notebook):

- numpy
- pandas
- matplotlib
- scikit-learn
- kagglehub

## Quick setup

```bash
python -m venv .venv
# Windows PowerShell
.\.venv\Scripts\Activate.ps1

pip install --upgrade pip
pip install numpy pandas matplotlib scikit-learn jupyter kagglehub
```

## Running the project

1. Open the project folder
2. Launch Jupyter/VS Code
3. Open `unsupervised_clustering.ipynb` and run the cells in order
4. Open `kaggle_dataset.ipynb` and run the cells in order

## Expected results

### Clustering (NLP)

- Ranking table of clustering configurations
- Automatically selected best model
- PCA plots (dataset color vs predicted cluster)
- Representative keywords per cluster

### Images (Kaggle)

- Cleaning summary (duplicates/corruptions)
- Train/val/test split sizes
- Validation and test accuracy
- Classification report
- Diagnostic plots

## Limitations

- Unsupervised clustering remains sensitive to preprocessing and hyperparameters
- If the `color` column is uniform, visual color-based checks are less informative
- The RandomForest baseline on flattened images is a starting point, not SOTA

## Improvement ideas

- Test modern text embeddings (Sentence Transformers)
- Evaluate cluster stability across multiple seeds/subsamples
- Replace the image baseline with a CNN (transfer learning)
- Add a versioned `requirements.txt` file for better reproducibility

## Authors

Project completed as part of CR-CVRIE (Epitech).