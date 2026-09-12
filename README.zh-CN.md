# Monash Exchange Tracker — 公开演示版

[English](README.md) · **简体中文**

[在线演示](https://waldo0926.github.io/monash-abroad-tracker-demo/) · [演示数据](./data/current.json)

这是 **Monash Abroad 交换项目变化追踪器** 的公开作品集演示版。

完整项目会抓取 Monash 项目搜索页的数据，标准化项目字段，对不同运行结果进行字段级比较，记录变化，并生成可搜索、筛选和排序的查看页面。由于完整项目仍与在读课程项目有关，其生产仓库目前保持私有。

这个公开仓库只用于展示界面和数据结构，不公开生产环境中的真实抓取数据，也不公开完整抓取器实现。

> **虚构数据说明：** `data/current.json` 中的大学、国家/地区、URL、成绩要求、申请状态、可用性和名额数字全部为虚构内容。本仓库不包含任何真实的 Monash 交换项目记录。

## 这个 Demo 展示什么

- 按大学或国家/地区搜索
- 按项目状态、地区和 Exchange Availability 筛选
- 点击表头进行排序
- 展示上一轮分数线和预计名额
- 展示搜索页与详情页字段不一致的警告
- 使用与私有正式项目一致的 canonical JSON 结构来驱动页面

## 公开 Demo 与正式项目的区别

| 组成部分 | 正式项目 | 本公开 Demo |
| --- | --- | --- |
| Scraper / crawler | 位于私有仓库 | 不公开 |
| 数据结构 | 正式 tracker schema | 演示字段使用同样结构 |
| 数据 | 实际抓取记录 | 18 条完全虚构记录 |
| Viewer | 正式查看器 | 作品集安全版静态查看器 |
| 自动抓取 | 定时执行 | 不包含 |
| 历史变化追踪 | 有 | Demo 只用静态 metadata 表示 |

## 仓库结构

```text
.
├── index.html          # 静态搜索 / 筛选 / 排序页面
└── data/
    └── current.json    # 使用 tracker schema 的完全虚构数据
```

## 本地运行

页面通过 `fetch()` 加载 JSON，因此不要直接双击打开 `index.html`，而应启动一个简单的本地 HTTP server：

```bash
python3 -m http.server 8000
```

然后访问：

```text
http://localhost:8000/
```

## 为什么正式仓库保持私有

正式 tracker 中包含实际的数据收集实现以及真实抓取到的交换项目数据。当前保持私有，可以避免在课程项目仍处于活跃阶段时公开评估相关源代码和生产数据，同时又能通过本仓库向招聘方展示界面、数据模型和项目思路。

## 免责声明

这是一个独立的个人作品集项目，**不是 Monash University 官方服务**。本 Demo 中所有记录均为虚构内容，不能用于交换申请规划、资格判断或选校决策。
