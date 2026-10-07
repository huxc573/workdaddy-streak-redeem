# WorkDaddy 连登奖励自动领取

> WorkDaddy 自动化任务（第三方）· 纯官方能力实现，**不需要任何补丁或修改版**
> 在官方原版 WorkDaddy 上即可直接运行。

逐个账号读取「活跃地图」的连登状态，把已达档、本月还没领的奖励自动领掉。

## 功能

- 覆盖 **7 / 14 / 28 天**三档，可累计领取，每档每月限 1 次
- **不切换账号、不触碰界面**：直接用各账号自己的 token 请求官方接口
- 未达档位、或本月已领过的档位自动跳过；奖励实时到账对应账号
- 运行日志逐档位记录结果，末尾一条汇总

## 安装

### 方式一：从 WorkDaddy 面板导入（推荐）

1. 打开 WorkDaddy → 自动化 → 「发现更多自动化任务」
2. 搜索 **连登奖励自动领取**
3. 点「导入」（导入后默认停用，请先自行评估再启用）

### 方式二：手动导入

把 `tasks/daily-growth-redeem.json` 的内容并入
`%APPDATA%\WorkDaddy\automations.json` 的数组里，然后重启 WorkDaddy。

## 任务参数

| 项 | 值 |
|---|---|
| 账号范围 | 全部账号（`switch:false`，不切号） |
| 触发 | 手动 |
| 定时 | 每 12 小时 |
| 幂等 | 依据官方返回的档位状态判断，已领自动跳过 |

## 用到的能力

全部是 WorkDaddy **官方原生 op**：

`account.forEach`（`switch:false`）、`http.requestAsAccount`、`logic.catch`、`logic.if`、
`value.uuid`、`value.number`、`vars.set`、`log.write`。

不涉及 `account.activate` 或任何需要改动 daemon / 注入层的能力，因此在未修改的官方
WorkDaddy 上可直接运行（任务使用 `schemaVersion: 2`）。

## 调用的官方接口

| 接口 | 用途 |
|---|---|
| `GET /activity/growth/streak` | 读取连登状态与各档位可否领取 |
| `POST /activity/growth/redeem` | 领取指定档位奖励（`tier` = `7d` / `14d` / `28d`） |

以上均为 `https://www.workbuddy.cn` 站点上的既有功能，任务只做「查状态 → 领取」。

## 目录

```
tasks/
  daily-growth-redeem.json   任务定义
```

## 常见问题

**在「发现更多自动化任务」里搜不到？**

该列表有 10 分钟缓存，且按仓库最后一次提交时间决定是否重扫。若刚建仓 / 刚更新时没显示，
等 10 分钟后重开面板即可；仍未出现欢迎提 issue。

## 免责声明

- 本任务仅调用你自己账号在官方站点上的既有功能，不破解、不修改官方客户端。
- 请自行评估使用风险，因使用本任务产生的任何后果由使用者承担。
- 与 WorkDaddy 官方无隶属关系。

## License

MIT © 2026 [huxc573](https://github.com/huxc573)
