# AI Query TRIZ Analysis

Feature Name: ai-query-triz-analysis
Updated: 2026-08-06

## Description

`knowledge_lookup` 查询先基于项目资料生成可追溯的资料初判，DeepSeek 输出 TRIZ 方案，豆包和智谱清言并行输出独立质询。

## Components and Interfaces

- `app.ai_client._artifact_answer`: 生成资料初判。
- `app.ai_client._build_triz_solution_prompt`: 组装包含资料初判、命中资料和紧凑方案约束的 DeepSeek TRIZ 请求。
- `app.ai_client._build_triz_challenge_prompt`: 组装包含 TRIZ 方案、资料和质询约束的豆包或智谱清言请求。
- `app.ai_client._run_knowledge_lookup_flow`: 先调用 DeepSeek，再通过 `asyncio.gather` 并行调用豆包和智谱清言，保留每一步的工作流角色和状态。

## Correctness Properties

- 最终报告仅使用当前查询上下文中的项目资料和资料初判。
- DeepSeek 缺失或调用失败时，资料初判继续作为可用答案。
- 响应记录包含 `knowledge_base_answer`、`deepseek_triz_analyst`、`doubao_challenger` 和 `zhipu_challenger` 工作流角色，便于审计。
- 任一质询调用失败时，DeepSeek TRIZ 方案继续作为主结果。

## Error Handling

- DeepSeek TRIZ 阶段输出 700 个汉字以内的紧凑方案；豆包和智谱清言质询各输出 450 个汉字以内的结果。
- 豆包和智谱清言质询使用各自的复核超时配置与零次重试，调用失败时保留 DeepSeek TRIZ 方案。

## Test Strategy

- 使用模拟 DeepSeek、豆包和智谱清言响应验证方案生成和并行质询。
- 验证任一并行质询失败时保留 DeepSeek TRIZ 方案。
- 验证页面包含四段进度条、并行质询文案和按钮运行态。
- 运行完整 V1 测试套件验证现有 AI 工作流回归。
