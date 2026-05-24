---
date: 2026-05-20
tags:
  - skills
  - daily-report
  - claude-code
  - openclaw
  - trae
---

# Skills 每日统计报告 - 2026-05-20

> 本报告统计本地所有技能目录，覆盖 Claude Code、OpenClaw、Trae/SOLO 三大来源

---

## 统计摘要

| 指标 | 数值 | 变化 |
|:-----|:-----|:-----|
| **Claude Code Skills** | 158 个 | 顶层目录 |
| **Trae/SOLO Skills** | 132 个 | 内置技能 |
| **OpenClaw Workspace Skills** | 406 个 | 工作区技能 |
| **总计（去重估算）** | **~700 个** | - |

---

## 来源分布

| 来源 | 数量 | 占比 |
|:-----|:-----|:-----|
| Claude Code | 158 | 22.6% |
| OpenClaw | 406 | 58.0% |
| Trae/SOLO | 132 | 18.9% |

---

## 使用频率 Top 5 技能

基于近期会话日志分析：

### 1. GitHub 集成技能
- **使用次数**: 15+
- **场景**: 连接 GitHub、搜索项目、创建仓库
- **来源**: Trae/SOLO 内置

### 2. Open Design 系列
- **使用次数**: 12+
- **场景**: 网页原型设计、UI 优化
- **来源**: Claude Code

### 3. Web 开发技能
- **使用次数**: 10+
- **场景**: 摄影网站开发、Cloudflare 部署
- **来源**: Claude Code + Trae/SOLO

### 4. 任务管理技能
- **使用次数**: 8+
- **场景**: 每日任务更新、技能统计
- **来源**: Claude Code

### 5. 系统管理技能
- **使用次数**: 6+
- **场景**: Bash 进程管理、系统清理
- **来源**: Trae/SOLO 内置

---

## 新尝试的技能

1. **agent-designer** - 多智能体系统设计
2. **agent-workflow-designer** - Agent 工作流设计器
3. **book-illustration-workflow** - 书籍插画工作流
4. **qiaomu-info-card-designer** - 乔木信息卡设计
5. **browser-automation** - 浏览器自动化

---

## 遇到的问题和改进建议

### 问题记录：

| 问题 | 影响 | 状态 |
|:-----|:-----|:-----|
| GitHub API 网络受限 | 无法直接搜索 GitHub | 已配置 Token |
| OpenClaw 项目未初始化 Git | 无法直接推送更新 | 需要初始化 |
| Bash 进程过多 | 系统资源占用高 | 已限制数量 |
| 技能目录分散 | 统计困难 | 考虑统一索引 |

### 改进建议：

1. 建立技能使用日志
2. 优化技能搜索（统一索引）
3. 定期清理未使用技能
4. 技能版本管理

---

## 待探索的新技能推荐

### 1. hv-analysis（横纵分析法）
- 深度研究产品、公司、概念或技术
- 触发词: "横纵分析"、"深度研究"

### 2. tacit-mining（隐性知识挖掘）
- 提取隐性知识，沉淀方法论
- 触发词: "挖隐性知识"、"深度访谈"

### 3. ljg-card（内容铸造器）
- 内容转化为 PNG 视觉卡片
- 触发词: "铸"、"做成图"

### 4. seedance-video（视频生成）
- 使用 Seedance 模型生成视频
- 触发词: "生成视频"、"创建视频"

### 5. rag-architect（RAG 管道设计）
- 设计检索增强生成管道
- 触发词: "设计 RAG"、"知识库"

---

## 技能目录变更记录

与上次记录对比（2026-05-10 → 2026-05-20）：

| 对比项 | 上次 | 本次 | 变化 |
|:-------|:-----|:-----|:-----|
| Claude Code 顶层技能 | 158 | 158 | 持平 |
| Trae/SOLO 内置 | 132 | 132 | 持平 |
| OpenClaw Workspace | 406 | 406 | 持平 |

---

## 待办事项

- [ ] 整理 alirezarezvani 系列嵌套技能清单
- [ ] 为 OpenClaw workspace 技能建立索引
- [ ] 设置每日自动更新任务
- [ ] 建立技能使用频率统计机制
- [ ] 创建技能推荐算法
- [ ] 同步技能清单到 GitHub 仓库

---

*报告生成时间: 2026-05-20*  
*数据来源: ~/.claude/skills/, ~/.trae-cn/skills/, ~/.openclaw/workspace/*
