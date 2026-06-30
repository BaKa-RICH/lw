# B. Feasibility Validation

## 实验目的
Verify that S01 closes the chain from candidate gap evaluation to a selected developed gap and an executable MV trajectory.

## 使用场景
S01 only: `X = [-180, -90, -25, 30, 110, 190, 250]`.

## 生成的图表和表格
- `D:\PycharmProjects\ICUS\experiment_evidence\B_feasibility_validation\table_candidate_gap_evaluation_S01.csv`
- `D:\PycharmProjects\ICUS\experiment_evidence\B_feasibility_validation\table_candidate_gap_evaluation_S01.md`

## 核心结果
S01 has 6 candidate gaps in the table. The selected row is `gap4` with interval `[30, 110]`.

## 关键现象解释
The selected row traces the candidate score table to the developed gap and then to `trajectory_with_acceleration.csv`, including acceleration `a` for the merge vehicle and mainline vehicles.

## 数据质量和注意事项
All numbers in the table are copied from generated one-step raw gap rows. The report does not use S07 or S05 as evidence.

## 源数据追踪
| processed artifact | raw source | absolute raw path | processing rule |
| --- | --- | --- | --- |
| D:\PycharmProjects\ICUS\experiment_evidence\B_feasibility_validation\table_candidate_gap_evaluation_S01.csv | S01 gap rows | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\one_step\S01\gap_rows.csv | select and format all S01 candidate gap rows |
| D:\PycharmProjects\ICUS\experiment_evidence\B_feasibility_validation\table_candidate_gap_evaluation_S01.md | S01 gap rows | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\one_step\S01\gap_rows.csv | markdown rendering of the same table |
| D:\PycharmProjects\ICUS\experiment_evidence\B_feasibility_validation\report_B_feasibility_validation.md | S01 gap rows and trajectory | D:\PycharmProjects\CORMC\artifacts\icus论文实验 raw artifacts\shared_runs\icus_v1\one_step\S01\trajectory_with_acceleration.csv | human-readable report with raw path trace |

## 论文写作建议
Use this section for Table I and the feasibility paragraph: emphasize candidate gaps as developable resources and point readers to the executable trajectory artifact.
