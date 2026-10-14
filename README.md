# Supabase Keepalive

通过 GitHub Actions 定时请求 Supabase，防止免费版因不活跃被暂停。

## 使用前准备

### 1. 建表

在 Supabase SQL Editor 执行：

```sql
CREATE TABLE IF NOT EXISTS keepalive (
  id SERIAL PRIMARY KEY,
  pinged_at TIMESTAMP DEFAULT NOW()
);
```

### 2. 配置 Secrets

在 GitHub 仓库 Settings → Secrets and variables → Actions 中添加：

| Secret | 值 |
|---|---|
| `SUPABASE_URL` | 你的项目 URL，如 `https://xxx.supabase.co` |
| `SUPABASE_KEY` | Service Role Key（`sb_secret_` 开头） |

## 触发方式

- **自动**：每天北京时间 09:00 和 21:00
- **手动**：Actions → Supabase Keepalive → Run workflow

## 工作原理

每次触发向 `/rest/v1/keepalive` 发一条 POST 请求，在 `keepalive` 表写入一条带时间戳的记录，让 Supabase 判定为活跃状态。