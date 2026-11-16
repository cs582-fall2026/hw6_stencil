# Homework 6: Introduction to GNNs and R-GCNs

## Overview

In this homework, you will work with biomedical graph data to explore fundamental ideas behind Graph Neural Networks (GNNs) and Relational Graph Convolutional Networks (R-GCNs). You will:

- Load and process graph-structured datasets
- Construct meaningful node and edge representations
- Train GNN and R-GCN models for link prediction
- Evaluate model performance
- Visualize learned node embeddings and graph structure

---

## Dataset

You are provided with two biomedical graph datasets for experimentation.

- **Drug-disease interactions** (`pharmaDB_*.csv`, ~1.4k edges): nodes are drugs and diseases. The `Y` column is the relation type:

  | `Y` | relation | meaning |
  |---|---|---|
  | 0 | `treats` | disease-modifying indication |
  | 1 | `palliates` | symptomatic indication |
  | 2 | `neither` | the pair was reviewed and the drug is **not** indicated for the disease |

- **Drug-drug interactions** (`drugbank_*.csv`, ~131k edges): nodes are drugs, and `Y` is one of 86 pharmacological interaction types. These types are anonymous integer IDs in this release (no text labels), so interpret them by ID and by the drugs involved.

The data comes as `*_train_set.csv` / `*_test_set.csv` files. The split cell at the top of `src/playground.ipynb` turns these into the train/test splits used in this homework (see the docstring there for why). Use part of your training set for validation. In this homework, you will convert those CSV files into PyTorch Geometric `Data` objects and use them for graph-based link prediction.

Note: the DrugBank training file is ~100 MB, and training on it takes much longer than on the drug-disease graph: roughly 1–3 s per epoch on a CPU (R-GCN is the slower one), so a single DrugBank R-GCN run can take several minutes. Budget time for your sweeps, and start them early.

---

## Installation

1. **Clone** this repo:
   ```bash
   git clone git@github.com:cs582-fall2026/hw6_stencil.git
   cd hw6_stencil
   ```
2. Create virtual environment:
    ```bash
    python -m venv .hw6
    ```
3. Install dependencies:
    ```bash
    source .hw6/bin/activate (Linux/MacOS) or .\.hw6\Scripts\activate (Windows)
    pip install -r requirements.txt
    ```

After creating and activating the virtual environment, select it as the Jupyter kernel in `src/playground.ipynb` to run the notebook using the same installed dependencies.

---

## Tasks

You will complete the following:

- [ ] Implement your own CSV-to-graph loading pipeline:
  - Parse the split CSV files
  - Build shared node and relation ID mappings
  - Construct PyTorch Geometric `Data` objects
- [ ] Preserve lookup metadata during loading so you can interpret embeddings later:
  - Store a mapping from node ID back to the original drug or disease identifier
  - Store a mapping from relation ID back to the original interaction label
- [ ] Load and explore both graphs (drug-disease and drug-drug)
- [ ] Visualize node degree and edge-type distributions
- [ ] Train a simple **GCN** model for link prediction (for both datasets)
- [ ] Train an **R-GCN** model for link prediction (for both datasets)
- [ ] Compare **GCN** and **R-GCN** performance on the provided data
- [ ] Report a ranking metric for link prediction on the test set (at least **ROC-AUC** and **average precision**), in addition to accuracy/F1
- [ ] Experiment with different node feature types:
  - Random features
  - One-hot features
  - [Optional] Pretrained features derived from the entity description
    (for example, embeddings using the drug's or disease's definition provided with the data)
    Some DrugBank descriptions are missing (`NaN`), so fill them (e.g., with the drug name) before vectorizing.
- [ ] Compare results across multiple hyperparameter settings:
  - Embedding dimension
  - Number of GNN layers
  - Node feature type (random, one-hot, description-based features)
- [ ] Generate PCA and t-SNE projections of learned node embeddings
- [ ] Interpret whether drugs, diseases, or relation types cluster together or separate cleanly, or behave like outliers

---

## Final Reflection

You will then write a **2-3 page reflection** that includes **figures** and **interpretation** of your results. Your write-up should clearly reference the plots, tables, and metrics you generated.

- [ ] How does the performance of a **GCN** compare to that of an **R-GCN**?
- [ ] How much do relation types help link prediction, and when do they matter most?
- [ ] How do node feature choices affect results: `random` vs `one-hot`, and `pretrained` if you implemented description-based features as an extension?
- [ ] What do the PCA and t-SNE visualizations reveal about the structure of the learned node embeddings?
- [ ] Do drugs and diseases form meaningful clusters or separations in embedding space, and where do those patterns fail?
- [ ] Which biomedical entities or relation types seem to drive the most interesting clusters, outliers, or separations?
- [ ] How stable are your conclusions across different hyperparameter settings?
- [ ] What aspects of the graph structure or model behavior surprised you?
- [ ] What are the strengths and limitations of GNNs for biomedical data, especially when datasets are small, sparse, or synthetic?

---

## Expected Skills

By the end of this homework, you should be able to:

- Represent structured biomedical data as graphs
- Understand and apply GNN and R-GCN layers
- Handle multi-relation graphs for link prediction
- Evaluate graph models with appropriate metrics
- Reflect on the role of graph structure and node features in biomedical learning
