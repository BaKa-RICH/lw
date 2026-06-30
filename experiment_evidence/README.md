# ICUS Experiment Evidence

- run_id: `icus_v1`
- Fixed scenarios: `S01, S02, S03, S06, S08`.
- `S07` is not run in this package; S01 represents the front-side preference case for this task.
- `S05` is not used for D because it is not treated as representative successful merging evidence here.

## Directories

- `B_feasibility_validation/`: S01 candidate gap evaluation table and report.
- `C_capacity_adjustment_ablation/`: selected-gap capacity and adjustment ablation table and report.
- `D_external_algorithm_comparison/`: OneStep vs RiosTorres/FIFO candidate table, figures, rationale, and report.

## Raw Artifact Root

- `D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1`

## Rerun

```powershell
$env:PYTHONIOENCODING='utf-8'
python scripts/icus_experiment_evidence.py --run-id icus_v1
```
