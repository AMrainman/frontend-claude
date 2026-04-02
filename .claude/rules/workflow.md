# 前端代码生成后的强制流程

适用条件：根目录存在 `package.json` 且包含前端框架依赖（react / vue / svelte / next / nuxt 等）。
每次完成前端代码编写后**自动执行**，无需在对话中提醒。

## 步骤一：依赖完整性检查

检查本次新增的 `import`，确认第三方包已在 `package.json` 中声明。
- 存在未声明的包 → 提示安装命令，**不自动执行**，由我决定
- 忽略相对路径导入和 Node 内置模块

## 步骤二：TypeScript 类型检查

检测根目录存在 `tsconfig.json` 则执行：
```bash
npx tsc --noEmit
```
- 类型错误必须修复后才能继续
- 禁止用 `as any` / `@ts-ignore` 绕过，除非附注释说明原因

## 步骤三：Prettier 格式化

检测配置文件（`.prettierrc` 系列 / `prettier.config.*` / `package.json#prettier`）：
- **存在** → `npx prettier --write <changed_files>`
- **不存在** → 跳过，不自行发明格式化规则

## 步骤四：ESLint 检查与修复

检测配置文件（`.eslintrc` 系列 / `eslint.config.*` / `package.json#eslintConfig`）：
- **存在** → `npx eslint --fix <changed_files>`，修复后重新检查
- 仍有无法自动修复的错误 → 任务结束时单独列出，不得忽略
- **不存在** → 跳过

## 步骤五：console.log 清理

移除改动文件中的调试日志：`console.log` / `console.debug` / `console.dir`
- 保留：`console.error` / `console.warn`
- 保留：行内注释标注 `// 保留` 的日志

## 步骤六：生成测试用例

- 跳过条件：纯样式调整、文案修改、配置文件变更
- 测试框架：从 `package.json#devDependencies` 判断（Vitest / Jest / Testing Library）
- 覆盖要求：正常路径 + 边界值 + 异常路径
- 测试描述用中文：`it('当输入为空时，应返回错误提示')`
- 禁止在测试中访问真实数据库或外部 API，必须 mock

## 步骤七：执行测试直到全部通过

```bash
npm run test   # 或 npx vitest run / npx jest --runInBand
```
- 失败 → 分析原因 → 修复 → 重新执行，循环直至通过
- 禁止删除用例或写无意义断言凑通过率
- 修复超过 3 次仍失败 → 停止并说明问题

## 步骤八：无障碍与语义检查（UI 组件）

- `<img>` 必须有 `alt`（纯装饰用 `alt=""`）
- 交互元素必须有可见文本或 `aria-label`
- 表单控件与 `<label>` 正确关联（`htmlFor` / `id` 匹配）
- 用语义化标签（`<button>` 而非 `<div onClick>`，`<nav>` / `<main>` / `<section>`）
- 不依赖纯颜色传递关键信息

## 步骤九：输出变更摘要

```
✅ 依赖检查：无新增未声明依赖
✅ TS 类型检查：通过
✅ Prettier：已格式化 3 个文件
✅ ESLint：已自动修复 2 处，无残留错误
✅ console.log：已清理 1 处
✅ 测试：新增 5 个用例，全部通过（共 23/23）
✅ 无障碍：无问题
⚠️  LoginForm 未覆盖网络超时场景，建议后续补充
```