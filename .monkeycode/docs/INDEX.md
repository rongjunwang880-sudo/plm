# 工业炉平台 V1 文档

## 文档目录

- `ARCHITECTURE.md`：应用结构和 AI 查询工作流。
- `INTERFACES.md`：主要 HTTP 接口与 AI 查询响应约定。
- `DEVELOPER_GUIDE.md`：本地运行、测试和 AI 运行时配置说明。
- `操作说明书.md`：现有操作说明书的 Markdown 版本。

## 当前 AI 流程

`AI 查询 -> DeepSeek TRIZ -> 豆包与智谱清言并行质询` 依次执行资料库检索、DeepSeek TRIZ 方案生成，以及双模型独立质询。
