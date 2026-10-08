# IT爱学堂-慕课-Agent Skills 多平台应用实战「完结无密」
获课：aixuetang.xyz/24483/

打通跨平台工程化——从碎片化困境到统一部署
Agent Skills 的跨平台部署面临一个现实困境：Claude Code 读取 .claude/skills/，Cursor 读取 .cursor/skills/，Codex 读取 .codex/skills/，GitHub Copilot 读取 .github/skills/，而厂商中立标准使用 .agents/skills/。同一套技能内容需要维护五份不同平台的副本，这是跨平台部署首先要解决的技术问题。

单一事实源：跨平台部署的核心原则
跨平台部署的关键设计原则是“单一事实源加自动分发”。agentpack 工具的思路颇具代表性：开发者只需维护一份 SKILL.md 文件和一份 agentpack.yaml 配置文件，运行一条命令即可为每个平台生成正确的发布包，包括 Claude Code 的 plugin.json 清单和 marketplace.json，以及 Cursor、Codex、Copilot 等平台各自的技能目录结构。

这个工具在构建前会进行验证，检查 description 缺失、name 与文件夹名不匹配、命名不合规范等常见问题，同时自动适配各平台方言——宿主平台不支持的 frontmatter 字段会被丢弃并报告，Cursor 的 globs 字段会映射为 paths，slash 命令会转换为手动调用技能。

另一种方案是“符号链接加同步脚本”，将 Skills 存储在独立的 Git 仓库中，各平台的技能目录通过符号链接或自动化同步工具指向该仓库，实现一处修改、处处生效。

MCP 通用接入：让技能跨越平台边界
对于已经使用 MCP 协议的智能体，AgentSkill MCP 提供了一条更通用的路径。它是一个通用 MCP 服务器，让任何支持 MCP 的智能体应用都能使用 Anthropic 官方格式的 Agent Skills，同时实现渐进式披露。这意味着 Cursor、Codex、Kilo Code、Roo Code 等 MCP 兼容平台无需修改技能文件即可直接调用，实现零锁定。

该工具的配置也足够轻量，通过指定 skills 目录参数即可接入，支持项目级和全局技能目录的自动发现。

技能同步与持续进化
跨平台部署不仅是分发问题，还涉及版本同步和经验共享。SkillClaw 通过共享存储层实现了跨 Agent、跨设备、跨用户的技能同步，统一使用 SKILL.md 格式存储，支持 Hermes、Codex、Claude Code、OpenClaw 等主流框架。其核心机制是在 Agent 和 LLM 之间插入 Client Proxy，默默记录每一次交互的完整轨迹，再由 Evolve Server 分析这些记录，发现技能膨胀、重复和过期问题，自动触发去重、补全和优化。

值得注意的是，技能进化不是每一条记录都值得固化成技能，单次偶然现象会被过滤掉。只有当多个用户、多台设备、多个 Agent 在相似场景中反复遇到同类问题时，才会触发技能更新。这种基于真实使用数据的进化机制，让技能在多平台部署后能够持续自我优化。
