# 工业炉平台 V1 接口

## AI 查询

`POST /api/projects/{project_id}/ai-analyses`

请求包含 `analysis_type`、`question`、`artifact_ids`、`execution_ids` 和可选的 `pasted_images`。AI 查询页面使用 `analysis_type: knowledge_lookup` 发起 TRIZ 分析工作流。

响应的 `result.responses` 按工作流保留各阶段内容：

- `workflow_role: knowledge_base_answer`：资料库检索初判。
- `workflow_role: triz_analyst`：DeepSeek TRIZ 方案。
- `workflow_role: triz_challenger`：智谱清言方案质询。

响应的 `fallback_reason` 标记工作流状态。DeepSeek 失败时返回资料初判；智谱清言缺失或失败时返回 DeepSeek TRIZ 方案。

## 静态页面

`GET /` 返回控制台页面。`app/main.py` 会依据 `index.html` 的修改时间刷新内存缓存，因此前端文案调整可由运行中的服务直接加载。
