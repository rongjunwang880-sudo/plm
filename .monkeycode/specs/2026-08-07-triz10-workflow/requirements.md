# Requirements Document

## Introduction

系统为项目分析人员提供十轮 TRIZ 分析流程。每轮保存独立技术总结、方案和满意度结论，使分析人员能够根据反馈继续迭代或执行方案。

## Glossary

- **TRIZ 轮次**: 一次带有阶段职责、分析输入、技术总结、方案和满意度的可追溯分析记录。
- **满意度**: 分析人员对当前轮次产出作出的满意或继续分析结论。
- **参考资料**: 本地知识库、同类专利、专利评价和相关报道的检索范围说明。

## Requirements

### Requirement 1

**User Story:** AS 项目分析人员, I want a ten-round TRIZ workflow, so that I can complete staged engineering analysis and retain every technical conclusion.

#### Acceptance Criteria

1. WHEN 分析人员创建 `triz10` 分析请求, 系统 SHALL 创建第一轮分析记录。
2. WHEN 当前轮次小于十, 系统 SHALL 支持创建下一轮分析记录。
3. WHEN 系统创建任一轮次, 系统 SHALL 保存轮次编号、阶段职责、输入上下文、技术总结和方案。
4. WHEN 当前轮次为十, 系统 SHALL 提供最终技术总结和执行状态。

### Requirement 2

**User Story:** AS 项目分析人员, I want the first five rounds to follow specified TRIZ analysis responsibilities, so that the analysis progresses from evidence to solutions and contradictions.

#### Acceptance Criteria

1. WHEN 系统创建第一轮, 系统 SHALL 记录本地知识库、同类专利、专利评价和相关报道的检索范围，并输出查险、简单 TRIZ 分析、执行建议和满意度判断。
2. WHEN 系统创建第二轮, 系统 SHALL 输出功能分析技术总结。
3. WHEN 系统创建第三轮, 系统 SHALL 输出因果分析技术总结和逐步形成的方案。
4. WHEN 系统创建第四轮, 系统 SHALL 使用理想解、小人解、聪明小人法、九屏幕法、物场分析和 76 个标准解输出方案与远期分析。
5. WHEN 系统创建第五轮, 系统 SHALL 输出物理矛盾、技术矛盾和逐步形成的方案。

### Requirement 3

**User Story:** AS 项目分析人员, I want to record satisfaction after each round, so that I can continue analysis or execute a satisfactory proposal.

#### Acceptance Criteria

1. WHEN 分析人员提交满意度和反馈, 系统 SHALL 将满意度和反馈保存到当前轮次。
2. WHEN 分析人员选择继续分析且当前轮次小于十, 系统 SHALL 创建下一轮。
3. WHEN 分析人员选择满意, 系统 SHALL 将当前轮次标记为待执行并保留所有前序轮次。
4. WHEN 分析人员选择重新分析, 系统 SHALL 保留当前轮次并创建新的后续轮次。
