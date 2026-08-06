# AI Query TRIZ Analysis

Feature Name: ai-query-triz-analysis
Updated: 2026-08-06

## Description

`knowledge_lookup` 查询先基于项目资料生成可追溯的资料初判，豆包输出 TRIZ 方案，DeepSeek 和智谱清言并行输出独立质询。每个 `AiAnalysisRound` 保存一轮结果和触发下一轮的人工意见，最多五轮。

## Components and Interfaces

- `app.ai_client._artifact_answer`: 生成资料初判。
- `app.ai_client._build_triz_solution_prompt`: 组装包含资料初判、前一轮意见和紧凑方案约束的豆包 TRIZ 请求。
- `app.ai_client._build_triz_challenge_prompt`: 组装包含豆包 TRIZ 方案、资料和质询约束的 DeepSeek 或智谱清言请求。
- `app.ai_client._run_knowledge_lookup_flow`: 先调用豆包，再通过 `asyncio.gather` 并行调用 DeepSeek 和智谱清言。
- `app.models.AiAnalysisRound`: 为每个分析会话保存轮次、人工意见、模型响应、状态和版本顺序。
- `app.main._build_ai_analysis_docx`: 生成具有标题、版本、信息卡片和分段配色的标准 Word `.docx` 包。
- `POST /api/projects/{project_id}/ai-analyses/{analysis_id}/rounds`: 用人工意见创建第 2 至第 5 轮。
- `GET /api/projects/{project_id}/ai-analyses/{analysis_id}/rounds/{round_no}/download-docx`: 下载指定版本的 AI 分析 Word 文档。

## Correctness Properties

- 最终报告仅使用当前查询上下文中的项目资料和资料初判。
- 豆包缺失或调用失败时，资料初判继续作为可用答案。
- 响应记录包含 `knowledge_base_answer`、`doubao_triz_analyst`、`deepseek_challenger` 和 `zhipu_challenger` 工作流角色，便于审计。
- 任一质询调用失败时，豆包 TRIZ 方案继续作为主结果。
- 导出文档从持久化的轮次记录生成，包含原始问题、已返回工作流步骤和适用人工意见。

## Error Handling

- DeepSeek TRIZ 阶段输出 700 个汉字以内的紧凑方案；豆包和智谱清言质询各输出 450 个汉字以内的结果。
- 豆包和智谱清言质询使用各自的复核超时配置与零次重试，调用失败时保留 DeepSeek TRIZ 方案。

## Test Strategy

- 使用模拟 DeepSeek、豆包和智谱清言响应验证方案生成和并行质询。
- 验证任一并行质询失败时保留 DeepSeek TRIZ 方案。
- 验证页面包含四段进度条、并行质询文案和按钮运行态。
- 验证 Word 导出接口返回可解压的 `.docx` 文件和完整的方案内容。
- 运行完整 V1 测试套件验证现有 AI 工作流回归。
