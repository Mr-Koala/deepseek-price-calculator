# 部署与项目配置笔记

> 本文件记录了 deepseek-price-calculator 项目的 GitHub 部署配置要点，
> 方便后续新建类似项目时参考。

---

## GitHub Pages 部署

### 工作流文件位置
`.github/workflows/deploy-pages.yml`

### 关键配置
```yaml
permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

steps:
  - uses: actions/checkout@v4
  - uses: actions/configure-pages@v5
  - uses: actions/upload-pages-artifact@v3
    with:
      path: './deepseek_calculator.html'   # 只部署目标文件
  - uses: actions/deploy-pages@v4
```

### 仓库设置
- **Settings → Pages → Source**：选 **GitHub Actions**（不是 Deploy from a branch）
- **主页链接**：`gh repo edit --homepage "https://mr-koala.github.io/deepseek-price-calculator/deepseek_calculator.html"`
  或仓库 Settings → Website 输入框 → Save

---

## README 模板规范

### 顶部 Banner（访问入口）
```markdown
# 项目名

> 🚀 **在线使用**：[mr-koala.github.io/xxx](https://...) | 📦 [GitHub 仓库](https://...)
```

### 推荐结构
1. 一句话项目简介
2. 功能列表（要点式）
3. 定价/对比表（表格）
4. 使用方式
5. 截图演示
6. 英文版（可选，方便国际搜索）

---

## 纯前端 URL Hash 分享

```js
// 同步配置到 hash
function syncHash() {
  const params = new URLSearchParams({ b: budget, u: unit, ... });
  history.replaceState(null, '', '#' + params.toString());
}

// 从 hash 恢复配置
function loadFromHash() {
  const hash = location.hash.slice(1);
  if (!hash) return;
  const p = new URLSearchParams(hash);
  if (p.has('b')) budget.value = p.get('b');
}

// 复制分享链接
navigator.clipboard.writeText(location.href);
```

---

## 深色模式（CSS 变量方案）

### 优先级规则
手动选择（`data-theme`）> 系统偏好（`prefers-color-scheme`）

### 实现要点
```css
/* 亮色默认值定义在 :root */
:root { --body-bg: #f1f5f9; --card-bg: white; ... }

/* 跟随系统暗色 - 仅当用户未手动选亮色时生效 */
@media (prefers-color-scheme: dark) {
  :root:not([data-theme="light"]) { --body-bg: #0b1120; ... }
}

/* 手动暗色 */
[data-theme="dark"] { --body-bg: #0b1120; ... }
/* 手动亮色 */
[data-theme="light"] { --body-bg: #f1f5f9; ... }
```

```js
// JS 切换
localStorage.setItem('theme', 'dark');
document.documentElement.setAttribute('data-theme', 'dark');
```

### `:root:not([data-theme="light"])` 技巧
这个选择器意味着：只要用户没主动选"亮色模式"，暗色系统偏好就生效。
用户手动点切换按钮时，显式设置 `data-theme` 覆盖系统默认。

---

## 本项目提交记录

```
(最新) feat: 添加快捷预设标签和全输入模式，调整默认参数
c3745a0 docs: 添加部署与项目配置笔记
5e061cb docs: README 头部添加在线访问链接
edbfb21 fix: Pages 只部署 deepseek_calculator.html，排除无关文件
5b6699f feat: 添加深色模式切换和URL分享功能，补充README英文版
f8a1c16 feat: 优化页面样式并添加项目文档
e134452 feat: 初始化 DeepSeek 双向预算分析器
```

## 默认参数

当前默认 URL：`#b=1&u=token&d=yi&r=200&h=90&m=20`
- b=1：预算 1 亿 Token
- u=token：预算单位为 Token 数
- d=yi：Token 显示单位为亿
- r=200：输入输出比例 200:1
- h=90：缓存命中率 90%
- m=20：Pro 模型占比 20%

## 预设标签参数

| 参数 | 预设 | 值 |
|------|------|----|
| 比例 | 全输入 | 1:0（sentinel） |
| 比例 | 编程Agent | 200:1 |
| 比例 | 代码补全 | 50:1 |
| 比例 | 文档 | 10:1 |
| 比例 | 问答 | 5:1 |
| 比例 | 翻译 | 1:1 |
| 比例 | 全输出 | 0:1 |
| 缓存命中率 | 低/中/高/极致 | 50%/80%/90%/98% |
| Pro占比 | 穷/小康/富/任性 | 20%/40%/80%/100% |
