本文档总结了 [Augment Code 官方博客](https://link.zhihu.com/?target=https%3A//www.augmentcode.com/blog/best-practices-for-using-ai-coding-agents) 中关于使用 Augment Agent 的最佳实践。

## 目录

-   [简介](https://zhuanlan.zhihu.com/write#%E7%AE%80%E4%BB%8B)
-   [Augment Agent 适合做什么？](https://zhuanlan.zhihu.com/write#augment-agent-%E9%80%82%E5%90%88%E5%81%9A%E4%BB%80%E4%B9%88)
-   [如何向 Agent 分配任务？](https://zhuanlan.zhihu.com/write#%E5%A6%82%E4%BD%95%E5%90%91-agent-%E5%88%86%E9%85%8D%E4%BB%BB%E5%8A%A1)
-   [当 Agent 没有按照预期工作时怎么办？](https://zhuanlan.zhihu.com/write#%E5%BD%93-agent-%E6%B2%A1%E6%9C%89%E6%8C%89%E7%85%A7%E9%A2%84%E6%9C%9F%E5%B7%A5%E4%BD%9C%E6%97%B6%E6%80%8E%E4%B9%88%E5%8A%9E)
-   [如果不信任 Agent 怎么办？](https://zhuanlan.zhihu.com/write#%E5%A6%82%E6%9E%9C%E4%B8%8D%E4%BF%A1%E4%BB%BB-agent-%E6%80%8E%E4%B9%88%E5%8A%9E)
-   [Agent 运行时应该做什么？](https://zhuanlan.zhihu.com/write#agent-%E8%BF%90%E8%A1%8C%E6%97%B6%E5%BA%94%E8%AF%A5%E5%81%9A%E4%BB%80%E4%B9%88)
-   [如何审查 Agent 编写的代码？](https://zhuanlan.zhihu.com/write#%E5%A6%82%E4%BD%95%E5%AE%A1%E6%9F%A5-agent-%E7%BC%96%E5%86%99%E7%9A%84%E4%BB%A3%E7%A0%81)

## 简介

AI 编码代理（如 Augment Agent）的工作方式与人类相似。最有效的使用方法是**将其视为与一位聪明但经验较少的工程师合作**。近期 AI 的进步与人类工作方式有许多相似之处：

-   **更多思考时间，更好的结果** – 与人类一样，AI 在有时间思考时表现更好
-   **使用工具** – AI 在拥有更好的工具时能力会增强
-   **从反馈中学习** – AI 通过反馈循环改进，类似于人类的学习方式
-   **受奖励激励** – 与人类类似，AI 也可以通过激励机制提高表现

## Augment Agent 适合做什么？

Augment Agent 在以下方面表现出色：

-   修复可重现和"可测试"的 bug
-   实现具有明确规格（来自任务单或设计文档）或定义清晰的需求的功能
-   头脑风暴和原型设计
-   深入探索复杂的代码库
-   处理 PR 审查评论

随着开发人员发现新的应用场景，这个列表还在不断扩展。

## 如何向 Agent 分配任务？

### 1\. 使用详细的提示

❌ **不好的例子**："将 `[SettingsWebviewPanel](https://zhida.zhihu.com/search?content_id=256109949&content_type=Article&match_order=1&q=SettingsWebviewPanel&zhida_source=entity)` 类改为使用事件"

✅ **好的例子**：

```text
目前，我们通过导入 `SettingsWebviewPanel` 类直接触发 `SettingsWebviewPanel.statusUpdate()`。 审核人员建议切换到 VSCode 事件来触发 `statusUpdate()`， 以减少组件之间的紧密耦合。 让我们通过以下步骤实现： 1. 创建一个新的 VSCode 事件 2. 在 `SettingsWebviewPanel` 构造函数中添加事件处理程序 3. 触发这个事件而不是直接调用 `statusUpdate()`
```

### 2\. 提供全面的背景信息

-   解释不仅是目标，还有背后的原因和约束条件
-   分享相关文档（任务单、GitHub issues、PR）
-   包含有用的参考示例
-   指定相关关键词和文件位置

### 3\. 将复杂任务分解为更小的部分

❌ **不好的例子**："阅读任务单 BC-986，实现设置菜单，编写测试并更新文档"

✅ **好的例子**：先要求阅读任务单 BC-986，然后实现设置菜单，再编写测试并更新文档

### 4\. 对于复杂任务，先与 Agent 讨论和完善计划

❌ **不好的例子**："在设置菜单中公开时区选择"

✅ **好的例子**："我需要在设置菜单中公开时区选择。首先提出一个计划，在我批准之前不要跳到实现阶段"

### 5\. 利用 Agent 对测试结果进行迭代的能力

❌ **不好的例子**：手动运行测试并将输出复制粘贴给 Agent

✅ **好的例子**："为 '[TextGenerator](https://zhida.zhihu.com/search?content_id=256109949&content_type=Article&match_order=1&q=TextGenerator&zhida_source=entity)' 类实现测试，并运行它们以确保它们正常工作"

### 6\. 尝试在不熟悉的任务上使用 Agent

✅ **好的例子**："我需要实现这个新的过滤算法。请探索代码并提出一些可行的实现方案"

### 7\. 提供积极反馈

✅ **好的例子**："很好，差不多对了！现在让我们处理这个边缘情况..."

## 当 Agent 没有按照预期工作时怎么办？

如果 Agent 偏离了轨道，您有两个选择：

1.  开始一个新的 Agent 会话（如果完全偏离）
2.  在同一会话中引导它回到正确的方向（如果只是稍微偏离）

其他提示：

-   当 Agent 进行错误的文件编辑时，使用 Augment Agent 的检查点系统
-   为不同的逻辑任务创建新的 Agent 会话
-   将复杂任务分解为可管理的部分
-   考虑使用英语以获得更好的结果（因为英语在 LLM 训练数据中占主导地位）
-   如果 Agent 在框架语法方面遇到困难，引导它查阅官方文档

## 如果不信任 Agent 怎么办？

-   开始时使用非自动模式
-   如果需要，依靠检查点系统来撤销更改
-   首先使用"问答"模式来验证 Agent 对代码库的理解
-   尝试简单的、自包含的任务
-   随着看到积极的结果，您的信心会自然增长

## Agent 运行时应该做什么？

用户与 Agent 的互动通常会经历三个技能水平：

1.  **初学者**：密切关注 Agent，经常中断以纠正
2.  **中级**：在偶尔检查进度的同时进行多任务处理
3.  **高级**：为不同的任务设置多个 Agent 会话，定期检查

## 如何审查 Agent 编写的代码？

-   在每个子任务后审查更改，避免积累审查债务
-   提出澄清性问题（以"只是一个问题："为前缀，以防止误解）
-   留下注释，如 `#TODO(agent): 在这里使用实用函数` 以进行多项改进
-   要求 Agent 运行 `git diff` 以验证完整性
-   利用 Agent 通过测试迭代代码的能力
