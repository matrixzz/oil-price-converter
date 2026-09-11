---
name: update-gas-prices
description: 从 AAA 和汽油价格网抓取最新美国/中国油价并更新 index.html 中的数据
disable-model-invocation: true
allowed-tools: Read Edit WebFetch Bash
---

# 更新油价数据

从 AAA Gas Prices 和汽油价格网获取最新美国/中国油价并更新 `index.html`。

## 步骤

1. **抓取最新油价** — 使用 WebFetch 从以下 AAA 页面获取各城市 Regular Unleaded 油价：
   - 西雅图 (Seattle-Bellevue-Everett): `https://gasprices.aaa.com/?state=WA`
   - 旧金山 (San Francisco): `https://gasprices.aaa.com/?state=CA`
   - 洛杉矶 (Los Angeles-Long Beach): `https://gasprices.aaa.com/?state=CA`
   - 纽约 (New York): `https://gasprices.aaa.com/?state=NY`
   - 华盛顿DC: `https://gasprices.aaa.com/?state=DC`
   - 奥斯丁 (Austin-San Marcos): `https://gasprices.aaa.com/?state=TX`

   对每个页面，提取对应 **metro area** 的 Regular Unleaded 价格（不是州平均价）。CA 页面需同时提取旧金山和洛杉矶两个城市。

2. **抓取中国油价** — 用 Bash 抓取汽油价格网首页（只需一次请求，含全部省份）：

   ```bash
   curl -s -m 20 -L http://www.qiyoujiage.com/ -o /tmp/qyj.html
   ```

   从页面的省份表格中提取 **92号汽油** 价格，映射到这四个地区：

   | 页面地区 | index.html key |
   |---|---|
   | 北京 | `beijing` |
   | 上海 | `shanghai` |
   | 广东 | `shenzhen` |
   | 湖南 | `changsha` |

   注意该站只有 HTTPS 证书错误的 HTTP 版本，必须用 `http://`。

3. **更新 index.html** — 修改 `cityPrices`（美国，3位小数）和 `cnCityPrices`（中国，2位小数）两个对象的 `price` 字段，并把 `priceUpdateDate` / `cnPriceUpdateDate` 更新为今天的日期（格式 YYYY-MM-DD）。

4. **验证** — 读取修改后的文件确认数据正确。

5. **提交并部署** — 提交更改并推送到 GitHub，自动触发 GitHub Pages 部署：
   ```
   git add index.html
   git commit -m "Update gas prices YYYY-MM-DD"
   git push
   ```

## 注意事项

- 油价精度保持3位小数（如 5.602），与 AAA 网站一致
- 如果某个城市的油价获取失败，保留原有数据并告知用户
- 中国油价是发改委按**省**定的最高零售价，每 10 个工作日调整一次，不是抽样统计值。深圳用广东省价、长沙用湖南省价（该站的城市页面只是照抄省级数据）
- 汽油价格网的 `shenzhen.shtml` 是废弃页面（仍是 90#/93#/97# 老标号，数据停在 2017 年），不要用
- WA 页面找 "Seattle-Bellevue-Everett"，CA 页面找 "San Francisco" 和 "Los Angeles-Long Beach"，NY 页面找 "New York" metro area，DC 页面找州平均价（DC 只有一个区域），TX 页面找 "Austin-San Marcos"
