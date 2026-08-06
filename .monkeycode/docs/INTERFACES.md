# 工业炉平台 V1 接口

## AI 查询

`POST /api/projects/{project_id}/ai-analyses`

请求包含 `analysis_type`、`question`、`artifact_ids`、`execution_ids` 和可选的 `pasted_images`。AI 查询页面使用 `analysis_type: knowledge_lookup` 发起 TRIZ 分析工作流。

响应的 `result.responses` 按工作流保留各阶段内容：

- `workflow_role: knowledge_base_answer`：资料库检索初判。
- `workflow_role: doubao_triz_analyst`：豆包 TRIZ 方案。
- `workflow_role: deepseek_challenger`：DeepSeek 方案质询。
- `workflow_role: zhipu_challenger`：智谱清言方案质询。

响应的 `fallback_reason` 标记工作流状态。豆包失败时返回资料初判；任一并行质询失败时保留豆包 TRIZ 方案。响应中的 `rounds` 保存当前会话的版本列表。

`POST /api/projects/{project_id}/ai-analyses/{analysis_id}/rounds`

请求体包含必填字段 `human_feedback`。系统将前一轮模型方案、质询结论和人工意见作为上下文，创建第 2 至第 5 轮评审记录。

`GET /api/projects/{project_id}/ai-analyses/{analysis_id}/download-docx`

返回第 1 轮 Word `.docx` 文件，内容包含分析问题和所有已返回的工作流结果。

`GET /api/projects/{project_id}/ai-analyses/{analysis_id}/rounds/{round_no}/download-docx`

返回指定轮次的版本化 Word `.docx` 文件，包含轮次、模型结论和该轮吸收的人工意见。

## 静态页面

`GET /` 返回控制台页面。`app/main.py` 会依据 `index.html` 的修改时间刷新内存缓存，因此前端文案调整可由运行中的服务直接加载。
