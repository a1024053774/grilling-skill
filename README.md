# Grilling Skill

一个面向通用 **Agent 项目**的持续拷问 Skill。

它会把从想法到运行维护的关键决策拆成多轮问题，检查目标、用户、约束、模型与工具、检索与记忆、权限、副作用、评测、发布和运营风险。每一轮都会将确认内容、未决问题、证据和当前 frontier 写入账本，长对话发生上下文压缩后仍能恢复。

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

会话状态默认保存在：

```text
.grilling/ACTIVE.md
.grilling/<topic-slug>.md
```

## 设计依据

流程参考 [Anthropic 的 AI-Native SDLC playbook](https://claude.com/blog/the-ai-native-sdlc-playbook)：每个阶段产出可读、可版本控制的 artifact，由下一阶段读取，并在人工确认的门禁后推进。

仓库地址：[github.com/a1024053774/grilling-skill](https://github.com/a1024053774/grilling-skill)

## License

MIT
