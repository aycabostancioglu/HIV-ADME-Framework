# HIV Bioactivity Comparative Evaluation Framework

- Das, Bihter, et al. **“A Comparative Evaluation Framework Integrating Machine Learning and Deep Learning Models with ADME-Based Pharmacokinetic Assessment for HIV-Related Compounds.”** *Pharmaceuticals* (MDPI), 2026.
- Authors: Bihter Das, Harun Uslu, Ayca Bostancioglu, Bunyamin Goktas, Yunus Santur, Seval Yilmaz, and Ibrahim Turkoglu

Predicting the molecular biological activity of Human Immunodeficiency Virus (HIV) is critical for accelerating early-stage drug discovery. Most existing studies focus only on prediction metrics and overlook pharmacokinetic suitability and drug-likeness. This repository provides a unified experimental protocol comparing classical machine learning, deep learning, and geometric deep learning models on the MoleculeNet HIV dataset, followed by ADME and drug-likeness assessment of top candidate compounds and complementary molecular docking of the prioritized GDL1 compound.

## Dataset

The original application dataset contains **3,043 unique compounds** (**1,443 active** and **1,600 inactive**). The full MoleculeNet HIV pool is provided under `dataset/` for reference. In a sensitivity analysis, we generated **30 independent chemically matched alternative inactive subsets** from the MoleculeNet HIV inactive pool while keeping the active compounds fixed.

<img width="1404" height="2078" alt="flowchart" src="https://github.com/user-attachments/assets/edb27f46-0029-4762-8ffb-5e53aff61b02" />


- **Best model (GDL)**: ROC-AUC **0.956 ± 0.015**
- **Strong baselines**: GRU (**0.930 ± 0.017**) and Random Forest (**0.927 ± 0.023**)
- **Statistical validation**: Friedman test (*p* < 0.001) with post-hoc pairwise comparisons
- **Pharmacokinetic screening**: ADME and drug-likeness (Lipinski, Ghose, Veber, Egan, Muegge) via SwissADME
- **Molecular docking**: GDL1 evaluated against HIV-1 protease (1AJV), integrase (1QS4), and reverse transcriptase (2ZD1)

## Repository structure

