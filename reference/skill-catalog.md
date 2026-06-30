# ICUS Skill 目录

本目录总结 `.agents/skills` 中的项目本地 skills，以及每个 skill 在撰写 ICUS 论文时最适合承担的工作。

## 快速决策表

| 写作需求 | 最适合的 Skill(s) | 一句话用途 |
|---|---|---|
| 最终 IEEE LaTeX 修改、BibTeX、编译错误 | `ieee-paper` | 在 IEEEtran 规则下，安全地把修改落到 `main.tex` 和 `refs.bib` 中。 |
| 题目模糊或研究问题不清楚 | `academic-research-suite` | 在起草前收敛研究问题，并暴露缺失的假设。 |
| 文献综述策略或引用完整性 | `academic-research-suite`, `paper-spine-citation` | 设计综述逻辑，并把每个 claim 连接到经过验证的支撑。 |
| 整篇手稿的 motivation 和结构 | `paper-spine`, `paper-spine-research`, `paper-spine-rewrite` | 围绕清晰的 motivation、证据链和章节计划重建论文。 |
| 从零散材料起草 | `paper-spine-build` | 把笔记、图、结果和参考文献转化为论文蓝图。 |
| 模拟审稿人批评 | `academic-research-suite`, `zhou_teacher_academic_paper_polishing` | 找出可能的审稿人异议、创新性风险和薄弱的证据连接。 |
| 控制/MAS 学术英语润色 | `lv-teacher-paper-polisher` | 改进控制理论风格、贡献表述和段落过渡。 |
| 严谨的跨领域论文润色 | `zhou_teacher_academic_paper_polishing` | 收紧问题定义、贡献边界和面向审稿的逻辑。 |
| PaperSpine 工作流设置 | `paper-spine-ui`, `paper-spine-intake` | 在研究、重写或构建阶段之前配置 PaperSpine 运行。 |
| PaperSpine 输出审计 | `paper-spine-audit` | 检查 motivation、引用、理由矩阵和最终产物是否完整。 |

## 最终手稿与构建

| Skill | 一句话用途 |
|---|---|
| `ieee-paper` | `main.tex`、`refs.bib`、IEEEtran 格式、LaTeX 编译错误、图/表布局和 Git hygiene 的最终权威。 |
| `paper-spine-latex` | 在任何选定内容迁移到 ICUS 的 `main.tex` 之前，检查 PaperSpine 生成的 LaTeX 草稿。 |

## 研究设计与审阅

| Skill | 一句话用途 |
|---|---|
| `academic-research-suite` | 用于研究问题收敛、文献综述规划、引用检查、模拟同行评审、rebuttal 规划和实验设计的通用学术工作流 skill。 |
| `zhou_teacher_academic_paper_polishing` | 面向控制、优化、博弈论、分布式算法、制导、交通和基于学习的轨迹规划论文的审阅导向润色 skill。 |

## Motivation、结构与证据主线

| Skill | 一句话用途 |
|---|---|
| `paper-spine` | 编排完整 PaperSpine 工作流，从配置到研究、引用支撑、重写/构建、LaTeX、翻译和审计。 |
| `paper-spine-research` | 在起草前研究本地参考文献、目标场景规范、SOTA gaps、风格模式和 motivation 选项。 |
| `paper-spine-citation` | 构建 claim 级引用支持库，使背景、相关工作和讨论中的 claim 拥有明确的证据锚点。 |
| `paper-spine-rewrite` | 通过逻辑图、章节蓝图、重写矩阵和与 motivation 对齐的修改计划，重做已有草稿。 |
| `paper-spine-build` | 从笔记、图、实验结果、PDF 和局部草稿等零散材料构建手稿计划。 |

## 风格、润色与可读性

| Skill | 一句话用途 |
|---|---|
| `lv-teacher-paper-polisher` | 以 Lv-teacher 风格润色控制和多智能体系统写作，尤其是摘要、引言、贡献、过渡和结论。 |
| `zhou_teacher_academic_paper_polishing` | 以 Zhou-teacher 审阅风格强化学术严谨性、逻辑过渡、贡献表述和证据连接型论证。 |
| `paper-spine-humanize` | 在保持技术含义和 claim 边界的同时，减少机械化或模板化措辞。 |
| `paper-spine-translate` | 在需要时，为 PaperSpine 中间产物和最终结构生成中文阅读/翻译包。 |

## PaperSpine 配置与质量门

| Skill | 一句话用途 |
|---|---|
| `paper-spine-ui` | 启动 PaperSpine 配置界面，用于选择工作流、场景、语言、材料和输出选项。 |
| `paper-spine-intake` | 验证或创建 `paper_rewriting_output/paper_spine_config.json`，使 PaperSpine 拥有清晰的任务合同。 |
| `paper-spine-audit` | 审计 PaperSpine 产物，检查 motivation confirmation 缺失、浅层理由矩阵、薄弱引用库和不完整输出。 |
| `paper-spine-update` | 从上游源码 checkout 维护项目本地 PaperSpine 安装。 |

## 推荐工作流

| 场景 | Skill 链 | 目的 |
|---|---|---|
| 已有草稿需要重大重构 | `academic-research-suite` -> `paper-spine-research` -> `paper-spine-citation` -> `paper-spine-rewrite` -> `lv-teacher-paper-polisher` 或 `zhou_teacher_academic_paper_polishing` -> `ieee-paper` | 诊断草稿，重建 motivation 和证据，润色关键章节，然后把修改落到 IEEE LaTeX 中。 |
| 只有材料，还没有完整论文 | `academic-research-suite` -> `paper-spine-build` -> `paper-spine-citation` -> `paper-spine-latex` -> `ieee-paper` | 收窄研究框架，组装论文蓝图，附上引用，然后生成并验证 LaTeX。 |
| 投稿就绪检查 | `academic-research-suite` -> `zhou_teacher_academic_paper_polishing` -> `paper-spine-audit` -> `ieee-paper` | 模拟审稿人异议，收紧严谨性，审计论文逻辑，并验证最终构建/布局。 |
| 引用和 related-work 清理 | `academic-research-suite` -> `paper-spine-citation` -> `ieee-paper` | 检查 claims 是否得到支撑，然后安全地添加经过验证的 BibTeX 条目和 citations。 |
| 不改变 claims 的语言润色 | `lv-teacher-paper-polisher` 或 `zhou_teacher_academic_paper_polishing` -> `paper-spine-humanize` -> `ieee-paper` | 改进学术风格和可读性，然后只把技术上安全的修改应用到源码。 |

## 边界规则

外部 skills 可以提出研究结构、审阅、重写、引用支撑和润色文本。最终项目文件仍然是：

```text
D:\PycharmProjects\ICUS\main.tex
D:\PycharmProjects\ICUS\refs.bib
```

使用 `ieee-paper` 进行最终修改、BibTeX 集成和本地 TeX Live 2026 构建验证。
