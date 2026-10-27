# Agent 任务执行与自动化评测平台

`agent-task-eval` 是基于 Inspect AI 整理的 Agent 任务评测项目，用于组织文件处理、代码执行和工具调用任务，集中查看任务评分与执行轨迹。

## 适用问题

- 人工逐条测试难以重复：将任务输入、参考结果和评分规则放入同一评测任务，便于批量执行和比较。
- 最终回答无法说明执行过程：通过样本日志查看模型消息、工具参数、工具返回和异常，辅助定位失败步骤。
- 模型或提示词更新后缺少回归入口：复用同一批任务，对比任务得分、耗时和 Token 用量。

## 评测流程

任务与样本配置 → 模型执行与工具调用 → 结果评分 → 样本日志 → 失败分析

评测过程使用 Inspect AI 的 Task、Dataset、Solver 和 Scorer 抽象。多轮工具交互、沙箱、评分器及日志查看器均由底层框架提供，可据此配置具体应用的评测任务。

## 可查看的示例

| 入口 | 内容 |
| --- | --- |
| `examples/hello_world.py` | 最小任务定义与精确匹配评分，适合检查模型连接和运行环境 |
| `examples/tool_use.py` | 工具调用任务示例 |
| `examples/code_execution.py` | Docker 沙箱中的代码执行示例，需额外配置 Docker 和相应模型 |
| `examples/bridge/` | 外部 Agent 框架的评测接入示例 |

这些示例保留自上游，具体任务实现和依赖以对应文件为准。

## 本地使用

需要 Python 3.10 或以上版本。在 Windows PowerShell 中进入仓库目录后执行：

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -e .
.\.venv\Scripts\inspect.exe --help
```

按照所选模型提供商的要求，在本机配置访问凭据；不要将密钥提交到仓库。下面以 OpenAI 模型为命令格式示例，运行时可替换为自己可访问的模型：

```powershell
.\.venv\Scripts\inspect.exe eval examples/hello_world.py --model openai/gpt-5
.\.venv\Scripts\inspect.exe view
```

工具调用示例入口：

```powershell
.\.venv\Scripts\inspect.exe eval examples/tool_use.py --model openai/gpt-5
```

完整模型配置、沙箱和评分器说明见 [Inspect AI 文档](https://inspect.aisi.org.uk/)。

## 来源与维护范围

本仓库由 ww68y 维护，基于 [UKGovernmentBEIS/inspect_ai](https://github.com/UKGovernmentBEIS/inspect_ai) 的 `71e00d28fc` 版本整理。原始评测框架由 UK AI Security Institute 开发。

本次整理包括独立的仓库展示名称、中文应用场景说明和示例运行入口；核心代码沿用上游实现。原项目说明保存在 [README.upstream.md](README.upstream.md)，原有提交历史与作者信息予以保留。

## 许可证

沿用上游 [MIT License](LICENSE)，保留原有版权声明与许可文本。
