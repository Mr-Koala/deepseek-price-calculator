# DeepSeek 双向预算分析器

纯前端单页面工具，用于估算 DeepSeek V4 API 的成本与 Token 用量。

## 功能

- **双向计算**：预算→Token（给定金额算用量）/ Token→费用（给定用量算费用）
- **双模型支持**：DeepSeek-V4-Pro 和 DeepSeek-V4-Flash
- **缓存命中率**：考虑缓存命中/未命中的混合输入单价
- **混合模式**：按比例拆分预算，同时计算 Pro + Flash 的组合用量
- **单位联动**：预算单位（元/Token数）、Token 显示单位（百万/亿）自动联动

## 模型定价

| 模型 | 缓存命中 | 未命中 | 输出 |
|------|---------|--------|------|
| DeepSeek-V4-Pro | ¥0.025/M | ¥3/M | ¥6/M |
| DeepSeek-V4-Flash | ¥0.02/M | ¥1/M | ¥2/M |

## 使用方式

直接用浏览器打开 `deepseek_calculator.html` 即可，无需后端服务。

## 演示

<!-- 截图请放到 docs/ 目录下，并替换下方路径 -->
### 按金额预算计算
![演示截图](docs/screenshot_1.jpg)

### 按token预算计算
![演示截图](docs/screenshot_2.jpg)
