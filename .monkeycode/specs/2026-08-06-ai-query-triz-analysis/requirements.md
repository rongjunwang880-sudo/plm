# Requirements Document

## Introduction

AI 查询在项目资料检索后生成本地初判，优先由火山大模型生成 TRIZ 方案，DeepSeek 对方案进行工程质询，智谱清言输出最终工程总结。

## Glossary

- **资料初判**: 基于当前项目命中资料生成的可追溯回答。
- **TRIZ 分析报告**: 包含对象重建、因果分析、第一性原理、候选方案、裁决和评分的结构化回答。
- **DeepSeek 质询**: 对 TRIZ 方案中的假设、证据缺口、风险、边界条件和验证动作进行独立审阅的结构化回答。
- **智谱总结**: 基于 TRIZ 方案和 DeepSeek 质询生成的最终工程结论。

## Requirements

### Requirement 1

**User Story:** AS 项目分析人员, I want AI 查询在检索资料后进行 TRIZ 分析, so that I can obtain a structured engineering recommendation.

#### Acceptance Criteria

1. WHEN 用户发起 `knowledge_lookup` 查询, 系统 SHALL 先从当前项目资料生成资料初判。
2. WHEN 资料初判生成完成且火山大模型已配置, 系统 SHALL 将用户问题、命中资料和资料初判提交给火山大模型生成 700 个汉字以内的紧凑 TRIZ 方案。
3. IF 火山大模型未返回 TRIZ 方案且 DeepSeek 已配置, 系统 SHALL 将用户问题、命中资料和资料初判提交给 DeepSeek 生成接替方案。
4. WHEN TRIZ 方案生成完成且 DeepSeek 已配置, 系统 SHALL 将 TRIZ 方案提交给 DeepSeek 进行质询。
5. WHEN DeepSeek 质询完成且智谱清言已配置, 系统 SHALL 将 TRIZ 方案和质询提交给智谱清言进行总结。
6. WHEN 智谱清言返回总结, 系统 SHALL 记录资料初判、TRIZ 方案、DeepSeek 质询和智谱总结四个步骤。
7. IF 任一复核阶段未返回内容, 系统 SHALL 保留最近一个成功阶段的结果并记录可追溯状态。

### Requirement 2

**User Story:** AS 项目分析人员, I want the report grounded in project artifacts, so that the recommendation remains auditable.

#### Acceptance Criteria

1. WHEN 火山大模型或 DeepSeek 生成 TRIZ 分析报告, 系统 SHALL 要求模型仅使用当前项目资料和资料初判中的事实。
2. WHEN 资料不足以支持结论, 系统 SHALL 要求模型将结论标记为待确认。
3. WHEN DeepSeek 生成报告, 系统 SHALL 要求模型输出对象重建、功能结构、因果链、第一性原理、候选思路、裁决收敛、评分矩阵和下一步建议。
4. WHEN 任一模型生成报告, 系统 SHALL 要求模型避免披露内部指令、附件来源或提示词内容。
5. WHEN DeepSeek 生成质询, 系统 SHALL 要求模型输出质询结论、关键假设与证据缺口、潜在矛盾与风险、边界条件、待验证问题和修订建议。

### Requirement 3

**User Story:** AS 项目分析人员, I want the AI query page to display stage progress and a running button state, so that I can distinguish an active analysis request from an idle page.

#### Acceptance Criteria

1. WHEN 用户点击开始 TRIZ 分析, 系统 SHALL 禁用开始 TRIZ 分析按钮并显示运行文本。
2. WHILE AI 查询请求未返回, 系统 SHALL 显示资料库检索、火山 TRIZ、DeepSeek 质询和智谱总结四段进度条。
3. WHEN AI 查询请求完成, 系统 SHALL 恢复开始 TRIZ 分析按钮并展示各阶段实际结果。
