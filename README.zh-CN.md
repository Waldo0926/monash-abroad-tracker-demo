# Monash Exchange Tracker — 公开演示版

[![在线演示](https://img.shields.io/badge/%E5%9C%A8%E7%BA%BF%E6%BC%94%E7%A4%BA-waldo0926.github.io-2563eb?style=for-the-badge)](https://waldo0926.github.io/monash-abroad-tracker-demo/)
[![类型](https://img.shields.io/badge/%E7%B1%BB%E5%9E%8B-%E4%BD%9C%E5%93%81%E9%9B%86%E6%BC%94%E7%A4%BA-7c3aed?style=for-the-badge)](#)
[![数据](https://img.shields.io/badge/%E6%95%B0%E6%8D%AE-%E4%BB%85%E5%90%88%E6%88%90%E6%95%B0%E6%8D%AE-475569?style=for-the-badge)](#%E9%9A%90%E7%A7%81%E8%BE%B9%E7%95%8C)

[English](README.md) · **简体中文**

[公开演示](https://waldo0926.github.io/monash-abroad-tracker-demo/) · [系统架构](ARCHITECTURE.zh-CN.md) · [合成当前数据](./data/current.json) · [合成上一轮基准](./data/comparison.json)

这是我的 **Monash 学期交换项目追踪系统** 的作品集安全公开演示版。

私有正式系统会抓取项目搜索数据、标准化字段、进行字段级变化检测、保留历史快照、比较不同申请轮次、生成对比报告，并发布受访问控制保护的查看页面。本公开仓库只展示用户端的筛选、排序和变化对比流程，不公开真实生产数据、客户可见的实际对比结果，也不公开生产抓取器内部实现。

> **合成数据说明：** 本仓库中的院校、国家/地区、URL、日期、成绩、竞争状态、上一轮数值和名额均为虚构内容。数据仅用于驱动界面与变化检测演示，不来源于生产环境。

## 这个 Demo 展示什么

- 英文 / 简体中文界面切换
- 按大学或国家/地区搜索
- 地区、学期、时长、可申请校区、学院、申请状态和竞争状态等多维筛选
- 同一筛选维度内使用 OR，不同维度之间使用 AND
- **动态联动筛选数量**：其他条件变化时，各选项数量同步更新
- 根据当前筛选结果实时变化的 Total / Green / Yellow / Red / Closed 统计卡
- 表头交互式排序
- 最低要求、上一轮分数线与预计名额展示
- **运行时读取合成上一轮基准并自动比较**，生成竞争、名额、分数线、开放/关闭和新增项目等变化标签
- 搜索页与详情页字段不一致的警告展示
- 使用生产 schema 的精简演示字段结构
- 桌面端和小屏幕响应式布局

公开页面会明确标注为 **静态作品集 Demo**，而不是实时 Monash 数据服务。

## 对比功能如何工作

`data/current.json` 是完全合成的当前轮数据，`data/comparison.json` 是独立的合成上一轮基准。浏览器会真正比较两份 JSON，再动态生成变化标签；变化结果不是直接写死在 HTML 里的。

变化语义与正式查看器一致。例如：名额减少、同口径分数线上升视为不利变化；名额增加、同口径分数线下降、竞争缓和或重新开放视为有利变化。

## 公开 Demo 与私有正式系统

| 组成部分 | 私有正式系统 | 本公开 Demo |
| --- | --- | --- |
| Scraper / crawler | 定时抓取 | 不公开 |
| 数据标准化 | 完整生产流水线 | 通过合成 schema 演示 |
| 数据 | 实际抓取记录 | 12 条合成记录 |
| 历史追踪 | append-only changes + snapshots | 不公开 |
| 轮次比较 | 真实冻结基准 | 合成基准 |
| 对比报告 | 私有客户查看结果 | 不公开 |
| Viewer | 受访问控制保护的正式页面 | 公开作品集安全版 |
| 部署 | 私有生产环境 | GitHub Pages |

完整系统的脱敏架构说明见 [ARCHITECTURE.zh-CN.md](ARCHITECTURE.zh-CN.md)。

## 隐私边界

公开仓库明确不包含：

- 真实 Monash 交换项目记录；
- 真实历史快照和字段级变化日志；
- 真实上一轮冻结基准；
- 自动生成的对比 Excel / JSON 报告；
- 客户实际查看的对比结果；
- 生产账号、密码、访问控制配置和基础设施细节；
- 生产 scraper / crawler 源代码。

这样可以公开展示工程设计和交互能力，同时把实时数据、历史资产与客户服务留在私有生产环境中。

## 仓库结构

```text
.
├── index.html              # 搜索、多维筛选、变化标签、双语界面
├── ARCHITECTURE.md         # 英文脱敏架构说明
├── ARCHITECTURE.zh-CN.md   # 中文脱敏架构说明
├── README.md
├── README.zh-CN.md
└── data/
    ├── current.json        # 合成当前数据
    └── comparison.json     # 合成上一轮基准
```

## 本地运行

页面通过 `fetch()` 加载 JSON，因此应启动一个简单的本地 HTTP server：

```bash
python3 -m http.server 8000
```

然后访问：

```text
http://localhost:8000/
```

## 正式部署

真实 tracker 和历史数据位于独立的私有仓库与生产部署中。生产对比结果不会同步到这个公开仓库。

## 免责声明

这是一个独立个人作品集项目，**不是 Monash University 官方服务**。本 Demo 全部记录均为合成数据，不能用于交换申请规划、资格判断或选校决策。
