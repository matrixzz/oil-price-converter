---
name: update-gas-prices
description: 从 AAA 网站抓取最新美国城市油价并更新 index.html 中的数据
disable-model-invocation: true
allowed-tools: Read Edit WebFetch Bash
---

# 更新美国油价数据

从 AAA Gas Prices 网站获取最新油价并更新 `index.html`。

## 步骤

1. **抓取最新油价** — 使用 WebFetch 从以下 AAA 页面获取各城市 Regular Unleaded 油价：
   - 西雅图 (Seattle-Bellevue-Everett): `https://gasprices.aaa.com/?state=WA`
   - 旧金山 (San Francisco): `https://gasprices.aaa.com/?state=CA`
   - 洛杉矶 (Los Angeles-Long Beach): `https://gasprices.aaa.com/?state=CA`
   - 纽约 (New York): `https://gasprices.aaa.com/?state=NY`
   - 华盛顿DC: `https://gasprices.aaa.com/?state=DC`
   - 奥斯丁 (Austin-San Marcos): `https://gasprices.aaa.com/?state=TX`

   对每个页面，提取对应 **metro area** 的 Regular Unleaded 价格（不是州平均价）。CA 页面需同时提取旧金山和洛杉矶两个城市。

2. **更新 index.html** — 用 Edit 工具修改 `index.html` 中的 `cityPrices` 对象，更新每个城市的 `price` 字段为最新值（保留3位小数）。同时将 `priceUpdateDate` 更新为今天的日期（格式 YYYY-MM-DD）。

3. **验证** — 读取修改后的文件确认数据正确。

4. **提交并部署** — 提交更改并推送到 GitHub，自动触发 GitHub Pages 部署：
   ```
   git add index.html
   git commit -m "Update gas prices to AAA data YYYY-MM-DD"
   git push
   ```

## 注意事项

- 油价精度保持3位小数（如 5.602），与 AAA 网站一致
- 如果某个城市的油价获取失败，保留原有数据并告知用户
- WA 页面找 "Seattle-Bellevue-Everett"，CA 页面找 "San Francisco" 和 "Los Angeles-Long Beach"，NY 页面找 "New York" metro area，DC 页面找州平均价（DC 只有一个区域），TX 页面找 "Austin-San Marcos"
