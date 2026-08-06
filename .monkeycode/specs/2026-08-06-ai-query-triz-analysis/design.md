# AI Query TRIZ Analysis

Feature Name: ai-query-triz-analysis
Updated: 2026-08-06

## Description

`knowledge_lookup` 查询先基于项目资料生成可追溯的资料初判，优先由火山大模型输出 TRIZ 方案，DeepSeek 输出方案质询，智谱清言输出最终工程总结。

## Components and Interfaces

- `app.ai_client._artifact_answer`: 生成资料初判。
- `app.ai_client._build_triz_solution_prompt`: 组装包含资料初判、命中资料和紧凑方案约束的火山 TRIZ 请求。
- `app.ai_client._build_triz_challenge_prompt`: 组装包含 TRIZ 方案、资料和质询约束的 DeepSeek 请求。
- `app.ai_client._build_triz_summary_prompt`: 组装包含 TRIZ 方案、DeepSeek 质询和总结约束的智谱清言请求。
- `app.ai_client._run_knowledge_lookup_flow`: 依次调用火山、DeepSeek 和智谱清言，保留每一步的工作流角色和回退原因。

## Correctness Properties

- 最终报告仅使用当前查询上下文中的项目资料和资料初判。
- DeepSeek 缺失或调用失败时，资料初判继续作为可用答案。
- 响应记录包含 `knowledge_base_answer`、`volc_triz_analyst`、`deepseek_challenger` 和 `zhipu_summarizer` 工作流角色，便于审计。
- 火山 TRIZ 阶段失败时，`deepseek_triz_fallback` 生成接替方案。

## Error Handling

- 火山 TRIZ 阶段响应为空或失败时，DeepSeek 生成接替方案。
- 火山 TRIZ 阶段输出 700 个汉字以内的紧凑方案，使用 12 秒上限；DeepSeek 接替方案使用 30 秒上限；DeepSeek 质询和智谱总结输出 450 个汉字以内的结果，使用 18 秒上限及零次重试，调用失败时返回最近一个成功阶段的结果。

## Test Strategy

- 使用模拟火山、DeepSeek和智谱清言响应验证完整顺序。
- 验证火山失败时 DeepSeek 生成接替方案。
- 验证页面包含四段进度条和按钮运行态。
- 运行完整 V1 测试套件验证现有 AI 工作流回归。
