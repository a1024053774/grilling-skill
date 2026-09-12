# Grilling Skill

一个用于**持续拷问计划、决策和想法**的 Agent Skill。

它会把讨论拆成一轮一轮的问题，沿着决策树逐步检查目标、约束、取舍、风险和隐含假设。与只依赖聊天上下文的提问不同，它会把已确认内容、未决问题和当前问题 frontier 持久化到工作区的 `.grilling/` 账本中，因此长对话发生上下文压缩后仍然可以继续。

## 适合什么时候用

- 评审产品或项目计划
- 压力测试技术架构和方案取舍
- 找出需求中的隐含假设和遗漏约束
- 在开始实现前确认真正要解决的问题

## 怎么用

安装：

```bash
npx skills add a1024053774/grilling-skill@grilling -g -y
```

然后直接对 Agent 说：

```text
用 grilling skill 帮我压力测试这个方案：……
```

Skill 会逐轮提问，每个问题都会给出推荐答案；你回答后，它会更新账本并继续下一轮。所有关键分支确认完毕后，它会生成最终基线，等待你确认，再进入后续执行。

账本默认位于：

```text
.grilling/ACTIVE.md
.grilling/<topic-slug>.md
```

仓库地址：[github.com/a1024053774/grilling-skill](https://github.com/a1024053774/grilling-skill)

## License

MIT
