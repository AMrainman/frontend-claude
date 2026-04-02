---
paths:
  - "**/*.ts"
  - "**/*.tsx"
  - "**/*.js"
  - "**/*.jsx"
  - "**/*.vue"
  - ".env*"
---

# 安全与环境变量

## 环境变量

- Vite：只有 `VITE_` 前缀的变量暴露给客户端
- Next.js：只有 `NEXT_PUBLIC_` 前缀的变量暴露给客户端
- **禁止**将 Secret Key / 数据库密码等敏感变量加公开前缀
- 新增环境变量时同步更新 `.env.example`，不提交 `.env` 文件

## 安全规范

- 禁止硬编码 API Key、密钥、token
- 用户输入渲染到 DOM 前必须转义；禁止使用 `dangerouslySetInnerHTML`
- 敏感操作（删除、支付、权限变更）需要二次确认
- 外链加 `rel="noopener noreferrer"`，防止 `window.opener` 攻击