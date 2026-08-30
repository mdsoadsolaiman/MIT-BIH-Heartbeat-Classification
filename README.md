# MIT-BIH Heartbeat Classification

This project compares three approaches to patient-independent ECG heartbeat classification on the MIT-BIH Arrhythmia Database: Logistic Regression, Random Forest, and a compact one-dimensional CNN. It is a methodological portfolio project, not a clinically validated diagnostic system.

## Objective

Classify annotated heartbeats into four AAMI-style groups: **N** (normal), **S** (supraventricular ectopic), **V** (ventricular ectopic), and **F** (fusion). The analysis emphasizes balanced and per-class performance because the dataset is severely imbalanced.

## Dataset

The [MIT-BIH Arrhythmia Database v1.0.0](https://physionet.org/content/mitdb/1.0.0/) contains 48 half-hour, two-channel ambulatory ECG recordings from 47 subjects. Signals were sampled at 360 Hz. The database is downloaded from PhysioNet and is not committed to this repository.

Records are assigned to fixed, disjoint training, validation, and test sets. Recordings 201 and 202 are kept together because they belong to the same subject. The held-out test set contains eight records.

## Models

1. **Logistic Regression** — 18 engineered rhythm and morphology features with a training-only fitted scaler and a linear classifier.
2. **Random Forest** — the same engineered features with a nonlinear tree ensemble.
3. **CNN** — normalized heartbeat waveforms with representations learned directly by a compact one-dimensional convolutional network.

## Methodology

Expert annotations are mapped to N/S/V/F classes. Q-class annotations are audited and excluded because paced beats dominate that group and cannot support the same patient-independent evaluation design. ECG signals are band-pass filtered from 0.5–40 Hz using a third-order zero-phase Butterworth filter. Each complete heartbeat segment contains 100 samples before and 180 samples after the annotation, followed by beat-wise z-standardization.

Model configuration is selected using validation data. Final metrics are computed once on the fixed held-out test records. Accuracy, macro precision, macro recall, macro F1, weighted F1, Average Precision, ROC-AUC, confusion patterns, and record-level stability are retained for interpretation.

## Results

The values below come from [`results/metrics/model_comparison.csv`](results/metrics/model_comparison.csv).

| Model | Accuracy | Macro precision | Macro recall | Macro F1 | Macro AP |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.690 | 0.494 | 0.611 | 0.455 | 0.630 |
| Random Forest | 0.892 | 0.504 | 0.500 | 0.498 | 0.515 |
| CNN | 0.952 | 0.484 | 0.457 | 0.469 | 0.471 |

The CNN has the highest accuracy, Logistic Regression has the highest macro recall and macro Average Precision, and Random Forest has the highest point-estimate macro F1. These rankings describe different trade-offs; they do not establish one universal best model. RF and CNN record-cluster uncertainty intervals overlap, and only eight independent test records are available.

## Key Findings

- Accuracy alone is misleading under severe class imbalance.
- Logistic Regression detects many abnormal beats but also produces many false positives.
- Random Forest gives the strongest balanced point estimate while retaining high N/V performance.
- CNN performs strongly on N and V but has 0% test recall for S and F.
- Minority-class and cross-patient generalization remain the central limitations.

## Repository Structure

```text
.
├── README.md
├── requirements.txt
├── notebooks/                 # six numbered analysis notebooks
├── data/
│   └── README.md              # download and regeneration policy
├── figures/                   # report and supplementary figures
├── results/
│   ├── metrics/               # final and supporting CSV/JSON outputs
│   └── predictions/           # held-out predictions and probabilities
├── docs/
│   └── MIT_BIH_Heartbeat_Classification_Report.pdf
└── LICENSE_RECOMMENDATION.md
```

## Setup

Python 3.12 was used for the completed analysis.

```bash
python -m venv .venv
```

Activate the environment on Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

Or on macOS/Linux:

```bash
source .venv/bin/activate
```

Install the top-level dependencies:

```bash
python -m pip install -r requirements.txt
```

## Dataset Download

From the repository root:

```bash
python -c "import wfdb; wfdb.dl_database('mitdb', dl_dir='data/raw/mitdb')"
```

The raw database is approximately 104 MB and remains excluded from Git. See [`data/README.md`](data/README.md) for details.

## Execution Order

Run the notebooks from a clean kernel in this order:

1. `notebooks/01_data_eda.ipynb`
2. `notebooks/02_preprocessing_features.ipynb`
3. `notebooks/03_logistic_regression.ipynb`
4. `notebooks/04_random_forest.ipynb`
5. `notebooks/05_cnn.ipynb`
6. `notebooks/06_evaluation.ipynb`

Notebooks 01–02 download/audit the data and create deterministic processed inputs. Notebooks 03–05 train and evaluate the three models. Notebook 06 compares their locked predictions.

## Reproducibility

- Record-level split files are fixed and disjoint.
- The Logistic Regression scaler is fitted on training data only.
- Hyperparameters and early stopping are selected from validation performance.
- Test labels are not used for model selection.
- Random seeds and deterministic settings are recorded where supported.
- Large processed waveform arrays are regenerated by notebook 02 and are not committed.
- Run notebook 04 to regenerate `results/models/random_forest.joblib`; the large serialized model is not committed through normal Git.

The lightweight metrics and complete held-out prediction tables are retained so the final evaluation can be inspected without changing predictions.

## Limitations

The recordings were collected between 1975 and 1979 and represent only 47 subjects. Minority classes are severe and record-concentrated, particularly F. ECG leads are not uniform across all records, demographic metadata are insufficient for a fairness analysis, and only eight independent test records are available. No external or prospective clinical validation was performed.

## Report

The final project report is available at [`docs/MIT_BIH_Heartbeat_Classification_Report.pdf`](docs/MIT_BIH_Heartbeat_Classification_Report.pdf).

## References

- Moody, G. B., & Mark, R. G. (2001). *The impact of the MIT-BIH Arrhythmia Database*. IEEE Engineering in Medicine and Biology Magazine, 20(3), 45–50.
- Goldberger, A. L., et al. (2000). *PhysioBank, PhysioToolkit, and PhysioNet*. Circulation, 101(23), e215–e220.
- MIT-BIH Arrhythmia Database, version 1.0.0. DOI: [10.13026/C2F305](https://doi.org/10.13026/C2F305).

The MIT-BIH data remain subject to the license shown by PhysioNet and are not relicensed by this repository. See `LICENSE_RECOMMENDATION.md` before choosing a license for the project code and original documentation.
