---
paths:
  - "src/**/*.tsx"
  - "src/**/*.jsx"
  - "src/**/*.vue"
  - "pages/**/*.tsx"
  - "app/**/*.tsx"
---

# 状态管理与性能

## 状态管理

- 组件内部状态：`useState` / `useReducer`
- 跨组件共享：引入状态库前先考虑 Context + useReducer
- 服务端数据：用 React Query / SWR，**不在 `useEffect` 里手写 fetch 逻辑**
- URL 状态（筛选、分页、tab）：存入 query string，不放 useState

## 性能

- 列表渲染必须有稳定的 `key`（不用数组 index，除非列表静态不变）
- 大列表用虚拟滚动（`@tanstack/react-virtual` 等）
- 不在渲染函数中内联创建对象/数组/函数（导致子组件无意义重渲染）
- 图片用懒加载（`loading="lazy"`），首屏关键图片加 `fetchpriority="high"`
- 非首屏模块用 `React.lazy` / 动态 `import()` 拆包