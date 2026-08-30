# Report-ready deployment comparison

| Criterion | Logistic Regression | Random Forest | CNN |
|---|---|---|---|
| Test accuracy | 0.690 | 0.892 | **0.952** |
| Test macro F1 | 0.455 | **0.498** | 0.469 |
| Test weighted F1 | 0.785 | 0.913 | **0.938** |
| Test macro AP | **0.630** | 0.515 | 0.471 |
| Minority behaviour | S recall 0.931 but precision 0.114; F F1 0.003 | Strong N/V; S F1 0.119; F F1 0.003 | Strong N/V; 0% recall for true S and F beats |
| Representation | 18 engineered features | Same 18 engineered features | Learned directly from 280-sample waveform |
| Model size | 1.6 KB plus 1.0 KB scaler | 27.9 MB | 292 KB |
| Transparency | Standardised coefficients | Permutation importance; nonlinear rules less transparent | Lowest feature-level transparency |
| Record accuracy IQR | 0.336 | 0.081 | 0.029 |

No model dominates all criteria. CNN provides the highest overall accuracy and weighted F1, RF the highest point-estimate macro F1, and Logistic Regression the highest macro Average Precision. LR's high S AP indicates useful ranking despite an unsuitable balanced-weight hard-decision boundary; RF offers the most favourable fixed-label balance; CNN provides compact waveform learning but misses every true S and F test beat.

RF's macro-F1 advantage over CNN is uncertain at record level because their record-cluster bootstrap intervals overlap and the test contains only eight independent records. This overlap indicates substantial uncertainty but is not a formal paired proof of no difference. Beat-level McNemar analysis favoured CNN in overall correctness, although within-record clustering makes that result supplementary rather than definitive patient-level evidence.

None of the models has adequate evidence for autonomous clinical diagnosis. Any future use would require contemporary external validation, calibration and threshold analysis, privacy/governance controls, distribution-shift monitoring, and human clinical oversight as decision support or screening assistance.