```text
dataset/        # original application set + full MoleculeNet HIV pool
models/
  classical/    # SVM, RF, MLP, Mol2Vec+SVM
  sequential/   # RNN, BRNN, GRU, CNN
  graph/        # GCN, GAT, MPNN, GDL
  analysis/     # statistical comparison & sensitivity analysis
  extra/        # additional experiments (VAE)
figures/        # workflow, performance, ADME, and docking figures
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

## ADME Analysis Results

For each of the 12 predictive models, the two highest-ranked compounds from the independent test set (ranked by predicted probability) were characterized with SwissADME. The selected chemical structures, BOILED-Egg localization, and oral-bioavailability physicochemical space are shown below.

Chemical structures of the prioritized compounds:

<img width="1011" height="1327" alt="molecul" src="https://github.com/user-attachments/assets/a4e47682-93fd-4c08-be64-fac18adecfdc" />

BOILED-Egg model of all selected compounds:

<img width="1388" height="807" alt="boiled_egg" src="https://github.com/user-attachments/assets/1d5ca953-eb7f-4b7b-b8f8-9f279a1992f2" />

Predicted physicochemical space for oral bioavailability:

<img width="1388" height="1110" alt="physicochemical" src="https://github.com/user-attachments/assets/ec7c95f5-90eb-4680-bbef-45fada1d40e7" />

### Drug-likeness, water solubility, and pharmacokinetic properties

| Comp. No | Lipinski | Ghose | Veber | Egan | Muegge | LogS | Class | GI abs. | F |
|---|---|---|---|---|---|---|---|---|---|
| SVM1 | + | + | + | + | + | -4.45 | Moderately | High | 0.55 |
| SVM2 | + | + | + | + | + | -4.01 | Moderately | High | 0.55 |
| RF1 | + | - | + | + | + | -5.77 | Moderately | High | 0.55 |
| RF2 | + | + | + | + | + | -2.06 | Soluble | High | 0.55 |
| MLP1 | + | + | + | + | + | -3.38 | Soluble | High | 0.55 |
| MLP2 | + | - | + | - | + | -5.78 | Moderately | Low | 0.55 |
| BRNN1 | + | + | + | + | + | -3.82 | Soluble | High | 0.55 |
| BRNN2 | + | + | + | + | + | -2.84 | Soluble | High | 0.55 |
| CNN1 | + | + | + | + | + | -3.57 | Soluble | High | 0.55 |
| CNN2 | + | + | + | + | + | -3.22 | Soluble | High | 0.55 |
| Mol2Vec-1 | + | - | - | - | - | -6.61 | Poorly | Low | 0.11 |
| Mol2Vec-2 | - | - | - | - | - | -5.26 | Moderately | Low | 0.17 |
| GDL1 | + | + | + | + | + | -4.00 | Soluble | High | 0.56 |
| GDL2 | + | + | + | + | + | -3.41 | Soluble | High | 0.55 |
| GRU1 | + | + | + | + | + | -4.99 | Moderately | High | 0.55 |
| GRU2 | + | - | + | - | + | -5.78 | Moderately | Low | 0.55 |
| RNN1 | + | + | + | + | + | -2.95 | Soluble | High | 0.55 |
| RNN2 | + | + | + | + | + | -2.13 | Soluble | High | 0.55 |
| GAT1 | + | + | + | + | + | -3.68 | Soluble | High | 0.55 |
| GAT2 | + | + | + | + | + | -3.58 | Soluble | High | 0.55 |
| GCN1 | + | + | + | + | + | -3.77 | Soluble | High | 0.55 |
| GCN2 | + | + | + | + | + | -4.48 | Moderately | High | 0.55 |

### Physicochemical and lipophilicity properties

| Comp. No | MW | Fsp3 | RB | HBA | HBD | MR | TPSA | cLogP |
|---|---|---|---|---|---|---|---|---|
| SVM1 | 352.79 | 0.20 | 3 | 4 | 1 | 92.11 | 100.37 | 2.79 |
| SVM2 | 306.34 | 0.14 | 5 | 4 | 1 | 81.31 | 100.37 | 2.24 |
| RF1 | 485.48 | 0.36 | 10 | 3 | 1 | 128.02 | 64.09 | 3.44 |
| RF2 | 311.33 | 0.71 | 3 | 6 | 2 | 81.49 | 96.79 | 0.67 |
| MLP1 | 332.33 | 0.07 | 4 | 6 | 2 | 93.21 | 128.25 | 1.71 |
| MLP2 | 457.28 | 0.14 | 4 | 7 | 1 | 97.34 | 123.54 | 4.05 |
| BRNN1 | 236.27 | 0.07 | 2 | 3 | 1 | 69.80 | 54.56 | 2.63 |
| BRNN2 | 214.31 | 0.50 | 1 | 2 | 0 | 69.39 | 27.03 | 2.38 |
| CNN1 | 250.75 | 0.25 | 1 | 1 | 0 | 71.62 | 45.53 | 3.19 |
| CNN2 | 237.36 | 0.54 | 3 | 2 | 2 | 70.21 | 71.06 | 2.84 |
| Mol2Vec-1 | 563.34 | 0.09 | 5 | 9 | 4 | 128.52 | 174.65 | 3.39 |
| Mol2Vec-2 | 581.66 | 0.73 | 6 | 10 | 2 | 149.91 | 177.44 | 2.80 |
| GDL1 | 355.84 | 0.38 | 6 | 4 | 1 | 91.97 | 89.93 | 3.24 |
| GDL2 | 386.53 | 0.64 | 7 | 4 | 1 | 124.19 | 55.89 | 2.28 |
| GRU1 | 452.53 | 0.26 | 7 | 6 | 2 | 128.41 | 123.76 | 3.42 |
| GRU2 | 457.28 | 0.14 | 4 | 7 | 1 | 97.34 | 123.54 | 4.16 |
| RNN1 | 235.31 | 0.45 | 0 | 2 | 2 | 65.66 | 89.88 | 2.15 |
| RNN2 | 281.29 | 0.58 | 3 | 6 | 1 | 70.20 | 76.30 | 0.65 |
| GAT1 | 277.30 | 0.08 | 3 | 4 | 0 | 72.17 | 88.34 | 2.29 |
| GAT2 | 261.30 | 0.08 | 3 | 3 | 0 | 71.48 | 82.10 | 2.38 |
| GCN1 | 292.31 | 0.08 | 4 | 4 | 1 | 76.51 | 100.37 | 1.93 |
| GCN2 | 352.79 | 0.20 | 3 | 4 | 1 | 92.11 | 100.37 | 2.79 |
| MPNN1 | 230.20 | 0.08 | 0 | 2 | 1 | 65.25 | 47.11 | 2.14 |
| MPNN2 | 459.45 | 0.00 | 4 | 10 | 3 | 111.61 | 183.34 | 2.75 |

Abbreviations: **Fsp3** = Fraction Csp3; **RB** = number of rotatable bonds; **MR** = molar refractivity; **TPSA** = topological polar surface area; **F** = bioavailability score.

Comparative ADME analysis showed that high predictive performance did not necessarily correspond to favorable predicted pharmacokinetic properties. Compounds prioritized by GDL (GDL1, GDL2) exhibited balanced MW/LogP profiles, Lipinski compliance, high GI absorption, and high QED scores, and were therefore selected for downstream docking.

## Molecular Docking Results

The highest-ranked GDL compound (**GDL1**) was docked against three major HIV-1 therapeutic targets using AutoDock4 and AutoDock Vina: HIV-1 protease (PDB: 1AJV), HIV-1 integrase (PDB: 1QS4), and HIV-1 reverse transcriptase (PDB: 2ZD1).

| Target (PDB ID) | Estimated Ki | AutoDock4 Dock Score | AutoDock Vina Dock Score |
|---|---|---|---|
| HIV-1 Protease (1AJV) | 665.18 nM | -8.43 | -8.0 |
| HIV-1 Integrase (1QS4) | 29.40 µM | -6.18 | -5.6 |
| HIV-1 Reverse Transcriptase (2ZD1) | 163.49 nM | -9.26 | -7.9 |

Dock score represents the predicted binding free energy (kcal/mol). Among the three targets, HIV-1 reverse transcriptase showed the most favorable predicted binding affinity (Ki = 163.49 nM; AutoDock4 = -9.26 kcal/mol).

2D binding interactions of GDL1 with HIV-1 reverse transcriptase:

<img width="453" height="355" alt="HIV_reverse_Dock" src="https://github.com/user-attachments/assets/f462f397-0e27-425f-898a-0c8e764a6ce1" />

3D binding interactions of GDL1 with HIV-1 reverse transcriptase:

<img width="453" height="269" alt="3D_HIC_reverse_dock" src="https://github.com/user-attachments/assets/642c00e1-8d63-443f-bc84-f5906720e691" />

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
