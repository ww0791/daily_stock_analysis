# GitHub Actions 免费数据源运行说明

这个仓库已经内置免费行情源，A 股优先使用腾讯、新浪、EFinance、AkShare，港股/美股等会按项目已有逻辑走 AkShare/YFinance 等兜底。因此只想先跑起来时，不需要配置 `TUSHARE_TOKEN`、`TICKFLOW_API_KEY` 或 `LONGBRIDGE_*`。

## 你必须配置的内容

在 fork 仓库进入：

`Settings` -> `Secrets and variables` -> `Actions`

### Secrets

DeepSeek 用户配置：

```text
DEEPSEEK_API_KEY=你的 DeepSeek API Key
```

如果你不用 DeepSeek，也可以配置：

```text
OPENAI_API_KEY=你的 OpenAI API Key
```

### 飞书推送

最简单方式是配置飞书群自定义机器人 Webhook：

```text
FEISHU_WEBHOOK_URL=https://open.feishu.cn/open-apis/bot/v2/hook/...
```

如果飞书机器人开启了签名校验，再加：

```text
FEISHU_WEBHOOK_SECRET=飞书机器人签名密钥
```

### Variables

新增一个 Repository variable：

```text
STOCK_LIST=600519,000001
```

可以换成你的自选股，例如：

```text
STOCK_LIST=600519,hk00700,AAPL
```

可选：如果你的 OpenAI Key 不能使用默认模型，再新增：

```text
LITELLM_MODEL=deepseek/deepseek-chat
REPORT_TYPE=full
NOTIFICATION_REPORT_CHANNELS=feishu
```

## 手动跑一次

1. 打开 `Actions`
2. 选择 `Free Daily Stock Analysis`
3. 点 `Run workflow`
4. `mode` 建议选 `full`，会同时生成大盘复盘并把大盘环境注入个股推荐
5. `force_run` 保持勾选，周末/休市也能测试流程
6. 跑完后会推送到飞书；同时也可以在 workflow run 页面下载 `free-analysis-reports-*` artifact

## 默认定时

默认每个工作日北京时间 18:10 自动运行一次。

## 免费数据源说明

新 workflow 固定了：

```text
REALTIME_SOURCE_PRIORITY=tencent,akshare_sina,efinance,akshare_em
```

并关闭了需要额外服务或不稳定成本的增强项：

```text
ALPHASIFT_ENABLED=false
ENABLE_CHIP_DISTRIBUTION=false
```

推荐质量默认开启：

```text
REPORT_TYPE=full
DAILY_MARKET_CONTEXT_ENABLED=true
MARKET_REVIEW_ENABLED=true
```

未配置飞书时，报告仍会上传到 GitHub Actions artifact。
