---
paths:
  - "**/*.ts"
  - "**/*.tsx"
  - "**/*.js"
  - "**/*.jsx"
  - "**/*.vue"
---

# 编码规范

## 命名

| 场景 | 规范 | 示例 |
|---|---|---|
| 组件文件 | PascalCase | `UserProfile.tsx` |
| Hook / 工具函数 / 普通模块 | camelCase | `useAuth.ts`, `formatDate.ts` |
| 常量 | UPPER_SNAKE_CASE | `MAX_RETRY_COUNT` |
| CSS Modules / 类名 | kebab-case | `.user-profile` |

## 导入顺序

组间空一行，顺序如下：

```ts
// 1. Node 内置
import path from 'path'

// 2. 第三方库
import { useEffect } from 'react'
import { useQuery } from '@tanstack/react-query'

// 3. 内部绝对路径（alias）
import { Button } from '@/components/ui'

// 4. 相对路径
import styles from './index.module.css'
```

项目配置了 `eslint-plugin-import` 或排序插件时，以工具输出为准。

## 模块导出

- 优先命名导出（named export），便于 tree-shaking 和 IDE 跳转
- `index.ts` 只做 barrel 导出，不写业务逻辑
- 发现循环依赖立即拆分

## 代码风格

- 用 `async/await`，不写 `.then()` 链
- 函数组件 + Hooks，不写 Class 组件
- 复杂逻辑提取为自定义 Hook（`use` 前缀）
- 避免魔法数字，提取为具名常量
- 单文件不超过 300 行，超出则拆分

## 错误处理

- 异步操作必须有 catch，不允许静默吞掉错误
- 用户可见操作失败必须有提示（Toast / 行内错误信息）
- 关键业务模块用 Error Boundary 包裹，提供降级 UI