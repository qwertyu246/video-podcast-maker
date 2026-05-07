# Topic Research — OpenClaw Intro

> 基於使用者描述的 OpenClaw 概念整理，非官方產品文件。

## OpenClaw 是什麼

OpenClaw 是一個 Agent 編排 orchestration 系統，旨在管理多個 Agent、工具、路由和任務流程的協調工作。

它的定位不是聊天機器人，而是 agent routing / orchestration layer，類似於工作流編排引擎，讓多個 Agent 能被管理、分流、綁定與執行。

## 與一般 Coding Agent 的差異

| 單一 Coding Agent (如 Claude Code) | OpenClaw |
|-----------------------------------|----------|
| 單一 Agent 承擔所有任務 | 多個 Agent 分工協作 |
| 單打獨鬥 | 編排層統一管理 |
| 任務排隊單一瓶頸 | 精準分流到對的 Agent |
| 單一入口 | 多入口（Discord/CLI/Webhook） |

## 核心概念

1. **Agent** — 執行任務的最小工作單位（Planner/Builder/QA/Researcher/Infra）
2. **Route** — 任務路由器，決定任務送往哪個 Agent
3. **Binding** — 將入口（Discord/CLI）綁定到特定 Agent
4. **Tool / Script** — Agent 可呼叫的擴展能力
5. **Agent Team** — 多個 Agent 的分工協作結構

## 實際運作流程

```
Discord / CLI / Webhook
    ↓
Binding（識別入口對應的 Agent）
    ↓
Route Layer（判斷任務類型）
    ↓
對應 Agent（Planner/Builder/QA/Infra 等）
    ↓
任務執行
    ↓
結果回報
```

## 使用情境

- Discord 多頻道分流（不同 channel 對應不同 Agent）
- 任務分工流水線（企劃 → 製作 → QA）
- 自動化測試（UI Test Agent）
- 文件整理（Researcher → Writer Agent）
- 基礎設施維運（Infra Agent 部署監控）

## 參考資源

本影片內容基於使用者提供的 OpenClaw 描述，無官方文件。
如有需要，請至 GitHub 搜尋 OpenClaw取得最新資訊。