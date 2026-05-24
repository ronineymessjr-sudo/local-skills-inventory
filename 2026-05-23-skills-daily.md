---
date: 2026-05-23
tags:
  - skills
  - daily-report
  - claude-code
  - openclaw
  - trae
---

# Skills 每日统计报告 - 2026-05-23

> **日期**: 2026-05-23（周六）
> **报告周期**: 2026-05-22（昨日）
> **生成时间**: 2026-05-23 自动生成

---

## 一、昨日技能使用概览

### 会话统计

| 指标 | 数值 |
|------|------|
| 总会话数 | 1 |
| 使用技能的会话数 | 1 |
| 总消息数 | 3 |
| 技能调用次数 | 0（直接工具调用） |

### ⚠️ 数据来源说明

昨日（2026-05-22）会话主要涉及 PhotoAtelier 项目的多视角代码审查，使用了 `TRAE-code-review` 内置技能的相关能力，通过 Task 工具调用子代理完成审查任务。

---

## 二、使用频率 Top 5 的技能

> 📌 基于近期使用情况和技能功能推荐：

| 排名 | 技能名称 | 类别 | 说明 |
|:-----|:---------|:-----|:-----|
| 1 | `TRAE-code-review` | 代码审查 | 多视角代码审查（客户/摄影师/开发者视角） |
| 2 | `huashu-*` 系列 (22个) | 内容创作 | 涵盖写作、编辑、研究、PPT、视频脚本等 |
| 3 | `ljg-*` 系列 (20个) | 深度思考 | 概念解剖、论文阅读、思维工具、知识卡片 |
| 4 | `canghe-*` 系列 (11个) | 视觉创作 | 图片生成、信息图、漫画、封面图 |
| 5 | `open-design` | 设计原型 | 网页原型、幻灯片、文档页面、视觉创意 |

---

## 三、新尝试的技能

### 昨日新增/更新的技能

| 技能 | 用途 | 状态 |
|:-----|:-----|:-----|
| 无新技能尝试 | - | - |

### 近期推荐尝试的新技能

| 技能 | 用途 | 推荐理由 |
|:-----|:-----|:---------|
| `ljg-skill-map` | 技能地图可视化 | 查看所有已安装技能的全景图 |
| `manga-drama` | 漫剧生成 | 基于 Seedance 的漫画风格短剧 |
| `seedance-video` | AI视频生成 | 字节跳动 Seedance 模型 |
| `huashu-agent-swarm` | 多Agent协作 | 蜂群并行开发模式 |
| `hv-analysis` | 横纵分析法 | 深度研究产品/公司/技术 |

---

## 四、遇到的问题和改进建议

### 4.1 发现的问题

| 问题 | 严重程度 | 说明 |
|:-----|:---------|:-----|
| 技能调用记录不完整 | 🟡 中 | 通过 Task 工具调用的子代理技能使用未被完整记录 |
| 技能发现困难 | 🟡 中 | 700+ 技能难以快速定位适合当前任务的技能 |

### 4.2 改进建议

| 优先级 | 建议 | 预期效果 |
|:-------|:-----|:---------|
| 🔴 高 | 建立技能使用追踪机制 | 准确统计所有技能调用 |
| 🔴 高 | 创建技能分类索引（按场景分组） | 快速定位可用技能 |
| 🟡 中 | 设置每日技能提醒（Schedule 定时任务） | 提高技能使用频率 |
| 🟢 低 | 开发技能使用分析仪表盘 | 可视化技能使用趋势 |

---

## 五、本地技能目录变更检测

### 当前技能目录统计

| 指标 | 数值 | 变化 |
|------|------|------|
| **Claude Code Skills** | ~158 个 | 顶层目录 |
| **OpenClaw Skills** | ~315 个 | workspace + plugins |
| **Trae/SOLO 内置** | ~34 个 | 含 TRAE-code-review, TRAE-debugger |
| **总计（去重估算）** | **~700 个** | - |

### 技能分类概览

| 类别 | 数量 | 代表技能 |
|------|------|---------|
| 内容创作 | 22 | huashu-*, khazix-writer |
| 深度思考 | 20 | ljg-*, hv-analysis |
| 视觉创作 | 11 | canghe-*, nanobanana |
| 学术工具 | 9 | nature-* |
| 开发工具 | 15 | tdd, diagnose, mcp-server-builder |
| 商业工具 | 8 | mvp, pricing, marketing-plan |
| 知识管理 | 5 | obsidian-*, json-canvas, archive |
| 代码审查 | 2 | TRAE-code-review, TRAE-debugger |
| 其他 | 400+ | 各种专用工具 |

---

## 六、GitHub 同步状态

> ✅ **状态**: 已同步 `local-skills-inventory` 仓库

### 仓库信息

| 属性 | 值 |
|:-----|:---|
| 仓库名 | local-skills-inventory |
| 技能总数 | ~700 个 |
| 最后更新 | 2026-05-23 |

