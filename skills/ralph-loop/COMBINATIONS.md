# Ralph Loop 协同组合方案

## 1. Task Breakdown + Ralph Loop = 项目自动完成

**工作流**：
1. `/task-breakdown` 将大任务分解为小任务
2. 对每个子任务运行 `ralph-loop` 迭代完成
3. 自动检查完成条件后进入下一个子任务

**Prompt 模板**：
```
首先用 task-breakdown 将项目分解为 3-5 个子任务。
然后对第一个子任务运行 ralph-loop：
ralph-loop "实现用户认证模块：JWT、登录注册、中间件、测试。完成后输出 <promise>SUBTASK_1_DONE</promise>" --completion-promise "SUBTASK_1_DONE" --max-iterations 20
完成后继续下一个子任务。
```

---

## 2. Code Review + Ralph Loop = 质量螺旋上升

**工作流**：
1. Ralph Loop 编写代码和测试
2. 每次迭代后自动调用 `/code-review` 审查
3. 根据审查反馈改进代码
4. 循环直到代码质量达标

**Prompt 模板**：
```
ralph-loop "实现一个 RESTful API。
每次编写代码后：
1. 运行测试确保通过
2. 使用 /code-review 审查代码
3. 根据审查意见修复问题
4. 重复直到测试通过且代码审查无严重问题
完成后输出 <promise>CODE_COMPLETE</promise>" --completion-promise "CODE_COMPLETE" --max-iterations 25
```

---

## 3. Error Diagnose + Ralph Loop = 自动故障修复

**工作流**：
1. 遇到错误时启动 Ralph Loop
2. 循环：诊断 → 修复 → 测试 → 诊断
3. 直到错误完全解决

**Prompt 模板**：
```
ralph-loop "当前项目有测试失败。
迭代流程：
1. 使用 /error-diagnose 分析错误
2. 根据诊断修复问题
3. 运行测试验证
4. 如果仍有错误，回到步骤 1
完成后输出 <promise>ALL_TESTS_PASSING</promise>" --completion-promise "ALL_TESTS_PASSING" --max-iterations 15
```

---

## 4. Gas Optimizer + Ralph Loop = 智能合约优化循环

**工作流**：
1. Ralph Loop 实现基础功能
2. Gas Optimizer 分析 gas 消耗
3. 根据建议优化代码
4. 循环直到 gas 最优

**Prompt 模板**：
```
ralph-loop "实现一个 ERC20 代币合约。
迭代流程：
1. 编写合约和测试
2. 确保所有测试通过
3. 使用 /gas-optimizer 分析 gas 使用
4. 根据建议优化代码
5. 重新测试确保功能正常
6. 重复直到 gas 使用优化到最低
完成后输出 <promise>GAS_OPTIMIZED</promise>" --completion-promise "GAS_OPTIMIZED" --max-iterations 20
```

---

## 5. Web3 Security + Ralph Loop = 安全开发循环

**工作流**：
1. 编写智能合约代码
2. Web3 Security 审查漏洞
3. 修复安全问题
4. 循环直到无安全漏洞

**Prompt 模板**：
```
ralph-loop "开发一个去中心化交易所合约。
每次迭代：
1. 实现功能模块
2. 编写测试用例
3. 使用 /web3-security 进行安全审计
4. 修复发现的漏洞
5. 重新测试
完成后输出 <promise>SECURE_AND_TESTED</promise>" --completion-promise "SECURE_AND_TESTED" --max-iterations 30
```

---

## 6. Project Scaffold + Ralph Loop = 完整项目生成

**工作流**：
1. Project Scaffold 生成项目骨架
2. Ralph Loop 填充业务逻辑
3. Code Review 确保质量
4. Deploy Wizard 部署

**Prompt 模板**：
```
首先运行 /project-scaffold 创建一个 Next.js + TypeScript 项目骨架。

然后运行 ralph-loop "填充项目业务逻辑：
- 实现用户认证页面
- 实现数据展示组件
- 添加 API 路由
- 编写单元测试
- 确保构建无错误

每完成一个模块后使用 /code-review 审查。
完成后输出 <promise>PROJECT_COMPLETE</promise>" --completion-promise "PROJECT_COMPLETE" --max-iterations 40
```

---

## 7. Solana Dev + Ralph Loop = Solana 程序迭代开发

**工作流**：
1. Solana Dev 指导框架搭建
2. Ralph Loop 迭代实现功能
3. 测试验证
4. 优化改进

**Prompt 模板**：
```
ralph-loop "使用 /solana-dev 最佳实践开发一个 Solana 程序：
- 创建程序框架
- 实现核心指令
- 添加测试用例
- 部署到本地网络验证
- 优化性能

每步参考 /solana-dev 的建议。
完成后输出 <promise>SOLANA_PROGRAM_READY</promise>" --completion-promise "SOLANA_PROGRAM_READY" --max-iterations 25
```

---

## 8. Solidity Dev + Token Factory + Ralph Loop = DApp 完整开发

**工作流**：
1. Token Factory 快速生成代币合约
2. Solidity Dev 指导最佳实践
3. Ralph Loop 完善功能
4. Web3 Security 确保安全

**Prompt 模板**：
```
1. 使用 /token-factory 生成 ERC20 代币合约骨架

2. 运行 ralph-loop "完善代币 DApp：
   - 添加铸造功能
   - 实现质押奖励
   - 前端集成
   - 按照 /solidity-dev 最佳实践
   - 通过 /web3-security 审查
   完成后输出 <promise>DAPP_COMPLETE</promise>" --completion-promise "DAPP_COMPLETE" --max-iterations 35
```

---

## 超级组合：全流程自动化

**终极工作流**：
```
我要创建一个 DeFi 项目。请按以下流程执行：

1️⃣ /task-breakdown - 分解为 5-8 个子任务
2️⃣ 对每个子任务依次执行：
   - ralph-loop 迭代实现
   - 每次迭代后 /code-review 审查
   - 智能合约部分用 /web3-security 审查
   - 代币合约用 /gas-optimizer 优化
3️⃣ 所有子任务完成后：
   - /project-scaffold 生成部署配置
   - /deploy-wizard 部署到测试网

目标：创建一个完整的、安全的、优化的 DeFi 协议
```

---

## 关键原则

1. **任务前置分解**：先用 task-breakdown，再用 ralph-loop 逐个击破
2. **质量门控**：每次迭代后用 code-review 确保质量不下降
3. **专项优化**：根据领域用 gas-optimizer/web3-security 等专项 skills
4. **自动化测试**：每个循环都要有测试验证
5. **明确完成标准**：设置清晰的 completion-promise

## 效果倍增示例

```
❌ 单独使用 ralph-loop:
"写一个智能合约" → 可能写出不安全、未优化的代码

✅ 组合使用:
"用 task-breakdown 分解 → ralph-loop 实现 → 每次迭代 code-review → web3-security 审查 → gas-optimizer 优化"
→ 得到安全、高质量、最优化的代码
```
