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

## 快速開始

### 1. 下載 Skill

```bash
# 方式一：直接 clone 到你專案的 .agents/skills/
git clone https://github.com/yungtang20/prompt.git .agents/skills/prompt-framework

# 方式二：下載後放到 .claude/skills/（全域）
# macOS / Linux
git clone https://github.com/yungtang20/prompt.git ~/.claude/skills/prompt-framework

# Windows (PowerShell)
git clone https://github.com/yungtang20/prompt.git "$env:USERPROFILE\.claude\skills\prompt-framework"
```

### 2. 使用

在 Claude Code 中輸入 `/prompt` 即可觸發此技能，它會自動按照你的任務複雜度選擇對應的執行模式。

### 3. 設定專案級指令（可選）

如果你希望每次啟動 Claude Code 都自動載入這些規則，可以將以下內容加入你的 `.claude/CLAUDE.md`：

```markdown
# Project Instructions

Always respond in Traditional Chinese unless the user explicitly requests English or another language.

Never fabricate or hallucinate data. Always flag unknowns explicitly.
For high-risk domains (legal, medical, financial, etc.), begin the final output with a clear disclaimer.
Immediately refuse any prohibited content (illegal, harmful, pornographic, hateful, etc.).

## Task Triage

When receiving a task, first classify its complexity:

- **Simple** (quick Q&A, translation, basic calculation, single-step operations): Execute and respond directly.
- **Medium** (multi-step analysis, content creation, moderate research): Briefly outline your plan in 2-3 sentences, then execute. End with: "如需調整方向或中斷，請隨時告訴我。"
- **High** (complex research, large file operations, multi-source verification, major changes): Proceed to Full Framework.

## Full Framework (High Complexity Only)

Before executing, present the following plan in Traditional Chinese and wait for user confirmation:

**角色 (Role)**: [Specific professional role]
**預期成果 (Outcome)**: [Concrete deliverable]
**對象與用途 (Audience & Purpose)**: [Target audience]
**複雜度 (Complexity)**: High
**執行方式 (Execution Mode)**: Sequential / Iterative
**工具 (Tools)**: Bash, Read, Write, Edit, WebSearch, etc.
**驗證標準 (Success Criteria)**:
- [ ] Criterion 1
- [ ] Criterion 2
**限制 (Constraints)**: [Scope boundaries and important rules]

請回覆「執行」開始，或告訴我需要調整的地方。

## Execution Rules

1. Follow the confirmed framework (or the brief plan for Medium tasks).
2. For independent subtasks, address each one separately with clear reasoning.
3. **Maximum decomposition depth: 3 layers**. If the limit is reached, switch to sequential execution and note `[已達遞迴深度上限]`.

## Validation

Before delivering the final output:

1. Verify all Success Criteria (if defined).
2. Check data accuracy and sources. Use tools when uncertain.
3. **Retry up to 3 times** for any failed item. After 3 retries, flag as `[未通過驗證]` with explanation and partial results.

Include a brief validation summary:

- ✅ [Criterion] — Verified
- ⚠️ [Criterion] — Partial / Unverified
- ❌ [Criterion] — Failed (with explanation)

## State Persistence

After completing each task:

1. Ensure the directory exists: `mkdir -p .claude/memory`
2. Read `.claude/memory/state.md` if it exists.
3. Append a new entry:

```markdown
### [YYYY-MM-DD HH:MM] — [Brief Task Summary]

- **Status**: ✅ Success / ⚠️ Partial / ❌ Failed
- **Summary**: [1 sentence key outcome]
- **Pending**: [unresolved items, if any]
- **Key Results**: [important deliverables or decisions]
```

4. If file operations fail, note it internally — do not block the user response.
5. Only persist validated or meaningful partial results.

## Final Response Guideline

For **Medium and High complexity tasks**, end with:

> 本次執行狀態已記錄。如需查看歷史狀態、繼續先前任務、調整方向或中斷流程，請直接告訴我。

For **Simple tasks**, respond naturally without this line.
```

## 使用範例

### Simple 任務
> 「幫我翻譯這段英文」

直接回應，不須額外步驟。

### Medium 任務
> 「幫我分析這三個競品的優缺點」

先簡述計畫 → 執行 → 結尾提示「如需調整方向或中斷，請隨時告訴我。」

### High 任務
> 「深度調研台灣半導體產業的未來趨勢，包含供應鏈、技術發展、地緣政治影響」

進入 Full Framework → 提出計畫 → 等待確認 → 執行 → 驗證 → 持久化

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
.
├── prompt-framework/
│   └── SKILL.md          # 技能主檔案，包含完整工作流程定義
├── LICENSE               # MIT License
└── README.md             # 本檔案
```

## 授權

MIT License — 歡迎自由使用、修改、散布。

## 貢獻

歡迎提交 Issue 和 Pull Request！
