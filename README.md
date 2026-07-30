# HIV Bioactivity Comparative Evaluation Framework

- Das, Bihter, et al. **“A Comparative Evaluation Framework Integrating Machine Learning and Deep Learning Models with ADME-Based Pharmacokinetic Assessment for HIV-Related Compounds.”** *Molecules* (MDPI), 2026.
- Authors: Bihter Das, Harun Uslu, Ayca Bostancioglu, Bunyamin Goktas, Yunus Santur, Seval Yilmaz, and Ibrahim Turkoglu

Predicting the molecular biological activity of Human Immunodeficiency Virus (HIV) is critical for accelerating early-stage drug discovery. Most existing studies focus only on prediction metrics and overlook pharmacokinetic suitability and drug-likeness. This repository provides a unified experimental protocol comparing classical machine learning, deep learning, and geometric deep learning models on the MoleculeNet HIV dataset, followed by ADME and drug-likeness assessment of top candidate compounds.

## Dataset

The original application dataset contains **3,043 unique compounds** (**1,443 active** and **1,600 inactive**). The full MoleculeNet HIV pool is provided under `dataset/` for reference. In a sensitivity analysis, we generated **30 independent chemically matched alternative inactive subsets** from the MoleculeNet HIV inactive pool while keeping the active compounds fixed.

<img width="1245" height="1813" alt="flowchart (2)-1" src="https://github.com/user-attachments/assets/55afdc81-c714-4af4-9563-bd3bd2df628d" />

- **Best model (GDL)**: ROC-AUC **0.956 ± 0.015**
- **Strong baselines**: GRU (**0.930 ± 0.017**) and Random Forest (**0.927 ± 0.023**)
- **Statistical validation**: Friedman test (*p* < 0.001) with post-hoc pairwise comparisons
- **Pharmacokinetic screening**: ADME and drug-likeness (Lipinski, Ghose, Veber, Egan, Muegge) via SwissADME

## Repository structure

```text
dataset/        # original application set + full MoleculeNet HIV pool
models/
  classical/    # SVM, RF, MLP, Mol2Vec+SVM
  sequential/   # RNN, BRNN, GRU, CNN
  graph/        # GCN, GAT, MPNN, GDL
  analysis/     # statistical comparison & sensitivity analysis
  extra/        # additional experiments (VAE)
figures/        # workflow, data partition, and result plots
```

## Models

### Classical machine learning (`models/classical/`)
- `SVM_HIV_5Fold_Statistics_with_docking_prep.ipynb` — Support Vector Machine with Morgan fingerprints
- `RandomForest_HIV_5Fold_Statistics_with_docking_prep.ipynb` — Random Forest with Morgan fingerprints
- `MLP_with_docking_prep.ipynb` — Multilayer Perceptron
- `Mol2Vec_SVM_HIV7_5Fold_Reproducible.ipynb` — Mol2Vec embeddings + SVM

### Sequential deep learning (`models/sequential/`)
- `RNN_HIV_5Fold_Statistics_with_docking_prep_colab.ipynb` — Recurrent Neural Network on SMILES
- `BRNN_HIV_5Fold_Statistics_with_docking_prep_colab.ipynb` — Bidirectional RNN
- `GRU_HIV_5Fold_Statistics_with_docking_prep_colab.ipynb` — Gated Recurrent Unit
- `CNN_HIV_5Fold_Statistics_with_docking_prep_colab.ipynb` — 1D Convolutional Neural Network
- `CNN_HIV_5Fold_Statistics.ipynb` — CNN baseline (without docking prep)

### Graph-based geometric deep learning (`models/graph/`)
- `GCN_HIV_5Fold_Statistics_with_docking_prep_colab.ipynb` — Graph Convolutional Network
- `GAT_HIV_5Fold_Statistics_with_docking_prep_colab.ipynb` — Graph Attention Network
- `MPNN_HIV_5Fold_Statistics_with_docking_prep_colab.ipynb` — Message Passing Neural Network
- `GDL_HIV_docking_prep_colab.ipynb` — Proposed geometric deep learning (GDL) model

### Analysis (`models/analysis/`)
- `hiv_statistical_comparison_12_models.ipynb` — Friedman test and post-hoc comparison across 12 models
- `Inactive_Sampling_Sensitivity_RF_V4_Pure_Fingerprint_Colab.ipynb` — Inactive-sampling sensitivity analysis (RF)

### Extra (`models/extra/`)
- `VAE_HIV_5Fold_Statistics_v3_EarlyStop_AccCurves.ipynb` — Variational Autoencoder (additional experiment)

Each notebook contains training, 5-fold cross-validation, and evaluation code for the corresponding architecture on the HIV bioactivity task.

Data partitioning used in the experimental protocol (stratified train / validation / test split):

<img width="561" height="502" alt="data_partition" src="https://github.com/user-attachments/assets/a322a8ae-9fef-4cb4-b75c-5619b402a80e" />

## Results

Accuracy curves of deep learning and geometric deep learning models:

<img width="502" height="655" alt="acc" src="https://github.com/user-attachments/assets/434d6614-f01e-498f-abf3-c7a880c752b1" />

Loss curves of deep learning and geometric deep learning models:

<img width="505" height="647" alt="loss" src="https://github.com/user-attachments/assets/d5819ec0-dabb-4876-928d-682b7deda298" />

ROC curves for 5-fold cross-validation:

<img width="505" height="647" alt="loss" src="https://github.com/user-attachments/assets/18c6d8e2-2ecb-48ff-84cf-a2de9387a426" />

Mean ROC-AUC comparison across models:

<img width="989" height="490" alt="mean_roc_auc" src="https://github.com/user-attachments/assets/6762533b-d632-49cf-b2e0-8e1d3952e885" />

## Setup Environment

1. Clone this repository to your computer:

    ```bash
    git clone https://github.com/USERNAME/HIV-ADME-Comparative-Evaluation.git
    ```

2. Navigate to the project directory:

    ```bash
    cd HIV-ADME-Comparative-Evaluation
    ```

3. Open the notebooks under `models/` in [Google Colab](https://colab.research.google.com/) or a local Jupyter environment with Python 3 and common ML/DL libraries (e.g., scikit-learn, PyTorch / TensorFlow, RDKit, PyTorch Geometric where required).

## Contributing

If you want to contribute to this project, please follow these steps:

- **Fork:** Fork this repository to your GitHub account.
- **Create a Branch:** Create a new branch to add a new feature or fix a bug.
- **Commit:** Add clear commit messages explaining your changes.
- **Push:** Push your changes to the repository you forked.
- **Pull Request:** Create a pull request on GitHub.

## License

This project is licensed under the [GNU General Public License v3.0](LICENSE).