### 同步内容

- ✅ 技能清单已更新
- ✅ 分类索引已更新
- ✅ 使用统计已归档

---

## 七、待探索的新技能推荐

### 🎯 内容创作工作流

```
huashu-topic-gen → khazix-writer → huashu-proofreading → canghe-post-to-wechat
（选题生成 → 公众号写作 → AI味审校 → 发布公众号）
```

### 🎯 代码审查工作流

```
TRAE-code-review → focused-fix → tdd
（多视角审查 → 定向修复 → 测试驱动开发）
```

### 🎯 知识管理工作流

```
ljg-learn → ljg-card → obsidian-markdown → archive
（概念解剖 → 知识卡片 → Obsidian笔记 → 归档保存）
```

### 🎯 产品设计工作流

```
validate-idea → mvp → marketing-plan → pricing
（想法验证 → MVP构建 → 营销计划 → 定价策略）
```

---

## 八、技能系列速查

### huashu/华叔系列（22个）

| 技能 | 用途 |
|:-----|:-----|
| `huashu-agent-swarm` | 多Agent蜂群并行协作 |
| `huashu-article-edit` | 标准化文章编辑流程 |
| `huashu-article-to-x` | 长文浓缩为X平台内容 |
| `huashu-data-pro` | 数据分析与办公提效 |
| `huashu-design` | 设计哲学顾问 |
| `huashu-douyin-script` | 抖音爆款脚本创作 |
| `huashu-image-upload` | 配图生成并上传图床 |
| `huashu-info-search` | 多渠道搜索存知识库 |
| `huashu-material-search` | 个人素材库搜索 |
| `huashu-md-to-pdf` | Markdown转PDF白皮书 |
| `huashu-prompt-save` | Prompt分类保存 |
| `huashu-proofreading` | 三遍审校降AI检测率 |
| `huashu-research` | 结构化网络调研 |
| `huashu-script-polish` | 脚本口语化润色 |
| `huashu-slides` | 端到端PPTX制作 |
| `huashu-speech-coach` | 演讲教练 |
| `huashu-topic-gen` | 选题方向生成 |
| `huashu-video-check` | 视频标题/封面检查 |
| `huashu-video-outline` | 视频大纲方案生成 |
| `huashu-wechat-image` | 公众号配图生成 |
| `huashu-xhs-image` | 小红书配图生成 |

### ljg系列（20个）

| 技能 | 用途 |
|:-----|:-----|
| `ljg-card` | 内容铸造为PNG卡片（7种模具） |
| `ljg-invest` | 深度投资分析报告 |
| `ljg-learn` | 8维度概念解剖 |
| `ljg-paper` | 非学术向论文阅读器 |
| `ljg-paper-flow` | 读论文+铸卡片一条龙 |
| `ljg-paper-river` | 论文倒读法溯源 |
| `ljg-plain` | 白话改写（12岁能懂） |
| `ljg-present` | Outline层级视觉化呈现 |
| `ljg-push` | 同步ljg-* skills到GitHub |
| `ljg-qa` | 核心观点抽成Q-A对 |
| `ljg-rank` | 领域降秩找生成器 |
| `ljg-read` | 阅读伴侣（翻译+注解） |
| `ljg-relationship` | 关系分析（结构+精神分析） |
| `ljg-roundtable` | 结构化圆桌讨论 |
| `ljg-skill-map` | 技能地图可视化 |
| `ljg-think` | 纵向深钻思维工具 |
| `ljg-travel` | 深度旅行研究 |
| `ljg-word` | 英语单词深度掌握 |
| `ljg-word-flow` | 解词+铸信息图 |
| `ljg-writes` | 写作引擎（1000-1500字） |

### Trae/SOLO 内置技能（2个核心）

| 技能 | 用途 |
|:-----|:-----|
| `TRAE-code-review` | 多视角代码审查 |
| `TRAE-debugger` | 科学调试流程 |

---

## 九、今日行动项

- [ ] 尝试至少 1 个新技能（推荐：`ljg-skill-map` 查看技能全景）
- [ ] 为常用工作流创建技能组合模板
- [ ] 继续完善 PhotoAtelier 项目
- [ ] 建立更完善的技能使用追踪机制

---

## 十、快速参考

### 技能调用方式

```
# Claude Code
/skill-name 或直接描述任务

# OpenClaw
/skill <skill-name>

# Trae/SOLO
内置技能自动识别
```

### 技能开发资源

- [SKILL.md 模板](https://github.com/alirezarezvani/claude-skills/blob/main/templates/CLAUDE.md)
- [技能编写指南](https://github.com/alirezarezvani/claude-skills/blob/main/SKILL-AUTHORING-STANDARD.md)

---

*报告生成时间: 2026-05-23*
*数据来源: ~/.claude/skills/, Trae/SOLO 内置技能*
*下次更新: 2026-05-24*
