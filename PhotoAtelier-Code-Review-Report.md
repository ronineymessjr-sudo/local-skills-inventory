# PhotoAtelier 代码审查报告

**项目**: ronineymessjr-sudo/photo-workflow
**审查日期**: 2026-05-23
**审查范围**: 安全性 / 代码质量 / 性能 / 最佳实践

---

## 1. 项目概述

PhotoAtelier 是一个为摄影师打造的 AI 拍摄方案生成平台。用户选择风格和模特特征后，AI 自动生成完整拍摄方案并支持一键生成 9 张参考图。

### 1.1 技术栈

| 组件 | 技术 |
|------|------|
| 前端 | 纯 HTML/CSS/JS（单文件 index.html，8327 行 / 451KB） |
| 后端 | Cloudflare Workers (api/index.js, 472 行) |
| 数据库 | Supabase |
| 部署 | Cloudflare Pages + Workers |
| 外部服务 | MiniMax 图片生成、飞书日历/多维表格 |

### 1.2 审查结果总览

| 级别 | 数量 | 关键问题 |
|------|------|----------|
| **严重** | 5 | 密码明文存储、Token 可伪造、凭证硬编码、.gitignore 缺失、node_modules 提交 |
| **高** | 8 | XSS 漏洞、IDOR 漏洞、CORS 宽松、无速率限制、全局变量污染、无代码分割、a11y 不足 |
| **中** | 12 | 输入验证不足、错误处理不一致、无日志记录、无缓存策略、路由组织混乱、辅助脚本散落 |
| **低** | 6 | 安全头缺失、响应格式不一致、文档过时、语义化不足 |

---

## 2. 安全性审查

安全性是本次审查中发现问题最多、最严重的维度。项目存在多个严重级别的安全漏洞，**不建议在生产环境中以当前状态部署**。

### 2.1 [严重] 密码明文存储和比对

**位置**: api/index.js 第 550-563 行

**问题**: 字段名为 `password_hash`，但实际存储的是明文密码。登录时直接比对明文字符串，没有使用任何哈希算法。如果数据库被攻破，所有用户密码直接暴露。

**修复建议**:
- 使用 Web Crypto API 的 PBKDF2 对密码进行加盐哈希后再存储
- 注册时: `password_hash = await hashPassword(password)`
- 登录时: `await verifyPassword(password, users[0].password_hash)`

### 2.2 [严重] Token 使用 Base64 编码，可被伪造

**位置**: api/index.js 第 536-547 行

**问题**: `genToken()` 使用 `btoa()` 编码生成 Token，这只是 Base64 编码而非加密。任何人都可以 `atob()` 解码并伪造任意 `uid` 的 Token，冒充任何用户。

**修复建议**: 使用 JWT + HMAC-SHA256 签名，签名密钥存储在环境变量中。

### 2.3 [严重] 硬编码全部凭证

**位置**: api/index.js 第 4-17 行

**问题**: 源代码中直接硬编码了 Supabase URL/Key、飞书 App ID/Secret、多维表格 app_token/table_id 等全部凭证。所有凭证随 Git 提交永久保存在历史记录中。

**修复建议**:
1. 将所有密钥迁移到 Cloudflare Workers 环境变量
2. **立即轮换**所有已泄露的密钥
3. 使用 BFG Repo-Cleaner 清除 Git 历史中的密钥

### 2.4 [严重] .gitignore 缺失

**问题**: 项目没有 `.gitignore` 文件。`node_modules/` 和 `.wrangler/cache/` 被提交到仓库。`.wrangler/cache/wrangler-account.json` 泄露了 Cloudflare 账户信息。

**修复建议**: 立即创建 `.gitignore`，从 Git 中移除 `node_modules/` 和 `.wrangler/`。

### 2.5 [高] XSS 漏洞

**位置**: index.html 多处

**问题**: 虽然定义了 `escHtml()` 函数，但多处 innerHTML 赋值未使用它：
- 第 6900 行: AI 生成的 tips 直接插入 HTML（**高风险**）
- 第 4572、4600 行: 场地/模特列表渲染未消毒
- `escHtml` 缺少单引号转义

**修复建议**: 对所有 innerHTML 赋值统一使用消毒函数，补充单引号转义。

### 2.6 [高] IDOR 漏洞（缺少所有权验证）

**位置**: api/index.js `handleDeleteSchedule`、`handleDeleteMessage`、`handleUpdateMessage`

**问题**: 删除/更新操作虽然传入 `uid` 参数，但查询中完全没有使用 `uid` 进行过滤。任何已认证用户可以删除/修改任意用户的数据。

**修复建议**: 在所有涉及 `id` 的操作中加上 `user_id=eq.${uid}` 条件。

