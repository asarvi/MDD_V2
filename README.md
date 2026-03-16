# MDD_V2
Here is a **cleaner README that does not mention any specific model** and works for a repository containing **multiple models (RF, XGBoost, GCN, GraphSAGE, etc.)**.

---

# MDD Gene Prioritization – Network-Based Machine Learning Framework

This project aims to identify candidate genes associated with **Major Depressive Disorder (MDD)** using a **protein–protein interaction (PPI) network** combined with machine learning models.

The framework integrates **network topology, biological priors, and multiple training runs with different negative samples** to prioritize genes that may play a role in MDD but are not currently labeled as known disease genes.

---

# Pipeline Overview

The project consists of four main stages:

1. **Network Construction**
2. **Feature Extraction**
3. **Negative Gene Sampling**
4. **Model Training and Candidate Ranking**

Multiple models are implemented in the repository, including both **classical machine learning approaches and graph neural networks**.

---

# 1. Network Construction

A weighted gene interaction network is constructed using **protein–protein interaction data**.

* Nodes represent **genes**
* Edges represent **interactions between proteins**
* Edge weights correspond to **interaction confidence scores**

Only interactions above a predefined confidence threshold are retained to ensure network reliability.

Known MDD genes are used as **positive labels**.

---

# 2. Feature Extraction

For each gene in the network, a set of **topological and biologically informed features** is computed.

These include:

Network topology features:

* Degree
* Clustering coefficient
* Neighbor statistics

Centrality measures:

* Degree centrality
* Betweenness centrality
* Closeness centrality

Biological connectivity features:

* Connectivity strength to known disease genes

These features provide structural signals that help distinguish disease-associated genes from the rest of the network.

---

# 3. Negative Gene Sampling

Because true negative genes for MDD are unknown, a **heuristic-based negative sampling strategy** is used.

Steps:

1. A heuristic score is calculated for all non-positive genes based on network proximity and connectivity to known disease genes.

2. Candidate negatives are restricted to genes with **low heuristic scores**, reducing the risk of selecting potential disease genes as negatives.

3. Multiple **random negative gene sets** are generated from this pool.

Each negative set is paired with the positive genes to create **multiple training datasets**.

This approach helps reduce bias caused by uncertain negative labels.

---

# 4. Model Training

Each training dataset (positive genes + one negative set) is used to train a predictive model.

Models learn to distinguish between:

* **Known disease genes (positives)**
* **Sampled negative genes**

Multiple models are implemented in this repository, including both **traditional machine learning models and graph-based neural networks**.

Performance is evaluated using standard classification metrics such as:

* ROC-AUC
* Precision
* Recall
* F1-score

---

# 5. Candidate Gene Ranking

After training, models are used to score **all genes in the network**.

Genes that are not labeled as positives or negatives are treated as **candidate genes**.

Final rankings can be generated using:

**Single-model predictions**
or

**Consensus scores across multiple runs**, where the final score is computed as the average prediction across models trained with different negative samples.

This consensus approach improves robustness and reduces sensitivity to negative sampling.

---

# Output

The pipeline produces:

| File                           | Description                                 |
| ------------------------------ | ------------------------------------------- |
| `scored_all_nodes.csv`         | Features and heuristic scores for all genes |
| `refined_negative_genes_*.csv` | Generated negative gene sets                |
| `model_results.csv`            | Performance metrics across runs             |
| `candidate_gene_ranking.csv`   | Final ranked candidate genes                |

---

# Key Data Objects

| Variable             | Description                         |
| -------------------- | ----------------------------------- |
| `G`                  | Gene interaction network            |
| `positive_genes`     | Known MDD-associated genes          |
| `negative_gene_sets` | Randomly generated negative samples |
| `all_feature_df`     | Feature table for all genes         |
| `rank_candidates_df` | Ranked candidate genes              |

---

# Reproducibility

* Random seeds are used where possible for reproducibility.
* Multiple negative samples are generated to evaluate model stability.
* All models use identical feature sets for fair comparison.

---

# Dependencies

The project relies on common scientific Python libraries:

```
python
pandas
numpy
networkx
scikit-learn
matplotlib
torch
```

Additional libraries may be required depending on the model being used.

---



These make the project **much easier for reviewers or supervisors to follow.
