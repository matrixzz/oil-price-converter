# Oil Price Converter

美国油价 (USD/Gallon) 与中国油价 (CNY/Liter) 的双向转换静态网页，部署在 GitHub Pages。

## 项目结构

- `index.html` — 单文件静态网页，包含全部 HTML/CSS/JS
- `.github/workflows/pages.yml` — GitHub Pages 自动部署工作流

## 数据源

- **汇率**: 页面运行时通过 `open.er-api.com` API 实时获取 USD/CNY 汇率（备用: `exchangerate-api.com`）
- **油价**: 静态写入 `index.html` 中的 `cityPrices` 对象，数据来自 [AAA Gas Prices](https://gasprices.aaa.com)，需手动更新

## 更新油价

运行 `/update-gas-prices` 自动从 AAA 网站抓取最新数据并推送部署。

## 部署

- 仓库: `matrixzz/oil-price-converter`
- 网址: https://matrixzz.github.io/oil-price-converter/
- 推送到 `main` 分支自动触发 GitHub Actions 部署
