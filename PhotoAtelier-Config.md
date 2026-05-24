# PhotoAtelier 项目配置信息

> 最后更新: 2026-05-24

---

## 部署地址

| 服务 | 地址 |
|------|------|
| **前端** | https://photoatelier.pages.dev |
| **后端 API** | https://photoatelier-api.photomagic.workers.dev |
| **GitHub 仓库** | https://github.com/ronineymessjr-sudo/photo-workflow |

---

## Cloudflare 配置

### Workers (后端)
- **Worker 名称**: `photoatelier-api`
- **地址**: https://photoatelier-api.photomagic.workers.dev
- **Dashboard**: https://dash.cloudflare.com → Workers & Pages

### Pages (前端)
- **项目名称**: `photoatelier`
- **地址**: https://photoatelier.pages.dev
- **Dashboard**: https://dash.cloudflare.com → Workers & Pages

### 登录方式
- 通过 `wrangler login` 浏览器授权
- 无需 API Token

---

## Supabase 配置

| 配置项 | 值 |
|--------|-----|
| **Project URL** | `woywgfoqurumrkyoznnb.supabase.co` |
| **Anon Key** | `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6IndveXdnZm9xdXJ1bXJreW96bm5iIiwicm9sZSI6ImFub24iLCJpYXQiOjE3Nzg4OTMxMjEsImV4cCI6MjA5NDQ2OTEyMX0.RQrLvKLrDQKayT_Sreel2uSx99SelrZJqr5f76bucDE` |
| **Dashboard** | https://supabase.com/dashboard/project/woywgfoqurumrkyoznnb |

### 数据库表
- `users` - 用户表
- `schedules` - 日程表
- `plans` - 方案表
- `messages` - 消息表

---

## JWT 配置

| 配置项 | 值 |
|--------|-----|
| **JWT_SECRET** | `photoatelier-jwt-secret-2025` |
| **过期时间** | 7 天 |

---

## 飞书配置 (可选)

| 配置项 | 值 |
|--------|-----|
| **App ID** | `cli_a90a10b74af85bd9` |
| **App Secret** | `k626pzEQO2adxuhZhty2If81t0BwIdzr` |

### 多维表格
| 表格 | app_token | table_id |
|------|-----------|----------|
| schedules | `IeTubz0IJaW31asIcpec3Q9znkg` | `tbl3bLzlKfA2tnli` |
| venues | `G88ebeTj4aFscFst3jscKrWunjh` | `tblQvOK4Lj5Ba2PS` |
| models | `ZGwtbqZpNahQfJsAkIOcQrZ3ntf` | `tblwzEsBiS9gpKdQ` |
| plans | `RVlrb6rKla7BAnsMj0vcryDznGf` | `tblzim7PKRvx2tec` |

---

## 常用命令

### 部署后端
```bash
cd C:\Users\user\Documents\trae-soio\photo-workflow
wrangler deploy
```

### 部署前端
```bash
cd C:\Users\user\Documents\trae-soio\photo-workflow
wrangler pages deploy . --project-name=photoatelier
```

### 设置密钥
```bash
echo "value" | wrangler secret put SECRET_NAME
```

### 查看密钥列表
```bash
wrangler secret list
```

---

## API 端点

| 端点 | 方法 | 说明 |
|------|------|------|
| `/health` | GET | 健康检查 |
| `/api/auth/login` | POST | 登录/注册 |
| `/api/schedules` | GET/POST | 日程管理 |
| `/api/plans` | GET/POST | 方案管理 |
| `/api/messages` | GET/POST | 消息管理 |
| `/api/dashboard/stats` | GET | 看板统计 |

---

## 故障排查

### 后端无法访问
1. 检查 Worker 是否部署成功: https://dash.cloudflare.com
2. 检查密钥是否设置: `wrangler secret list`
3. 测试健康检查: `curl https://photoatelier-api.photomagic.workers.dev/health`

### 前端无法访问
1. 检查 Pages 项目: https://dash.cloudflare.com
2. 检查 API 地址是否正确指向 Workers

### 登录失败
1. 检查 Supabase 连接
2. 检查 JWT_SECRET 是否一致
3. 检查密码哈希逻辑

---

## 相关文档

- [[DEPLOYMENT]] - 部署指南
- [[TESTING]] - 测试文档
- [[USABILITY-TESTING]] - 用户测试计划
