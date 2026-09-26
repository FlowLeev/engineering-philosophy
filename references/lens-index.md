# 工程哲学镜头索引

本索引用于低成本扫描，不包含原则正文。先判断项目命中哪些问题，再完整读取对应镜头文件。编号是稳定引用，不代表优先级。

## 15 个互斥镜头

| 镜头 | 唯一问题 | 命中信号 | 原则文件 |
|---|---|---|---|
| V 价值与边界 | 为什么做，怎样算成功 | 立项、需求冲突、范围不清、指标替代目标 | [lens-value-boundary.md](lens-value-boundary.md) |
| K 证据与不确定性 | 我们凭什么相信 | 假设多、估算不稳、技术可行性未知、需要试验 | [lens-evidence-uncertainty.md](lens-evidence-uncertainty.md) |
| E 经济与取舍 | 稀缺资源怎样分配 | 方案选型、预算/工期取舍、投资优先级 | [lens-economics-tradeoffs.md](lens-economics-tradeoffs.md) |
| S 范围与简单性 | 现在最少需要做什么 | 过度设计、抽象争议、依赖或概念快速增加 | [lens-scope-simplicity.md](lens-scope-simplicity.md) |
| A 架构与分解 | 系统怎样划分责任 | 多模块、边界重构、耦合、依赖方向 | [lens-architecture-decomposition.md](lens-architecture-decomposition.md) |
| I 接口与契约 | 部件之间怎样正确交互 | API、协议、类型、输入校验、重试或错误语义 | [lens-interfaces-contracts.md](lens-interfaces-contracts.md) |
| D 状态与数据 | 什么是真实，如何保存和演化状态 | 数据库、缓存、同步、一致性、事务、时间 | [lens-state-data.md](lens-state-data.md) |
| F 故障与安全 | 出错时怎样限制伤害并恢复 | 高可用、灾备、降级、冗余、危害、恢复 | [lens-failure-safety.md](lens-failure-safety.md) |
| T 信任、安全与隐私 | 面对滥用或攻击时信任谁、暴露什么 | 身份、权限、敏感数据、外部输入、对手行为 | [lens-trust-security-privacy.md](lens-trust-security-privacy.md) |
| P 性能与容量 | 负载增长时怎样满足资源和时延预算 | 延迟、吞吐、队列、扩容、过载、资源上限 | [lens-performance-capacity.md](lens-performance-capacity.md) |
| Q 验证与质量 | 怎样证明实现符合意图 | 测试策略、验收、复现、合规证据、高后果结论 | [lens-verification-quality.md](lens-verification-quality.md) |
| C 变化与演进 | 怎样低风险地改变现有系统 | 迁移、兼容、发布、重构、技术债、弃用 | [lens-change-evolution.md](lens-change-evolution.md) |
| O 运行与反馈 | 上线后怎样发现、响应和学习 | 监控、告警、SLO、自动化、事故、漂移 | [lens-operations-feedback.md](lens-operations-feedback.md) |
| H 人与组织 | 人怎样理解、协作和承担责任 | 多团队、所有权、认知负荷、人机协作、激励 | [lens-people-organization.md](lens-people-organization.md) |
| L 全生命周期与外部性 | 从制造到退役留下什么 | 长寿命、制造维护、供应链、无障碍、环境、退役 | [lens-lifecycle-externalities.md](lens-lifecycle-externalities.md) |

## 选择规则

- 按**正在决定的对象**选择镜头，不按它可能造成的所有后果选择。
- 从零立项通常先读 V、K，再按系统特征增加镜头；已有系统的局部问题不必强制读取 V、K。
- 一个观察可以命中多个镜头，但每个镜头必须改变不同的决定。
- 未选镜头不等于“不重要”。若关键约束变化，重新扫描本索引。

## 常见组合

这些是路由起点，不是固定套餐：

- **新产品或平台立项**：V、K、E、S、A，按数据与风险补 D/F/T。
- **API 或系统集成**：I、D、F、C，按威胁和负载补 T/P。
- **性能与扩容**：P、K、O，若改变结构或一致性再补 A/D。
- **重大重构或迁移**：C、A、I、Q，涉及状态时补 D/F。
- **线上可靠性治理**：F、O、P、Q，跨团队时补 H。
- **安全与隐私审查**：T、I、D、Q，安全关键时补 F/H/L。
- **硬件或实体设施**：V、K、F、Q、L，复杂分解时补 A/I。
- **机器学习产品**：V、K、D、Q、O，敏感数据或自动决策时补 T/H。

原则发生冲突、归属不清或需要扩充目录时，读取 [principle-governance.md](principle-governance.md)；普通项目不加载该文件。
