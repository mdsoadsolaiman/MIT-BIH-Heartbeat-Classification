# Report-ready limitations

- **Historical data:** MIT-BIH contains ECGs recorded in 1975–1979. Historical acquisition equipment and clinical practice may not represent contemporary devices or populations.
- **Limited independent sample:** The database has 48 recordings from 47 subjects, and the locked test partition has only eight records. Approximately 110,000 beat annotations are not independent patients.
- **Lead heterogeneity:** MLII was available in 46 records; records 102 and 104 required V5. Anatomical lead comparability is therefore not perfectly uniform.
- **Selection bias:** Official documentation identifies 23 randomly selected records and 25 deliberately enriched for less common clinically important arrhythmias. Observed class prevalence is not population disease prevalence.
- **Class imbalance:** N dominates all splits. Minority metrics, rather than accuracy alone, are essential for interpretation.
- **F concentration:** Of 803 mapped F annotations, 735 occur in records 208 and 213. Validation contains 367 F beats, while independent test contains only 26, creating substantial record-composition uncertainty.
- **S and F generalisation:** F is an extreme scarcity and concentration problem. S has 2,037/310/434 train/validation/test beats, so its poor performance instead indicates difficult class separability and inter-record generalisation despite non-trivial support.
- **Validation/test shift:** Macro F1 declined by 0.111 for LR, 0.119 for RF, and 0.006 for CNN. F concentration and differing record morphology likely contribute, but are not established as the sole cause.
- **Uncertain ranking:** RF has the highest point-estimate macro F1, but its record-cluster interval overlaps CNN's marginal interval. With eight test records, the apparent advantage is suggestive rather than conclusive; overlapping intervals do not formally prove equivalence.
- **Accuracy can conceal failure:** CNN has the narrowest record-level accuracy IQR but achieved 0% recall for true S and F beats. Stable accuracy therefore does not demonstrate stable balanced multiclass performance.
- **Fairness evidence absent:** Demographic subgroup performance was not evaluated. Class-specific weakness is not automatically demographic unfairness.
- **No modern external validation:** Results have not been validated on a contemporary external ECG cohort, alternate devices, or distribution shifts.
- **Privacy and governance:** MIT-BIH predates HIPAA. Contemporary identifiable ECG deployment would require applicable privacy, security, clinical governance, and monitoring.
- **Task boundary:** This is annotated heartbeat classification, not complete patient-level arrhythmia diagnosis, treatment guidance, or evidence for autonomous clinical use.
