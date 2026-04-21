# CR-CVRIE

Projet d'analyse de donnees en Python autour de deux axes:

1. Clustering non supervise de temoignages patients (NLP)
2. Preparation et baseline de classification sur un dataset d'images medicales (Kaggle)

Le repository contient principalement deux notebooks:

- `unsupervised_clustering.ipynb`
- `kaggle_dataset.ipynb`

## Objectifs

- Construire un pipeline non supervise de bout en bout sur des textes non labels
- Comparer plusieurs algorithmes de clustering avec des metriques adaptees
- Visualiser et interpreter les groupes obtenus
- Telecharger/nettoyer un dataset d'images et etablir une baseline de classification

## Structure du projet

```text
CR-CVRIE/
|- README.md
|- Student_Dataset.csv
|- unsupervised_clustering.ipynb
`- kaggle_dataset.ipynb
```

## Donnees

### 1) Donnees texte

- Fichier: `Student_Dataset.csv`
- Contenu: temoignages patients (format heterogene selon les lignes)
- Utilisation: clustering non supervise dans `unsupervised_clustering.ipynb`

### 2) Donnees images

- Source: Kaggle via `kagglehub`
- Dataset cible dans le notebook: `bmadushanirodrigo/fracture-multi-region-x-ray-data`
- Utilisation: preparation, nettoyage, split train/val/test, baseline RandomForest dans `kaggle_dataset.ipynb`

## Methodologie

### Notebook 1 - `unsupervised_clustering.ipynb`

Pipeline principal:

1. Parsing du CSV (gestion des formats `id,testimony` et `id,color,testimony`)
2. Nettoyage texte (lowercase, ponctuation, espaces)
3. Vectorisation TF-IDF (unigrammes + bigrammes)
4. Reduction dimensionnelle (SVD + normalisation)
5. Comparaison de modeles:
	- KMeans
	- Agglomerative Clustering
	- DBSCAN
6. Evaluation sans labels:
	- Silhouette (plus grand = mieux)
	- Calinski-Harabasz (plus grand = mieux)
	- Davies-Bouldin (plus petit = mieux)
7. Selection automatique du meilleur modele
8. Visualisations PCA et extraction de mots-cles par cluster

Bonus dans le notebook:

- Section de comparaison Avant/Apres pour mesurer l'impact d'un changement de modele/hyperparametres

### Notebook 2 - `kaggle_dataset.ipynb`

Pipeline principal:

1. Telechargement du dataset Kaggle avec `kagglehub`
2. Exploration des fichiers
3. Nettoyage:
	- suppression des doublons via hash de pixels
	- detection/suppression des images corrompues
4. Construction d'un DataFrame (`filepath`, `label`)
5. Split stratifie train/validation/test (70/15/15)
6. Preprocessing image (resize, normalisation, flatten)
7. Entrainement d'une baseline `RandomForestClassifier`
8. Evaluation (accuracy + classification report)
9. Bloc diagnostic (verification du leak, baseline aleatoire, importance des features)

## Prerequis

- Python 3.10+
- Jupyter Notebook ou VS Code avec extension Notebook

Packages utilises (selon notebook):

- numpy
- pandas
- matplotlib
- scikit-learn
- kagglehub

## Installation rapide

```bash
python -m venv .venv
# Windows PowerShell
.\.venv\Scripts\Activate.ps1

pip install --upgrade pip
pip install numpy pandas matplotlib scikit-learn jupyter kagglehub
```

## Execution

1. Ouvrir le dossier du projet
2. Lancer Jupyter/VS Code
3. Ouvrir `unsupervised_clustering.ipynb` puis executer les cellules dans l'ordre
4. Ouvrir `kaggle_dataset.ipynb` puis executer les cellules dans l'ordre

## Resultats attendus

### Clustering (NLP)

- Tableau de ranking des configurations de clustering
- Modele retenu automatiquement
- Graphiques PCA (couleur dataset vs cluster predit)
- Mots-cles representatifs par cluster

### Images (Kaggle)

- Resume du nettoyage (doublons/corruptions)
- Tailles des splits train/val/test
- Accuracy validation et test
- Rapport de classification
- Graphiques de diagnostic

## Limites

- Le clustering non supervise reste sensible au preprocessing et aux hyperparametres
- Si la colonne `color` est uniforme, la verification visuelle par couleur est moins informative
- La baseline RandomForest sur images aplaties est un point de depart, pas un SOTA

## Pistes d'amelioration

- Tester des embeddings modernes pour le texte (Sentence Transformers)
- Evaluer la stabilite des clusters sur plusieurs seeds/sous-echantillons
- Remplacer la baseline image par un CNN (transfer learning)
- Ajouter un fichier `requirements.txt` versionne pour une meilleure reproductibilite

## Auteurs

Projet realise dans le cadre de CR-CVRIE (Epitech).