# Skills

可复用的 Codex skills。目前包含 PROQAID：面向多 Agent 软件开发的迭代治理体系。

## PROQAID

![PROQAID 体系图](./proqaid-system-diagram.png)

PROQAID 由 Product、Review、Orchestrator、Quality、Architecture、Interface、Delivery 七个职责组成。它关注的不是任务排期，而是长期 AI 开发中的目标、边界、证据、并发写入和文档卫生。

核心规则：

- Architecture、Interface 和 Quality 在开发前冻结模块边界、公开契约和测试所有权，Review 随后执行一次 Design Freeze 审计。
- Orchestrator 负责调度、依赖路由、串行集成和异常处理；生产代码只由具备独立写入范围的临时 worker 修改。
- 安全的独立任务尽量占满可用并发槽，共享文件和依赖链保持串行。
- Worker 负责模块 TDD；Review 只 fresh 验证高风险变化；Quality 每个集成波次验证一次跨模块业务闭环；最终阶段运行一份完整证据集。
- 治理文档必须有明确下游读者和决策用途。默认不维护 agent status 文件，也不保留重复的 inbox/outbox 副本。
- 测试绿色不能替代业务闭环、真实运行证据和最终审计。

详细规则见 [proqaid/SKILL.md](./proqaid/SKILL.md)。

## 安装

将 `proqaid/` 复制到 Codex skills 目录：

```text
~/.codex/skills/proqaid/
```

然后在新任务中使用 `$proqaid`，或直接要求 Codex 使用 PROQAID 运行软件项目迭代。

PROQAID 支持 Lite 和 Full。只有用户明确指定 Lite 时才使用 Lite，否则默认 Full；运行中不得自动切换级别。