### 2.7 [高] CORS 配置过于宽松

**位置**: api/index.js 第 479-483 行

**问题**: `Access-Control-Allow-Origin: *` 允许任何域名调用 API，配合 Token 可伪造，任何恶意网站都可以通过 CSRF 攻击调用所有 API。

**修复建议**: 将 `*` 替换为具体的前端域名白名单。

### 2.8 [高] 无速率限制

**问题**:
- 登录端点无速率限制，可被暴力破解密码
- 公开消息端点可被批量提交垃圾消息
- 图片生成端点调用付费 API，可被滥用导致高额费用

**修复建议**: 使用 Cloudflare KV 实现基于 IP/uid 的滑动窗口限流。

### 2.9 [中] 输入验证严重不足

**位置**: 多个 Handler 函数

**问题**: `handleLogin` 仅检查 email/password 非空；`handleCreateSchedule`、`handleCreatePlan`、`handleCreateMessage` 等未验证 body 中的任何字段。

### 2.10 [中] 错误信息泄露内部实现

**位置**: api/index.js 第 927-931 行

**问题**: 全局 catch 直接返回 `e.message`，可能包含 Supabase 连接字符串、飞书 API 端点等敏感信息。

---

## 3. 代码质量审查

### 3.1 [严重] 单文件体量过大

**位置**: index.html（8327 行 / 451KB）

**问题**: 所有前端代码（HTML + CSS + JS）都在一个文件中，包含认证、方案生成、日程管理、图片生成、看板等多个模块。远超合理单文件规模，严重影响可维护性、调试效率和团队协作。

**修复建议**: 将单文件拆分为模块化结构（CSS、HTML 模板、JS 模块分离），使用构建工具（Vite/Webpack）。

### 3.2 [高] 全局命名空间严重污染

**问题**: 约 80+ 个全局函数和 15+ 个全局变量直接定义在全局作用域。仅少量代码使用了 IIFE 隔离。

**修复建议**: 使用 ES Module 或 IIFE 封装各模块，避免全局污染。

### 3.3 [中] 路由组织为巨型 if-else 链

**位置**: api/index.js 第 809-934 行

**问题**: 约 125 行的 if-else if 链，每个路由都重复相同的认证检查模式，违反 DRY 原则。

**修复建议**: 提取声明式路由表和认证中间件。

### 3.4 [中] 错误处理不一致

**问题**: `sbQuery` 不检查 HTTP 状态码；部分函数返回数组，部分直接返回；多处空 catch 块静默吞没错误。

### 3.5 [中] 完全缺少日志记录

**问题**: 整个后端文件没有任何 `console.log` 或结构化日志。认证失败、数据库错误、外部 API 调用失败等关键事件均无日志。

### 3.6 [中] 函数命名不一致

**问题**: 混合风格: `handleLogin` (camelCase) vs `delEq` (缩写) vs `sg`/`ss` (过度缩写)。`sg` 和 `ss` 这种极短命名严重损害可读性。

---

## 4. 性能审查

### 4.1 [高] 无代码分割，同步加载

**问题**: 451KB 单文件一次性加载和解析；3 个外部 JS 库全部同步加载；外部依赖使用 `@latest` 标签存在兼容性风险。

**修复建议**: 锁定依赖版本号；为非关键脚本添加 `defer`/`async`；引入构建工具实现代码分割。

### 4.2 [中] Dashboard Stats 查询全量数据

**位置**: api/index.js `handleDashboardStats()`

**问题**: 查询 `select=*` 获取全部字段，但实际只用了 3 个字段。所有统计计算都在 Worker 端用 JS 完成。

**修复建议**: 使用 Supabase RPC 在数据库层完成聚合计算，添加边缘缓存。

### 4.3 [中] 飞书 API 串行调用

**问题**: 使用 `for...of` + `await` 串行处理每个日程，N 个日程总耗时为 N * 单次请求时间。

**修复建议**: 使用 `Promise.allSettled()` 并行处理。

### 4.4 [中] 无缓存策略

**问题**: 所有 API 响应都没有设置 `Cache-Control` 头。localStorage 无内存缓存层，每次操作都直接读写。

---

## 5. 最佳实践审查

### 5.1 [高] 可访问性严重不足

**问题**: 整个应用几乎没有使用 ARIA 属性；表单缺少 label 关联；模态框缺少焦点陷阱；自定义组件缺少键盘交互支持。

### 5.2 [中] 大量辅助脚本散落根目录

**问题**: 根目录存在 `check_syntax.py` 到 `check_syntax4.py` 共 4 个版本、`extract_scripts.py` 和 `extract_v2.py`、`find_error.js` 等 11+ 个临时辅助脚本。

**修复建议**: 清理并移入 `scripts/` 目录，将语法检查集成到 CI/CD 流水线中。

