# V4 — Shared well-independent partition and sensor audit

This directory preserves the original CSV artifacts supplied by the researcher for the adjusted V4 protocol, shared by C1 (supervised classification) and C2 (unsupervised anomaly detection). The files were published on November 25, 2026 without changes to their bytes, row order, column names or values.

## Files

| File | Contents |
|---|---|
| [V4_Pocos_CANDIDATA_AJUSTADA.csv](V4_Pocos_CANDIDATA_AJUSTADA.csv) | 40 well identifiers and their fixed subset assignments. |
| [V4_Instancias_CANDIDATA_AJUSTADA.csv](V4_Instancias_CANDIDATA_AJUSTADA.csv) | 1,119 real instances with file name, category, origin, well and subset. |
| [10_Auditoria_535_Arquivos_V4.csv](10_Auditoria_535_Arquivos_V4.csv) | Session 10 audit of the 535 training instances, including sample counts, window counts and processing status. |

## Partition

| Subset (original CSV value) | Instances | Wells |
|---|---:|---:|
| Training (Treino) | 535 | 28 |
| Validation (Validacao) | 176 | 6 |
| Test (Teste) | 408 | 6 |
| Total | 1,119 | 40 |

Relative to V3, WELL-00030 moved from training to validation and WELL-00022 moved from validation to training. All six test wells were preserved: WELL-00001, WELL-00004, WELL-00006, WELL-00010, WELL-00016 and WELL-00019.

The V4 split is fixed and must not be changed in response to classifier scores. Categories 1, 3 and 6 are absent from validation at the file-category level; this does not establish temporal-label coverage or a final list of excluded classes.

## Session 10 audit

The audit used windows of 100 samples with a stride of 50 and homogeneous temporal labels.

| Eligibility criterion | Files with windows | Wells with windows | Windows |
|---|---:|---:|---:|
| All three original sensors complete | 434 | 15 | 220,625 |
| At least two of five sensors complete | 478 | 27 | 321,929 |
| At least one of five sensors complete | 535 | 28 | 342,004 |

The five sensors are P-TPT, T-TPT, P-ANULAR, P-JUS-CKGL and P-MON-CKP.

Audit columns:

- `Arquivo`, `Poco`, `Categoria`: instance metadata.
- `Amostras`: number of temporal records.
- `Janelas_antigas`: windows eligible under the original three-sensor criterion.
- `Janelas_novas`: windows eligible under the at-least-one-of-five-sensors criterion.
- `Janelas_1_sensor`: windows with exactly one complete sensor.
- `Janelas_2_ou_mais`: windows with at least two complete sensors.
- `Status`: audit processing status.

All 535 audit rows have status OK. The audit matches the training manifest exactly by file name, well and category. The two-sensor rule was used in Session 11 to extract 321,929 training feature windows. No V4 classifier performance is implied by these counts.

## Reproduction

See [Session 10](../10_Preprocessamento_Final_C1.ipynb) and [Session 11](../11_Construcao_Matriz_Atributos_V4_C1.ipynb). The notebooks use Google Drive paths: place the two manifest CSVs in the expected `PROTOCOLO_V4_COMPARTILHADO` directory and the audit CSV in the project working directory (or adapt the paths). The original 3W time series and generated feature Parquet files are not included here.

## Integrity

SHA-256 checksums of the original CSV files:

```
7d04b59fbfcd45d4c9e56bcad60d78e53db198aab37ec4c4e06586712ac8935f  V4_Pocos_CANDIDATA_AJUSTADA.csv
9ed8a86eae8feaee73531d5ffbfc817bfc3ee8598035baaada91135cb98137e2  V4_Instancias_CANDIDATA_AJUSTADA.csv
c43fd9fe00b45b460af85eaad0652ef06084f30405b874153c5c0a269f8e9aaa  10_Auditoria_535_Arquivos_V4.csv
```

Checks performed before publication: no missing values in the three CSVs, unique well identifiers in the well manifest, unique file names in the instance and audit manifests, real-only instances, no cross-subset well overlap, exact agreement between manifests, expected subset counts and exact correspondence between training and audit records. Published Git blob hashes were verified against the original uploaded files.
