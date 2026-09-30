# 独立镜头评审协议

本协议用于保证“第三方视角”来自真正不同的智能体，而不是同一智能体在一次推理中模拟多个角色。主智能体负责协调；每个选中镜头由一个不同、非实现作者的 reviewer 独立检查代码。

## 角色边界

### 评审协调者

- 确认触发门、评审对象、阶段、边界和成功判据；
- 通过 [lens-index.md](lens-index.md) 选择镜头；
- 识别参与当前候选实现的 agent/task ID；无法核实时保留 unknown，不自行推定独立；
- 为每个镜头派发不同 reviewer，并维护镜头—reviewer 映射；
- 等待所有首次报告后再去重、交叉引用和综合；
- 不亲自承担镜头检查，不把自己的实现判断包装成 reviewer 发现。

### 镜头 reviewer

- 只承担一个镜头，并完整读取该镜头文件；
- 独立阅读评审范围内的代码、测试、配置、契约和证据；
- 寻找会推翻当前设计或发布结论的反例、缺口和失败路径；
- 默认只读，不修改代码、测试、配置或评审记录；
- 首次报告前不读取其他 reviewer 的发现。

## 派发包

每个 reviewer 接收同一组中立事实，加上自己负责的镜头：

- 用户明确的评审阶段、目标与成功判据；
- 本次范围、非目标和不可改变的约束；
- 代码库位置、需要检查的路径和可运行的只读验证入口；
- 发布评审时的旧基线、完整变更清单和候选 revision；
- reviewer 可使用的权限、时间或环境限制；
- 分配的镜头文件与要求的报告格式。

首次派发不得包含实现者对正确性的结论、建议修法、预期缺陷或其他 reviewer 的发现。必须提供的既有架构决定或用户约束应作为来源可核验的事实给出，而不是作为“应该得出什么结论”的提示。

环境支持干净会话或无历史派发时必须使用它。若 reviewer 无法避免继承实现者结论、建议修法或预期缺陷，独立性状态为 `independence-unmet`，不能计入第三方评审。只有用户明确接受非独立降级后才能继续普通自查，且其结果不得写成独立评审基线。“不同智能体”不自动等于“完全无锚定”。

## 一镜头一智能体

- 每个选中镜头有且只有一个首要 reviewer；同一 reviewer 不得承担多个镜头。
- 映射使用平台提供的稳定 agent/task ID，不使用可任意伪造的角色名作为身份。所有 reviewer ID 必须全局唯一，且不得出现在当前候选的实现作者集合中。
- 映射必须记录 `author relation: non-author | author | unknown`；只有 `non-author` 满足独立性。身份或作者关系无法核实时使用 `unknown`，并判定 `independence-unmet`。
- 多个 reviewer 可以检查同一段代码，因为他们回答的是不同决策问题。
- 并发槽位不足时分批调度，不合并镜头来节省 reviewer。
- reviewer 失败或失联时，可用新的独立 reviewer 重试；不能由协调者代写结果。
- 完整评审选中全部 15 个镜头，因此需要 15 个 reviewer；可以分批，但必须等全部完成后再给整体结论。

## Reviewer 输出

每个 reviewer 返回：

```markdown
## Lens review
- Lens:
- Reviewer agent/task ID:
- Author relation: non-author | author | unknown
- Context mode: clean | inherited
- Independence limits:
- Status: complete | blocked | failed
- Scope inspected:
- Evidence commands or artifacts:

### Findings
1. Severity; principle ID; exact file/function/line or artifact; observed fact; impact; falsification or required evidence; confidence.

### No-finding areas
- Checked area; why no decision-changing issue was found.

### Unknowns
- Missing evidence or inaccessible scope and its consequence.

### Lens verdict
- pass | conditional | fail | not-assessed
```

`complete` 要求 reviewer 已访问本镜头的决策关键范围，检查了相关实现与证据，并给出可定位、可复核的报告。决策关键代码、契约、测试、环境或证据不可访问时必须标为 `blocked`；执行失败标为 `failed`。`blocked` 或 `failed` 的 verdict 必须是 `not-assessed`，不能用 `pass` 或 `conditional` 掩盖未知。

没有发现问题时也必须说明实际检查过什么和使用了什么证据；一句“无问题”不构成评审。

## 综合规则

所有 reviewer 首次报告完成后，协调者：

1. 按规范归属合并同一根因的重复发现，同时保留其他镜头描述的不同影响；
2. 对冲突结论并列证据、假设和适用边界，不用多数票决定正确性；
3. 将无法验证的主张标为未知，不替 reviewer 补造证据；
4. 综合前校验 reviewer ID 全局唯一、均为 `non-author`、上下文为 `clean`，并列出每个选中镜头的状态和 verdict；任一条件不满足时独立评审为 `incomplete`；
5. 只有全部选中镜头状态均为 `complete` 时，才可形成整体 verdict。阻塞性发现被用户明确接受时只能是 `conditional`；违反用户成功判据、硬约束或不可豁免底线时仍为 `fail`；
6. 修复发生后，只重新派发受影响镜头，但仍应使用非作者 reviewer；优先让原 reviewer 复查，无法使用时记录 reviewer 变更。

## 无法满足独立性时

若环境没有多智能体能力、无法获得足够不同 reviewer，或 reviewer 实际就是实现作者：

- 停止形成第三方评审结论；
- 明确列出未完成的镜头和能力限制；
- 询问用户是延期，还是明确接受由协调者执行的非独立降级检查；
- 未经用户确认，不得静默降级；即使用户接受降级，也只能标为非独立检查，不能写入有效的独立评审基线。
