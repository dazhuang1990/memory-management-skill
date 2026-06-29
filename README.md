# Memory Management Skill

> 🧠 零配置的项目记忆文件管理与知识库生成技能

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Agent Compatible](https://img.shields.io/badge/Agent-Compatible-brightgreen)](#compatible-agents)

## 📖 简介

**Memory Management** 是一个专为 AI Agent 设计的技能（Skill），用于自动压缩臃肿的项目记忆文件（如 `MEMORY.md`），并将详细内容归档到项目本地的 `docs/` 知识库中。

### 🎯 解决的问题

- 记忆文件越来越大，占用大量上下文 Token
- Agent 注意力被稀释，执行任务时"偷懒"或"脑补"
- 长期项目开发中，历史细节难以追溯

### ✨ 核心特性

- **零配置**：无需手动修改任何配置文件，Agent 自动接管
- **全自动**：自动探测记忆文件、创建知识库、提取细节、精简摘要
- **防偷懒**：内置强制检索协议，确保 Agent 在需要时必须读取归档文件
- **Git 友好**：生成的 `docs/` 目录可直接提交到代码仓库
- **Obsidian 兼容**：支持 `[[双链]]` 语法，可用 Obsidian 打开 `docs` 文件夹作为 Vault

## 🚀 快速开始

### 安装

将 `SKILL.md` 文件放到你的 Agent 技能目录中：

```bash
# Cursor 用户
cp SKILL.md ~/.cursor/rules/memory-management.mdc

# Cline / Roo Code 用户
cp SKILL.md ~/.clinerules/memory-management

# WorkBuddy 用户
cp SKILL.md ~/.workbuddy/skills/memory-management/SKILL.md
```

### 使用

在对话中输入以下任意触发词即可激活：

**中文触发词**：
- "记忆管理"
- "帮我压缩记忆"
- "MEMORY太大了"
- "上下文太长了"
- "记忆瘦身"
- "整理知识库"

**英文触发词**：
- "compress memory"
- "memory management"
- "memory optimization"

### 示例

```
用户：帮我压缩一下记忆文件

Agent：
1. 自动探测发现 MEMORY.md（265行）
2. 自动创建 docs/ 文件夹和 00-Index.md 索引
3. 提取细节，创建 docs/技术架构.md、docs/开发历程.md 等子文档
4. 注入双链 [[00-Index]]、#project-archive 标签
5. 精简 MEMORY.md，保留摘要
6. 汇报："✅ 记忆压缩完成：MEMORY.md 从 265行精简至 40行（减少85%）"
```

## 📋 工作流程

```
┌─────────────────────────────────────────────────────────────┐
│                      记忆压缩流程                            │
├─────────────────────────────────────────────────────────────┤
│  步骤1：探测与评估                                           │
│  - 扫描根目录寻找记忆文件                                    │
│  - 评估行数：≤150行→健康，>150行→触发压缩                    │
├─────────────────────────────────────────────────────────────┤
│  步骤2：基建（自动创建）                                     │
│  - 创建 docs/ 文件夹                                         │
│  - 创建 docs/00-Index.md 索引                                │
├─────────────────────────────────────────────────────────────┤
│  步骤3：转移（先写）                                         │
│  - 提取细节 → 写入 docs/[Module]-Details.md                  │
│  - 注入双链和标签                                            │
├─────────────────────────────────────────────────────────────┤
│  步骤4：精简（后删）                                         │
│  - 删除已转移细节，保留摘要                                  │
│  - 注入"强钩子"防止 Agent 偷懒                               │
├─────────────────────────────────────────────────────────────┤
│  步骤5：验证与汇报                                           │
│  - 确认原文件 <150行                                         │
│  - 确认 docs/ 文件已落盘                                     │
└─────────────────────────────────────────────────────────────┘
```

## 🛡️ 防偷懒机制

### 强钩子格式

禁止使用弱链接：
```
❌ 鉴权逻辑详见 [[docs/Auth-Module.md]]
```

必须使用强指令格式：
```
🛑 涉及鉴权模块开发/修改前，【必须调用 read_file 工具】读取 [[docs/Auth-Module.md]] 获取最新细节，严禁凭上下文脑补！
```

### 强制检索协议

当 Agent 处理涉及已归档模块的任务时，必须遵守：

1. **触发条件**：任务涉及已归档模块的代码实现、API参数、修Bug或具体配置
2. **强制动作**：在生成代码/回复前，必须显式调用文件读取工具读取对应的 `docs/*.md` 文件
3. **禁止脑补**：绝对不允许根据文件名猜测内容

## 📁 生成的目录结构

压缩完成后，项目目录结构如下：

```
项目根目录/
├── MEMORY.md          # 精简后的核心记忆（<150行）
├── docs/              # 自动生成的知识库
│   ├── 00-Index.md    # 双链索引
│   ├── 技术架构.md    # 归档的详细内容
│   ├── 开发历程.md
│   └── ...
└── ...
```

## 🎯 优化效果

| 项目 | 优化前 | 优化后 | 减少 |
|------|--------|--------|------|
| 记忆文件行数 | 265行 | 40行 | 85% |
| Token消耗 | ~3000 | ~500 | 83% |
| 查阅效率 | 低 | 高 | - |

## 🔧 兼容的 Agent

- [x] Cursor
- [x] Cline
- [x] Roo Code
- [x] Claude Code
- [x] WorkBuddy
- [x] 其他支持 Skill/Rules 的 Agent

## 📝 许可证

本项目采用 [MIT 许可证](LICENSE) 开源。

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📚 相关链接

- [WorkBuddy 官网](https://www.codebuddy.cn/workbuddy)
- [Agent 技能开发指南](https://docs.codebuddy.cn/workbuddy/skills)

---

**如果这个 Skill 对你有帮助，请给个 ⭐ Star 支持一下！**
