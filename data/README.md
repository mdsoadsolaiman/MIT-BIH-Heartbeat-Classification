# Data

This project uses the **MIT-BIH Arrhythmia Database, version 1.0.0**, published by PhysioNet.

- Dataset page: https://physionet.org/content/mitdb/1.0.0/
- Version DOI: https://doi.org/10.13026/C2F305
- Contents: 48 half-hour, two-channel ambulatory ECG recordings from 47 subjects
- Sampling frequency: 360 Hz per channel

## Download

From the repository root, after installing `requirements.txt`:

```bash
python -c "import wfdb; wfdb.dl_database('mitdb', dl_dir='data/raw/mitdb')"
```

The downloaded files should be located at `data/raw/mitdb/`.

The complete raw database is approximately 104 MB and is excluded from Git. It remains governed by the license and attribution requirements shown on the PhysioNet dataset page.

## Generated Data

Run notebooks 01 and 02 in order to recreate the processed artifacts. Large generated files—including `cnn_X_train.npy`, `cnn_X_val.npy`, `cnn_X_test.npy`, engineered split CSVs, and beat-level metadata—are excluded from Git because they can be deterministically regenerated from the raw database and fixed split files.

Lightweight audit summaries and the fixed train/validation/test record lists are retained because they document the data selection and split without duplicating the source signals.

