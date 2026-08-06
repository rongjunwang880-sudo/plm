# 用户指令记忆

本文件记录了用户的指令、偏好和教导，用于在未来的交互中提供参考。

## 格式

### 用户指令条目
用户指令条目应遵循以下格式：

[用户指令摘要]
- Date: [YYYY-MM-DD]
- Context: [提及的场景或时间]
- Instructions:
  - [用户教导或指示的内容，逐行描述]

### 项目知识条目
Agent 在任务执行过程中发现的条目应遵循以下格式：

[项目知识摘要]
- Date: [YYYY-MM-DD]
- Context: Agent 在执行 [具体任务描述] 时发现
- Category: [运维部署|构建方法|测试方法|排错调试|工作流协作|环境配置]
- Instructions:
  - [具体的知识点，逐行描述]

## 去重策略
- 添加新条目前，检查是否存在相似或相同的指令
- 若发现重复，跳过新条目或与已有条目合并
- 合并时，更新上下文或日期信息
- 这有助于避免冗余条目，保持记忆文件整洁

## 条目

V1 独立工程验证方法
- Date: 2026-06-04
- Context: Agent 在执行工业炉计算平台 V1 后端骨架开发时发现
- Category: 测试方法
- Instructions:
  - V1 独立工程位于 `/workspace/AA_VISIBLE_FILES/industrial_furnace_platform_v1`，右侧可见目录优先查看 `AA_VISIBLE_FILES`。
  - V1 工程验证命令为 `python3 -m compileall app tests` 和 `python3 -m pytest`。
  - V1 工程依赖安装命令为 `pip install --break-system-packages -r requirements.txt`。

页面功能模块拆分偏好
- Date: 2026-06-04
- Context: 用户指出不同功能一直放在同一个页面会影响边界清晰度
- Instructions:
  - 设计或修改工业炉计算平台页面时，应按业务模块拆分视图，例如项目总览、计算执行、审批报告、横向对比、项目资料、AI 联合分析。
  - 轻量单页工程也应使用导航和模块视图区分功能，避免把不同业务功能堆在一个连续页面里。
  - 项目管理页面用于项目台账和名目维护；项目选择、名目选择、计算树节点和状态输出应归入计算执行模块。

根目录工业炉平台验证方法
- Date: 2026-06-18
- Context: Agent 在执行当前仓库根目录工业炉平台整改时发现
- Category: 测试方法
- Instructions:
  - 根目录工程依赖安装命令为 `pip install --break-system-packages -r requirements.txt`。
  - 根目录工程当前接口回归命令为 `python3 -m pytest tests/test_api.py`。

AI 查询逐文档检索偏好
- Date: 2026-06-24
- Context: 用户要求 AI 查询先逐个文档查找再回答
- Instructions:
  - 处理项目资料问答时，优先按文档逐篇顺序检索，再汇总回答。
  - 出炉温度、入炉温度、炉底机械传动等参数题优先逐份技术性能表查找。
  - 冷却水管路设计、操作维护、注意事项类问题优先逐份 Word 正文或技术说明查找。

根目录 Web 服务启动方式
- Date: 2026-07-20
- Context: Agent 在启动当前仓库根目录工业炉计算平台时发现
- Category: 环境配置
- Instructions:
  - FastAPI 服务入口为 `app.main:app`，启动命令为 `python3 -m uvicorn app.main:app --host 0.0.0.0 --port 8000`。
  - README 中引用的 `app_server.py` 已不在当前工作区。

V1 资料管理工程运行依赖
- Date: 2026-07-20
- Context: Agent 在启动 V1 Python 工程时发现
- Category: 环境配置
- Instructions:
  - V1 运行目录为 `AA_VISIBLE_FILES/industrial_furnace_platform_v1`，服务启动命令为 `python3 -m uvicorn app.main:app --host 0.0.0.0 --port 8000`。
  - 除 requirements.txt 外，AI 适配模块还依赖 `tencentcloud-sdk-python`。

变更范围控制偏好
- Date: 2026-07-25
- Context: 用户要求继续开发计算平台时提出
- Instructions:
  - 修改前先确认本轮功能范围，避免对密码、二进制文件和服务器配置进行未授权操作。
