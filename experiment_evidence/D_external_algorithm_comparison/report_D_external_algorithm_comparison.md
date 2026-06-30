# D. External Algorithm Comparison

## 实验目的
Compare OneStep with RiosTorres/FIFO in representative successful-merging candidates.

## 使用场景
S01, S02, S03, S06, S08. S07 is not run; S05 is not used for D.

## 生成的图表和表格
- `D:\PycharmProjects\ICUS\experiment_evidence\D_external_algorithm_comparison\table_external_comparison_candidates.csv`
- `D:\PycharmProjects\ICUS\experiment_evidence\D_external_algorithm_comparison\table_external_comparison_candidates.md`
- `D:\PycharmProjects\ICUS\experiment_evidence\D_external_algorithm_comparison\selected_scenarios_rationale.md`
- `D:\PycharmProjects\ICUS\experiment_evidence\D_external_algorithm_comparison\figure_index.md`

## 核心结果
- S02: recommended candidate
- S03: recommended candidate
- S06: recommended candidate
- S08: recommended candidate

## 关键现象解释
The candidate rule requires both methods to complete and then checks affected mainline vehicles, mainline acceleration energy, and recovery time without automatically choosing the prettiest result.

## 数据质量和注意事项
S01 external comparison may reuse the existing manual artifact; provenance is recorded in `source_manifest.json`.

## 源数据追踪
| processed artifact | raw source | absolute raw path | processing rule |
| --- | --- | --- | --- |
| D:\PycharmProjects\ICUS\experiment_evidence\D_external_algorithm_comparison\table_external_comparison_candidates.csv | S01 metrics | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\external_comparison\S01\metrics.csv | copy selected comparison metrics into Table III rows |
| D:\PycharmProjects\ICUS\experiment_evidence\D_external_algorithm_comparison\table_external_comparison_candidates.csv | S02 metrics | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\external_comparison\S02\metrics.csv | copy selected comparison metrics into Table III rows |
| D:\PycharmProjects\ICUS\experiment_evidence\D_external_algorithm_comparison\table_external_comparison_candidates.csv | S03 metrics | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\external_comparison\S03\metrics.csv | copy selected comparison metrics into Table III rows |
| D:\PycharmProjects\ICUS\experiment_evidence\D_external_algorithm_comparison\table_external_comparison_candidates.csv | S06 metrics | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\external_comparison\S06\metrics.csv | copy selected comparison metrics into Table III rows |
| D:\PycharmProjects\ICUS\experiment_evidence\D_external_algorithm_comparison\table_external_comparison_candidates.csv | S08 metrics | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\external_comparison\S08\metrics.csv | copy selected comparison metrics into Table III rows |
| D:\PycharmProjects\ICUS\experiment_evidence\D_external_algorithm_comparison\selected_scenarios_rationale.md | scenario rationale | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\external_comparison\S01\metrics.csv | apply fixed S01/S07/S05 policy and candidate checks |
| D:\PycharmProjects\ICUS\experiment_evidence\D_external_algorithm_comparison\figure_index.md | comparison figures | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\external_comparison\S01\time_space_trajectory_comparison.png | index canonical comparison PNGs for every generated scenario |

## 论文写作建议
Use Table III for candidate comparison and use `selected_scenarios_rationale.md` before deciding which scenarios enter the final manuscript text.
