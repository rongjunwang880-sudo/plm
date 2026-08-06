# 工业炉平台 V1 开发指南

## 环境

项目使用 Python、FastAPI、SQLAlchemy、HTTPX 与 Pytest。依赖定义在 `AA_VISIBLE_FILES/industrial_furnace_platform_v1/requirements.txt`。

AI 运行时配置由项目根目录 `.env` 或 `.env.local` 提供。使用项目级 AI 环境变量，配置文件中保存的凭据不得写入源代码、测试输出或文档。

## 启动

在 `AA_VISIBLE_FILES/industrial_furnace_platform_v1` 目录运行：

```bash
python3 -m uvicorn app.main:app --host 0.0.0.0 --port 8000
```

## 测试

在同一目录运行：

```bash
python3 -m pytest
```

AI 查询改动至少应覆盖 DeepSeek TRIZ 方案成功、DeepSeek 失败、智谱清言质询成功和智谱清言质询失败四类路径。
