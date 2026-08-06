# 工业炉平台 V1 架构

## 概述

工业炉平台 V1 是基于 FastAPI 的工程项目管理与分析应用。应用提供项目资料管理、计算执行、审批报告、工程分析和 AI 查询等能力。

前端页面由 `app/static/index.html` 提供。`app/main.py` 承担 HTTP 接口、权限判断、资料解析和分析记录持久化。SQLite 或由 `DATABASE_URL` 指定的 SQLAlchemy 数据库保存业务记录。

## 核心模块

- `app/main.py`：FastAPI 应用入口、业务 API、静态页面和资料上传解析。
- `app/ai_client.py`：AI Provider 配置、项目资料检索、TRIZ 方案生成与方案质询。
- `app/industrial_furnace_knowledge.py`：工业炉资料关键词和匹配规则。
- `app/lightrag_retrieval.py`：LightRAG 检索适配。
- `app/db.py`、`app/models.py`、`app/schemas.py`：数据库与请求响应模型。
- `app/static/index.html`：单页控制台与 AI 查询结果展示。

## AI 查询工作流

1. `knowledge_lookup` 根据当前项目资料生成资料初判。
2. DeepSeek 接收用户问题、命中资料和资料初判，输出 TRIZ 方案。
3. 豆包与智谱清言同时接收 TRIZ 方案，分别输出质询结论、证据缺口、风险、边界条件和待验证问题。
4. 页面在请求期间显示资料库检索、DeepSeek TRIZ、豆包质询和智谱清言质询四段斜角进度条，并将开始按钮切换为禁用的运行状态。
5. 用户可将已保存的 AI 分析下载为 Word `.docx` 文档，文档包含问题与各工作流步骤结果。

## 运行时配置

`app/runtime_config.py` 从项目根目录 `.env` 或 `.env.local` 加载 AI Provider 配置。配置文件包含凭据，保持在版本控制之外。
