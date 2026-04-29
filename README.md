# A股短线精灵 — Short-Term Stock Picker

> 基于 [WorkBuddy](https://www.codebuddy.cn) 的 A 股智能选股推荐系统 Skill，通过实时行情数据、技术面分析和市场情绪评估，每天从沪深主板、创业板、科创板、北交所四大市场各推荐 2 只短线标的。

## 特性

- **四大市场覆盖**：沪深主板、创业板、科创板、北交所，共 8 只候选标的
- **六维评分模型**：涨幅动量 / 量价齐升 / 板块情绪联动 / 技术形态 / 资金流入 / 情绪位置
- **两步筛选机制**：初筛（快速过滤 5~8 只） → 复筛（并行深度扫描 + 六维打分）
- **实时数据驱动**：通过 NeoData 金融数据接口获取实时行情，禁止使用训练数据编造行情
- **完整风控体系**：单只仓位上限、总仓位 60%、严格止损、资金面警告机制
- **标准化报告输出**：每次推荐生成结构化 Markdown 报告

## 文件结构

```
short-term-stock-picker/
├── skill/
│   ├── SKILL.md                          # 核心 Skill 定义文件（执行流程、评分模型、风控规则）
│   └── references/
│       ├── report-template.md            # 推荐报告标准模板
│       ├── sentiment-framework.md        # 市场情绪打分体系
│       ├── short-term-rules.md           # 短线交易核心规则与禁忌
│       └── technical-analysis.md         # 技术面分析框架
├── README.md                             # 本文件
└── LICENSE                               # MIT License
```

## 安装

### 前置条件

1. 已安装 [WorkBuddy](https://www.codebuddy.cn) 桌面端
2. 已配置 **NeoData 金融数据** Skill（数据来源）

### 安装步骤

**方法一：手动安装（推荐）**

1. 克隆或下载本仓库：
   ```bash
   git clone https://github.com/lijq126/short-term-stock-picker.git
   ```

2. 将 `skill/` 目录下的所有文件复制到 WorkBuddy 用户级 Skill 目录：
   - **Windows**: `%USERPROFILE%\.workbuddy\skills\short-term-stock-picker\`
   - **macOS / Linux**: `~/.workbuddy/skills/short-term-stock-picker/`

   目录结构应为：
   ```
   ~/.workbuddy/skills/short-term-stock-picker/
   ├── SKILL.md
   └── references/
       ├── report-template.md
       ├── sentiment-framework.md
       ├── short-term-rules.md
       └── technical-analysis.md
   ```

3. 重启 WorkBuddy，Skill 即可使用

**方法二：通过 WorkBuddy Skill 管理器安装**

1. 在 WorkBuddy 中打开 Skill 管理器
2. 选择"从 GitHub 导入"
3. 输入 `lijq126/short-term-stock-picker`
4. 确认安装

## 使用方法

安装完成后，在 WorkBuddy 对话中输入以下任意指令即可触发：

| 触发词 | 示例 |
|--------|------|
| 推荐股票 | "帮我推荐今天的短线股票" |
| 选短线 | "帮我选几只短线" |
| 今天买什么 | "今天买什么好" |
| 强势股 | "今天有哪些强势股" |
| 今日金股 | "今日金股推荐" |
| 短线推荐 | "给我一个短线推荐" |

## 执行流程

```
第一步：查询全市场行情数据（大盘情绪 + 分市场强势股 + 板块资金）
    ↓
第二步：初筛候选池（涨幅/量价/板块三维度快速过滤，每市场 5~8 只）
    ↓
第三步：复筛深度扫描（并行查询个股行情/技术面/资金流向/近期走势）
    ↓
第四步：六维综合评分（涨幅动量20% + 量价齐升20% + 板块情绪20% + 技术形态20% + 资金15% + 情绪5%）
    ↓
第五步：生成推荐报告（标准化 Markdown 格式）
```

## 六维评分模型

| 维度 | 权重 | 说明 |
|------|------|------|
| 涨幅动量 | 20% | 今日涨幅、近 3 日涨幅加速度 |
| 量价齐升 | 20% | 当日量比、成交额放大倍数 |
| 板块与情绪联动 | 20% | 板块涨幅 + 情绪热度 |
| 技术形态 | 20% | MA + MACD + KDJ + BOLL + 量价 |
| 资金流入 | 15% | 主力净流入、超大单行为 |
| 情绪位置 | 5% | 连板位置、追高风险 |

## 风控规则

- 推荐候选池 8 只，实际精选 ≤ 5 只同时持仓
- 单只仓位上限：主板 20% / 创业板 15% / 科创板 15% / 北交所 10%
- 总仓位上限 60%
- 无条件止损 -5%
- 盈利 ≥ 8% 减半仓
- 大盘当日跌幅 > 2% 停止新开仓

## 数据来源

- **NeoData 金融数据**（`neodata-financial-search` Skill）—— 唯一数据来源
- 覆盖股票行情、技术指标、资金流向、板块数据、宏观指标等

## 注意事项

> ⚠️ **免责声明**：本系统基于技术面、资金面和市场情绪数据分析，不构成任何投资建议。短线交易风险极高，请在了解自身风险承受能力后独立决策。股市有风险，投资需谨慎。

## License

[MIT](./LICENSE)
