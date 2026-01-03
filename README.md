# Structurally Driven Discovery of Safe Anti-Methanogenic Molecules

## Overview

Atmospheric methane (CH₄) is a major short-lived climate forcer, and mitigation of ruminant enteric methane represents one of the most impactful near-term climate interventions. This repository contains all computational pipelines, data processing scripts, machine learning models, and analysis workflows used in the accompanying manuscript:

**Uncovering structural priors for inhibition of Ni(I) in methyl-coenzyme M reductase to identify non-toxic anti-methanogenic compounds**

We present an integrated, structure-informed, AI-enabled framework for identifying safe, permeable, and biologically effective anti-methanogenic molecules, grounded in the biophysics of methyl-coenzyme M reductase (MCR) and its F430 Ni(I) cofactor.

*This repository focuses exclusively on computational discovery and analysis; experimental protocols are described in the accompanying manuscript.*

---

## Key Contributions

* Structural biophysics of methanogenesis inhibition
  * Explicit modeling of ligand–F430–Ni(I) interactions in methyl-coenzyme M reductase (MCR)
  * Binding affinity and stoichiometric flooding analyses
* Contrastive learning on molecular embeddings
  * Functional clustering of 16 known inhibitors with ~54,000 bovine-linked metabolites
  * ChemBERTa-based molecular representations
* Multi-factor inhibitor prioritization
  * Thermodynamic favorability (docking-derived ΔG)
  * Bacterial membrane permeability
  * Human toxicity prediction
  * Commercial availability and biosynthetic feasibility
* Experimental and systems-level validation
  * In vitro rumen fermentation assays
  * Community metabolic modeling of the bovine gut

---

## Repository Structure

```
.
├── data/
│   ├── known_inhibitors/
│   ├── bovine_metabolites/
│   └── fermentation_assays/
├── structures/
│   └── MCR_F430/
├── modeling/
│   ├── docking/
│   ├── contrastive_learning/
│   ├── permeability/
│   └── toxicity/
├── metabolic_modeling/
├── notebooks/
├── figures/
├── environment/
│   ├── conda_environment.yml
│   └── requirements.txt
├── LICENSE
└── README.md
```

---

## Methodological Workflow

1. Structure preparation of the MCR–F430–Ni(I) complex
2. Docking and thermodynamic screening of candidate molecules
3. Contrastive learning and functional clustering
4. Multi-objective filtering (affinity, permeability, toxicity, availability)
5. Experimental validation and metabolic modeling

---

## Installation

### Conda Environment

```
conda env create -f Modeling/contrastive_learning/environment.yml
conda activate methano_env
```

*Note: GPU-enabled PyTorch and PyTorch Geometric should be installed separately depending on target hardware (CPU/macOS vs CUDA/HPC).*

### Key Dependencies

- python=3.8
- ipykernel
- scikit-learn
- pytorch
- seaborn
- rdkit
- pytorch_geometric
- transformers
- plotly
- numpy
- pandas
- libgfortran5
- openblas
- openbabel
- xtb
- conda-forge::crest

## Reproducing Results

### Molecular Docking


flowchart TD
    A[Activate Conda Environment] --> B[Set Working Directory]

    B --> C[Prepare MCR Structure<br/>(PDB: 5G0R)]
    C --> C1[Retain Chains A, D, E, F]
    C --> C2[Retain F430 (F43) from Chain A]
    C --> C3[Remove Solvent & Non-standard Residues]

    C --> D[Add Hydrogens to Protein<br/>(REDUCE)]

    D --> E[Prepare Receptor PDBQT<br/>(MGLTools)]
    E --> E1[Split Alternate Conformations]
    E --> E2[Select Optimal Conformation]
    E --> E3[Generate protein_only_A.pdbqt]

    E --> F[Prepare F430 Cofactor]
    F --> F1[Generate F43.pdbqt]
    F --> F2[Remove Torsions → Rigid Cofactor]
    F --> F3[Create F43_rigid.pdbqt]

    F --> G[Merge Protein + F430]
    G --> G1[protein_F43_ready.pdbqt]

    G --> H[Ligand Preparation]
    H --> H1[Generate 3D from SMILES]
    H --> H2[Add Hydrogens]
    H --> H3[Geometry Optimization (UFF)]
    H --> H4[Generate Ligand PDBQT]

    H --> I[Define Docking Grid]
    I --> I1[Grid Centered on Ni of F430]

    I --> J[AutoDock Vina Docking]
    J --> J1[Rigid Docking]
    J --> J2[Exhaustiveness = 32]
    J --> J3[20 Binding Modes]

    J --> K[Docking Output]
    K --> K1[docked_out.pdbqt]
    K --> K2[docking.txt]

    K --> L[Pose Extraction]
    L --> L1[Split All Poses]
    L --> L2[Extract Specific Poses]

## Contrastive Learning

Detailed reproduction of results and analysis for the **contrastive learning** component of this study can be found in the `modeling/contrastive_learning/` directory.

## Cowmunity1.0

The repository to access all metabolic modeling code and analysis of the study can be found here in this repo `./metaboli_modelling `or **https://github.com/mohammadrezazargar/cowmunity1.0/**

---

## Data Availability

All curated datasets used in the manuscript are included or referenced within the repository - ./data/.

---

## Citation

**If you use this repository, please cite:**

Aryee, R., *et al.* (2026).  *Uncovering structural priors for inhibition of Ni(I) in methyl coenzyme M reductase to identify non-toxic anti-methanogenic compounds* . **Manuscript under review.**

---

## License

MIT License

---

## Contact

Dr. Ratul Chowdhury - **ratul@iastate.edu** or Randy Aryee - **raryee5@iastate.edu**
Chemical & Biological Engineering
Iowa State University
