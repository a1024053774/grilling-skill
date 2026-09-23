# Grilling Skill

一个面向通用 **Agent 项目**的持续拷问 Skill。

它会把从想法到运行维护的关键决策拆成多轮问题，检查目标、用户、约束、模型与工具、检索与记忆、权限、副作用、评测、发布和运营风险。每一轮都会将确认内容、未决问题、证据和当前 frontier 写入账本，长对话发生上下文压缩后仍能恢复。

Grilling 只产出决定，不做实现：会话在去往目的地的路径清楚、结果写入 canonical artifact 并完成交接后结束，实现、测试运行和发布放到后续会话。它只在用户点名时启用：用自然语言说"用 grilling"或输入 `/grilling` 都可以，其他情况下不会主动介入。

## 流程

1. **确定目的地**：本次拷问要交付什么，例如 `intent.md`、`spec.md`、`plan.md` 或一项锁定的决定。目的地决定范围。
2. **广度扫描**：先整体扫一遍，把发现的内容分成三类：能精确表述的问题、暂时说不清的“迷雾”（Not yet specified）、超出目的地的范围外事项（Out of scope）。
3. **判断规模**：没有迷雾且一次会话能完成时，直接在对话中拷问，不创建账本；否则创建或恢复 `.grilling/` 决策地图。
4. **分轮提问**：每轮问 frontier 上最能解锁后续决定的问题（约五个以内），每个问题附类型、“为什么现在问”和推荐答案。
5. **交接**：逐项核对账本，没有遗留的 `pending`、`inferred` 或 `unknown`（已列为有负责人的风险除外），由负责人确认后写明下一步在哪里、由谁完成。

问题分五种类型：

| 类型 | 由谁定 | 适用情况 |
| --- | --- | --- |
| `decide` | 用户 | 默认类型，讨论即可定下 |
| `compare` | 用户 | 关键设计有两到三个可信方案，需要比较代价和取舍 |
| `prototype` | 用户 | “该长什么样、该怎么表现”靠讨论定不下来，先做最便宜的原型再选 |
| `research` | Agent | 决定依赖某个可以查到的事实 |
| `task` | Agent 或用户 | 做决定前必须先完成的准备工作，如开通权限、导入样例数据 |

Agent 不替用户在原型方案中选定胜者；只有用户明确要求时，才用多个 Agent 并行做候选方案。

## 适用范围

- 从问题或事故整理 `intent.md`
- 从 intent 评审 Agent 的 `spec.md`
- 在实现前拷问 `plan.md`
- 检查 Agent 的评测、工具调用、权限和真实后置状态
- 讨论部署门禁、回滚、监控、成本和持续改进

Skill 不绑定具体模型、框架、业务领域或仓库结构，也不会把自己的账本当成项目唯一事实源；项目已有的 canonical ledger 或 `intent/spec/plan` 仍然优先。

## 使用

安装：

```bash
npx skills add a1024053774/grilling-skill@grilling -g -y
```

然后直接告诉 Agent：

```text
用 grilling skill 帮我从 intent 开始压力测试这个 Agent 项目：……
```

如果已有阶段 artifact，也可以指定入口：

```text
读取现有的 spec.md，用 grilling skill 检查设计、权限、工具副作用和评测缺口。
```

需要账本时，会话状态保存在：

```text
.grilling/ACTIVE.md
.grilling/<topic-slug>.md
```

## 设计依据

流程参考 [Anthropic 的 AI-Native SDLC playbook](https://claude.com/blog/the-ai-native-sdlc-playbook)：每个阶段产出可读、可版本控制的 artifact，由下一阶段读取，并在人工确认的门禁后推进。

决策地图、迷雾、范围外事项和问题类型参考了 Matt Pocock 的 [wayfinder](https://github.com/mattpocock/skills/blob/main/skills/engineering/wayfinder/SKILL.md)：地图只做索引，不重复存放决定；能精确表述的问题才进入 frontier。

仓库地址：[github.com/a1024053774/grilling-skill](https://github.com/a1024053774/grilling-skill)

## License

MIT
