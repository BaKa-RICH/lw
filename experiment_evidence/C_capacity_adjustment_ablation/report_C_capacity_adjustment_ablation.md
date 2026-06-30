# C. Capacity and Adjustment Ablation

## 实验目的
Check adjacent-gap capacity and space-sensitive adjustment behavior on selected representative rows.

## 使用场景
S02, S06, S01, S08. S01 substitutes for S07 in this task.

## 生成的图表和表格
- `D:\PycharmProjects\ICUS\experiment_evidence\C_capacity_adjustment_ablation\table_capacity_adjustment_ablation.csv`
- `D:\PycharmProjects\ICUS\experiment_evidence\C_capacity_adjustment_ablation\table_capacity_adjustment_ablation.md`

## 核心结果
The C table has 4 rows and excludes S07. It reports Equal split vs Proposed for selected gaps only.

## 关键现象解释
Equal split is derived as Delta/2 on both sides; Proposed f/r is copied from `delta_f_star/delta_r_star` in raw selected rows.

## 数据质量和注意事项
This builder does not run Unweighted QP or Capacity-proportional baselines. S01 is explicitly labeled as the S07 substitute.

## 源数据追踪
| processed artifact | raw source | absolute raw path | processing rule |
| --- | --- | --- | --- |
| D:\PycharmProjects\ICUS\experiment_evidence\C_capacity_adjustment_ablation\table_capacity_adjustment_ablation.csv | S02 selected gap row | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\one_step\S02\gap_rows.csv | derive Equal split and Proposed f/r from selected-row Delta and adjustment values |
| D:\PycharmProjects\ICUS\experiment_evidence\C_capacity_adjustment_ablation\table_capacity_adjustment_ablation.csv | S06 selected gap row | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\one_step\S06\gap_rows.csv | derive Equal split and Proposed f/r from selected-row Delta and adjustment values |
| D:\PycharmProjects\ICUS\experiment_evidence\C_capacity_adjustment_ablation\table_capacity_adjustment_ablation.csv | S01 selected gap row | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\one_step\S01\gap_rows.csv | derive Equal split and Proposed f/r from selected-row Delta and adjustment values |
| D:\PycharmProjects\ICUS\experiment_evidence\C_capacity_adjustment_ablation\table_capacity_adjustment_ablation.csv | S08 selected gap row | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\one_step\S08\gap_rows.csv | derive Equal split and Proposed f/r from selected-row Delta and adjustment values |
| D:\PycharmProjects\ICUS\experiment_evidence\C_capacity_adjustment_ablation\report_C_capacity_adjustment_ablation.md | C table raw rows | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\one_step\S01\gap_rows.csv | explain S01 substitution for S07 without claiming S07 was run |

## 论文写作建议
Use this as Table II. Keep the S01/S07 substitution visible in drafts until a distinct S07 run is intentionally restored.
