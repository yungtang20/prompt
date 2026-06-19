# Prompt Framework

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-yellow.svg)](https://www.anthropic.com/products/claude-code)

一套完整的任務管理工作流程 Skill，專為 Claude Code 設計。根據任務複雜度自動選擇適當的執行模式，從簡單的問答到複雜的多階段研究都能涵蓋。

## 功能特色

- **三級任務分級** — Simple / Medium / High 自動判斷
- **Full Framework** — 高複雜度任務的完整規劃模板
- **自動驗證機制** — 最多 3 次重試，確保品質
- **狀態持久化** — 自動記錄每次任務的狀態到 `.claude/memory/`
- **繁體中文優先** — 預設使用台灣用語

## 安裝

### 方法一：直接複製（推薦）

```bash
# 在你的專案中執行
mkdir -p .agents/skills
cp -r path/to/prompt-framework .agents/skills/prompt-framework
```

### 方法二：連結到現有 skills

```bash
ln -s /path/to/prompt-framework .claude/skills/prompt-framework
```

### 方法三：全域安裝

將技能目錄放到 Claude Code 的全域 skills 路徑：

```bash
# macOS / Linux
cp -r prompt-framework ~/.claude/skills/

# Windows
xcopy /E /I prompt-framework "%USERPROFILE%\.claude\skills\prompt-framework"
```

## 使用方式

在 Claude Code 中輸入 `/prompt` 即可觸發此技能。

### 範例

**Simple 任務：**
> 「幫我翻譯這段英文」

**Medium 任務：**
> 「幫我分析這三個競品的優缺點」

**High 任務：**
> 「深度調研台灣半導體產業的未來趨勢，包含供應鏈、技術發展、地緣政治影響」

## 工作流程

```
收到任務
    │
    ▼
判斷複雜度
    │
    ├─ Simple ──→ 直接回應
    │
    ├─ Medium ──→ 簡短計畫 → 執行 → 提示可調整
    │
    └─ High ──→ Full Framework 計畫 → 確認 → 執行 → 驗證 → 持久化
```

## 檔案結構

```
prompt-framework/
└── SKILL.md          # 技能主檔案，包含完整工作流程定義
```

## 授權

MIT License — 歡迎自由使用、修改、散布。
