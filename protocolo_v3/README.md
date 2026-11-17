
# V3 — Experimental Partitioning Protocol

This folder contains the candidate V3 experimental partitioning protocol for the Petrobras 3W Dataset (version 2.0.0).

The protocol is shared between C1 and C2.

## Partition Summary

| Subset | Wells | Real Instances |
|---|---:|---:|
| Training | 28 | 528 |
| Validation | 6 | 183 |
| Test | 6 | 408 |
| Total | 40 | 1119 |

The partitions are grouped by well identifier, with no well overlap between subsets.

Classes 1, 3, 6 and 8 are absent from the fixed validation subset.

The protocol remains a candidate and its methodological limitations are documented.

## Files

- 01_Pocos_V3.csv — Well identifiers and partitions.
- 02_Instancias_Reais_V3.csv — Real instances and partitions.
- 03_Inventario_Completo.csv — Complete dataset inventory.
- 04_GroupKFold_V3.csv — Internal grouped cross-validation.
- 05_Divisao_Aleatoria.csv — Random-split comparison.
- 06_Cobertura_V3.csv — Class coverage.
- 07_Diagnostico_GroupKFold.csv — Cross-validation diagnostics.
- 08_LEIA_ME.txt — Original protocol documentation.
