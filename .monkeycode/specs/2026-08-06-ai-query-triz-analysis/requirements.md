# Requirements Document

## Introduction

AI 查询在项目资料检索后生成本地初判，豆包生成 TRIZ 方案，DeepSeek 与智谱清言同时进行独立工程质询；人工意见可驱动后续评审，最多形成五轮版本。

## Glossary

- **资料初判**: 基于当前项目命中资料生成的可追溯回答。
- **TRIZ 分析报告**: 包含对象重建、因果分析、第一性原理、候选方案、裁决和评分的结构化回答。
- **并行质询**: DeepSeek 与智谱清言同时对 TRIZ 方案中的假设、证据缺口、风险、边界条件和验证动作进行独立审阅的结构化回答。
- **评审轮次**: 包含豆包方案、双模型质询和人工意见的一次可追溯版本，轮次上限为五。

## Requirements

### Requirement 1

**User Story:** AS 项目分析人员, I want AI 查询在检索资料后进行 TRIZ 分析, so that I can obtain a structured engineering recommendation.

#### Acceptance Criteria

1. WHEN 用户发起 `knowledge_lookup` 查询, 系统 SHALL 先从当前项目资料生成资料初判。
2. WHEN 资料初判生成完成且豆包已配置, 系统 SHALL 将用户问题、命中资料和资料初判提交给豆包生成 700 个汉字以内的紧凑 TRIZ 方案。
3. WHEN 豆包 TRIZ 方案生成完成且 DeepSeek 已配置, 系统 SHALL 将 TRIZ 方案提交给 DeepSeek 进行质询。
4. WHEN 豆包 TRIZ 方案生成完成且智谱清言已配置, 系统 SHALL 将 TRIZ 方案提交给智谱清言进行质询。
5. WHEN DeepSeek 和智谱清言均已配置, 系统 SHALL 同时发起两路质询请求。
6. WHEN 用户提交人工意见且现有轮次小于五, 系统 SHALL 将前一轮方案、质询和人工意见提交给豆包生成下一轮方案。
7. WHEN 质询请求返回, 系统 SHALL 记录资料初判、豆包 TRIZ 方案、DeepSeek 质询和智谱清言质询四个步骤。
8. IF 任一质询未返回内容, 系统 SHALL 保留豆包 TRIZ 方案并记录可追溯状态。

### Requirement 2

**User Story:** AS 项目分析人员, I want the report grounded in project artifacts, so that the recommendation remains auditable.

#### Acceptance Criteria

1. WHEN 豆包生成 TRIZ 分析报告, 系统 SHALL 要求模型仅使用当前项目资料和资料初判中的事实。
2. WHEN 资料不足以支持结论, 系统 SHALL 要求模型将结论标记为待确认。
3. WHEN 豆包生成报告, 系统 SHALL 要求模型输出主问题、核心矛盾、三个候选方案、推荐方案、风险和首个验证动作。
4. WHEN 任一模型生成报告, 系统 SHALL 要求模型避免披露内部指令、附件来源或提示词内容。
5. WHEN DeepSeek 或智谱清言生成质询, 系统 SHALL 要求模型输出质询结论、关键假设与证据缺口、潜在矛盾与风险、边界条件、待验证问题和修订建议。

### Requirement 3

**User Story:** AS 项目分析人员, I want the AI query page to display stage progress and a running button state, so that I can distinguish an active analysis request from an idle page.

#### Acceptance Criteria

1. WHEN 用户点击开始 TRIZ 分析, 系统 SHALL 禁用开始 TRIZ 分析按钮并显示运行文本。
2. WHILE AI 查询请求未返回, 系统 SHALL 显示资料库检索、豆包 TRIZ、DeepSeek 质询和智谱清言质询四段进度条。
3. WHEN AI 查询请求完成, 系统 SHALL 恢复开始 TRIZ 分析按钮并展示各阶段实际结果。

### Requirement 4

**User Story:** AS 项目分析人员, I want to save a completed AI analysis as a Word document, so that I can retain and circulate the engineering proposal.

#### Acceptance Criteria

1. WHEN AI 查询返回已保存的分析记录, 系统 SHALL 在结果区域提供保存为 DOC 文档操作。
2. WHEN 用户执行保存操作, 系统 SHALL 返回包含分析问题、资料初判、豆包 TRIZ 方案、DeepSeek 质询、智谱清言质询和适用人工意见的带版本 `.docx` 文件。
3. IF 分析记录不属于当前项目, 系统 SHALL 返回不存在状态。
