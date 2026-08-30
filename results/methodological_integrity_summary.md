# Methodological integrity summary

- Genuine MIT-BIH Arrhythmia Database v1.0.0 files were used; all 48 primary records loaded and the supplied checksum manifest was verified.
- ECG headers, channels, raw annotations, and signal integrity were audited before modelling. MLII was selected by name where available; records 102 and 104 used their documented V5 alternative.
- The AAMI-style N/S/V/F mapping was fixed before modelling. Q was excluded before modelling for documented independent-record viability reasons.
- Splitting occurred at record level, not heartbeat level. Train, validation, and test records are disjoint, and the known shared-subject records 201/202 remain together.
- Fixed-length segmentation excludes only annotations lacking a complete 100-sample pre-window and 180-sample post-window. Forty-two boundary beats were excluded; no synthetic replacement was used.
- Logistic Regression scaling was fitted from training features only. Validation data were used for candidate selection, and the locked test set was loaded after each configuration was selected.
- Locked predictions retain beat and record provenance. Their class ordering, probability alignment, probability sums, row counts, and regenerated metrics were independently verified.
- Uncertainty uses 2,000 record-cluster bootstrap resamples of the eight test records, not individual-beat resampling. These intervals are descriptive because the number of independent records is small.
- McNemar analysis is supplementary: paired predictions are beat-level, while beats within the same ECG record are correlated, so it is not definitive patient-level statistical evidence.
- Error examples use the highest-confidence eligible Random Forest prediction with beat ID as a deterministic tie-break; examples were not selected visually.
- Random seeds, record IDs, preprocessing settings, model configurations, model artifacts, and prediction provenance are retained. Exact numerical reproducibility may still vary across TensorFlow hardware and oneDNN implementations.
- No demographic fairness claim is made because demographic subgroup performance was not analysed.

## Reproducibility statement

Locked prediction files were independently re-evaluated and reproduced the saved metrics exactly. Preprocessing checks confirmed that learned Logistic Regression scaling parameters match a training-partition-only fit. Random seeds, fixed split IDs, preprocessing settings, model configurations, saved artifacts, and beat-level provenance are retained to support reproducibility without claiming identical floating-point results on every hardware platform.
