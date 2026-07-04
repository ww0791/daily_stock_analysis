# GitHub Actions 免费数据源运行说明

这个仓库已经内置免费行情源，A 股优先使用腾讯、新浪、EFinance、AkShare，港股/美股等会按项目已有逻辑走 AkShare/YFinance 等兜底。因此只想先跑起来时，不需要配置 `TUSHARE_TOKEN`、`TICKFLOW_API_KEY` 或 `LONGBRIDGE_*`。

## 你必须配置的内容

在 fork 仓库进入：

`Settings` -> `Secrets and variables` -> `Actions`

### Secrets

确认已有：

```text
OPENAI_API_KEY
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
OPENAI_MODEL=gpt-5.5
```

## 手动跑一次

1. 打开 `Actions`
2. 选择 `Free Daily Stock Analysis`
3. 点 `Run workflow`
4. `mode` 建议第一次选 `stocks-only`
5. `force_run` 保持勾选，周末/休市也能测试流程
6. 跑完后，在 workflow run 页面下载 `free-analysis-reports-*` artifact

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

通知机器人不是必填。未配置企业微信、飞书、Telegram 等通知渠道时，报告仍会上传到 GitHub Actions artifact。
