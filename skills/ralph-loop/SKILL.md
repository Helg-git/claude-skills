---
description: "Start Ralph Loop in current session - continuous self-referential AI loops for iterative development"
---

# Ralph Loop - 迭代式开发循环

你正在使用 Ralph Loop，这是一个实现 Ralph Wiggum 技术的插件，通过自我反馈循环来迭代完成开发任务。

## 工作原理

Ralph Loop 会在你尝试退出时阻止退出，并将相同的 prompt 再次返回给你，让你看到之前的工作（在文件和 git 历史中），从而不断改进。

## 执行步骤

1. **运行设置脚本**：

```bash
/Users/heliguang/.claude/plugins/cache/claude-plugins-official/ralph-loop/manual-install/scripts/setup-ralph-loop.sh $ARGUMENTS
```

2. **开始工作**：按照任务描述开始工作

3. **尝试退出**：当你完成任务或想进入下一轮迭代时尝试退出

4. **循环继续**：Stop hook 会阻止退出并返回相同的 prompt

5. **完成条件**：
   - 达到 `--max-iterations` 限制
   - 输出 `<promise>YOUR_PROMISE</promise>` 完成承诺

## 使用方法

```
/ralph-loop "Build a REST API for todos" --max-iterations 20 --completion-promise "DONE"
```

**参数说明**：
- `prompt`（必填）：任务描述
- `--max-iterations N`：最大迭代次数（默认：无限）
- `--completion-promise "TEXT"`：完成信号短语

## 重要规则

**CRITICAL**: 如果设置了 completion promise，你只能在该语句**完全且毫无疑问地为真**时才输出它。不要输出虚假承诺来逃避循环，即使你认为卡住了或有其他原因应该退出。

## 最佳实践

1. **明确的完成标准**：
   - ✅ 好："Build a todo API. When complete: All CRUD working, tests passing. Output <promise>DONE</promise>"
   - ❌ 坏："Build a todo API and make it good"

2. **使用安全网**：
   - 总是设置 `--max-iterations` 防止无限循环

3. **迭代改进**：
   - 让循环自动改进，不要第一次就追求完美

## 监控进度

```bash
# 查看当前迭代
grep '^iteration:' .claude/ralph-loop.local.md

# 查看完整状态
head -10 .claude/ralph-loop.local.md
```

## 取消循环

使用 `/cancel-ralph` 或手动删除状态文件：
```bash
rm -f .claude/ralph-loop.local.md
```

## 哲学原则

1. **迭代 > 完美**：让循环完善工作
2. **失败是数据**：失败是可预测且信息丰富的
3. **坚持获胜**：持续尝试直到成功

## 适用场景

**适合**：
- 有明确成功标准的任务
- 需要迭代完善的任务（如让测试通过）
- 有自动验证的任务（测试、linter）

**不适合**：
- 需要人类判断的任务
- 一次性操作
- 成功标准不明确的任务

---

现在，首先执行设置脚本来激活 Ralph Loop！