### 5.3 [中] CI/CD 重复部署

**问题**: 两个工作流在相同触发条件下同时运行（GitHub Pages + Cloudflare Pages），且都没有构建/测试/lint 步骤。

**修复建议**: 确定唯一主部署平台，添加 PR 触发的 CI 检查。

### 5.4 [中] 冗余/疑似未使用的文件

| 文件 | 状态分析 |
|------|----------|
| `index-simple.html` | 疑似早期原型，未被任何配置引用 |
| `api/worker.js` | 与 `api/index.js` 功能重叠 |
| `api/vercel.json` | 与根目录 `vercel.json` 重复 |
| `landing.html` | `_redirects` 无对应路由规则 |

### 5.5 [低] _headers 安全头配置不完整

**缺失**: `Strict-Transport-Security (HSTS)`、`Content-Security-Policy (CSP)`

### 5.6 [低] 文档过时

**问题**: README 最后更新日期为 2025-01-20；BUILD.md 描述的是 Tauri 桌面应用安装步骤，与实际 Cloudflare 部署方式不匹配。

---

## 6. 问题汇总表

| 级别 | 类别 | 问题描述 | 位置 | 修复建议 |
|------|------|----------|------|----------|
| 严重 | 安全 | 密码明文存储和比对 | api/index.js:550 | 使用 PBKDF2 哈希 |
| 严重 | 安全 | Token 可被伪造 (Base64) | api/index.js:536 | 改用 JWT + HMAC |
| 严重 | 安全 | 硬编码全部凭证 | api/index.js:4-17 | 迁移到环境变量 |
| 严重 | 结构 | 单文件 8327行/451KB | index.html | 拆分为模块 |
| 严重 | 结构 | .gitignore 缺失 | 根目录 | 创建 .gitignore |
| 高 | 安全 | XSS: AI 内容未消毒 | index.html:6900 | 统一使用 escHtml |
| 高 | 安全 | IDOR: 删除缺所有权验证 | api/index.js:575 | 加 user_id 过滤 |
| 高 | 安全 | CORS: Allow-Origin * | api/index.js:479 | 限制为白名单 |
| 高 | 安全 | 无速率限制 | 多个端点 | 添加滑动窗口限流 |
| 高 | 质量 | 全局命名空间污染 | index.html | 使用 ES Module |
| 高 | 性能 | 无代码分割，同步加载 | index.html:19-21 | 添加 defer/async |
| 高 | 实践 | 可访问性严重不足 | index.html 全文 | 添加 ARIA 属性 |
| 中 | 安全 | 输入验证严重不足 | 多个 Handler | 添加 Schema 验证 |
| 中 | 安全 | 错误信息泄露堆栈 | api/index.js:927 | 返回通用错误 |
| 中 | 质量 | 路由巨型 if-else 链 | api/index.js:809 | 提取路由表 |
| 中 | 质量 | 完全缺少日志 | api/index.js | 添加结构化日志 |
| 中 | 性能 | Dashboard 查询全量数据 | api/index.js:750 | 数据库聚合 |
| 中 | 性能 | 飞书 API 串行调用 | api/index.js:590 | Promise.all |
| 中 | 性能 | 无缓存策略 | 全局 | 添加 Cache-Control |
| 中 | 结构 | 辅助脚本散落根目录 | 根目录 11+ 文件 | 移入 scripts/ |
| 中 | 结构 | CI/CD 重复部署 | .github/workflows/ | 统一部署平台 |

---

## 7. 修复优先级建议

### P0 - 立即修复（本周内）

1. **创建 .gitignore**，从 Git 中移除 `node_modules/` 和 `.wrangler/`
2. **将 api/index.js 中所有硬编码密钥迁移到环境变量**，并轮换已泄露的密钥
3. **修复 Token 机制**，使用 JWT + HMAC 签名替代 Base64
4. **实现密码哈希**（PBKDF2）替代明文存储

### P1 - 尽快修复（2 周内）

5. 修复 IDOR 漏洞（所有增删改操作加 `user_id` 过滤）
6. 修复 CORS 配置，限制为具体域名白名单
7. 统一使用 `escHtml` 消毒所有 innerHTML 赋值
8. 添加速率限制（登录、公开消息、图片生成端点）

### P2 - 计划修复（1 个月内）

9. 将 index.html 拆分为模块化结构，引入构建工具
10. 添加输入验证、日志记录、缓存策略
11. 重构后端路由结构，提取中间件
12. 清理辅助脚本和冗余文件，统一 CI/CD
13. 改善可访问性（ARIA、焦点管理、键盘导航）

---

*报告生成时间: 2026-05-23 | 审查工具: SOLO Code Review Agent*
