# sniffer

> 嗅出個所以然。

橫跨軟體開發生命週期的 AI agent 技能組，嗅出程式碼的壞味道——無需打開黑盒。

每項技能都是針對特定稽核領域訓練出的鼻子：架構漂移、介面缺口、測試盲點、行為不符。Agent 回報偵測結果，**由你決定要打開哪裡。**

## 什麼是灰箱驗證？

灰箱介於黑箱（你什麼都看不到）與白箱（你看到所有東西）之間。資訊太多會讓審查者淹沒在細節裡；資訊太少則遮蔽了所有壞味道。技能只收集足夠讓有經驗的審查者作出判斷的資訊——然後停下來。**不下結論、不給分數、不提建議。**

**審查者才是行動者，而非 agent。** 壞味道由你來定義；sniffer 標出值得看的地方。報告成功的標準，是讓審查者知道下一步該問什麼——或確認可以安全上線；而不是窮舉所有可能的問題。三條規則體現這種分工：

- **摘要，而非裁決** — 輸出的是觀察到的事實。審查者自行辨識壞味道。
- **定位，而非展開** — 指出在哪裡，而非是什麼。審查者決定是否深挖。
- **一致的披露深度** — 每項技能停留在自己的層次。X 光讀的是骨頭，不是全身掃描。

三條規則都讓技能固定在一個停止點。

### 為何要停下？

停下是刻意的設計選擇，並非對完整性的妥協：

- 白箱審查並非「更完整的灰箱審查」——那是不同的活動。
- 當資訊完全透明時，戰略判斷就會消解成逐行比對。
- 調查是一種刻意的行為。報告標出壞味道的聚集位置；深入追查則留給審查者自行判斷。

### 運作方式

稽核是診斷性的，而非窮舉性的。每項技能帶有針對其稽核領域訓練的壞味道目錄——特定模式的辨識——並回報目標與之相符的位置。部分技能還透過*成對比對*（規格 vs 程式碼、設計 vs 計畫）偵測漂移，兩份相關文件之間的分歧本身就是一種壞味道。局部資訊就已足夠，因為目標是縮小不確定性，而非認證完整性。

因此，sniffer 不是 CI 閘門、持續監控或逐行審查工具——它是預審查的鼻子，在某個階段的產出物出現時被觸發，幫助審查者決定把注意力放在哪裡。

相同的形態——載入最少訊號、按需展開——在三個層次重複出現。Anthropic 用漸進式披露來管理技能*載入*；sniffer 將它延伸至技能*輸出*和*文件*：

| 層次 | 始終載入 | 觸發後載入 | 按需載入 |
|------|----------|------------|----------|
| 技能載入 | name + description 元資料 | SKILL.md 本體 | 捆綁的腳本／參考文件 |
| 技能輸出 | 頂部聚集 + 覆蓋率 | 完整觀察集（工作記憶） | 審查者詢問後展開 |
| 文件 | 主文件（決策 + 範圍） | `references/<topic>.md` | 依連結閱讀 |

## 技能列表

技能以扁平結構存放於 `skills/` 下（無階段子目錄）。以下依 SDLC 階段導覽，遵循 [agentskills.io](https://agentskills.io/home) 開放標準規範。

| 階段 | 技能 | 狀態 | 稽核領域 |
|------|------|------|----------|
| Define | [`requirements-doc-review`](skills/requirements-doc-review/SKILL.md) | v1 | 需求清晰度與可驗證性（問題空間） |
| Define | [`design-doc-review`](skills/design-doc-review/SKILL.md) | v1 | 設計文件健全性；視範圍需要時亦涵蓋需求追溯性 |
| Define | [`skill-design-review`](skills/skill-design-review/SKILL.md) | v1 | SKILL.md 設計品質（sniffer 原創 meta） |
| Plan | [`task-decomposition-review`](skills/task-decomposition-review/SKILL.md) | v1 | 任務列表結構品質（原子性／順序／垂直切片） |
| Plan | [`implementation-plan-review`](skills/implementation-plan-review/SKILL.md) | v1 | 計畫對設計的符合度結構準備程度；視範圍需要時亦涵蓋設計 vs 計畫繼承關係 |
| Build | [`interface-contract-review`](skills/interface-contract-review/SKILL.md) | v1 | 合約壞味道（API／schema／型別） |
| Build | [`ui-quality-review`](skills/ui-quality-review/SKILL.md) | v1 | UI 品質（無障礙性／響應式／狀態覆蓋） |
| Verify | [`test-coverage-review`](skills/test-coverage-review/SKILL.md) | v1 | 測試套件品質 |
| Verify | [`runtime-test-review`](skills/runtime-test-review/SKILL.md) | v1 | 執行期測試計畫／測試會話 |
| Verify | [`diagnosability-review`](skills/diagnosability-review/SKILL.md) | v1 | 可觀測性／操作手冊缺口 |
| Review | [`complexity-review`](skills/complexity-review/SKILL.md) | v1 | 結構可維護性 |
| Review | [`architecture-review`](skills/architecture-review/SKILL.md) | v1 | 模組邊界／依賴方向（sniffer 原創） |
| Review | [`security-review`](skills/security-review/SKILL.md) | v1 | 安全態勢 |
| Review | [`performance-review`](skills/performance-review/SKILL.md) | v1 | 效能態勢 |
| Review | [`doc-alignment-review`](skills/doc-alignment-review/SKILL.md) | v1 | 文件 vs 規格／程式碼對齊 |
| Review | [`doc-hygiene-review`](skills/doc-hygiene-review/SKILL.md) | v1 | 文件集衛生（一致性／連結／受眾／生命週期） |
| Ship | [`commit-quality-review`](skills/commit-quality-review/SKILL.md) | v1 | commit 歷史品質 |
| Ship | [`pr-review`](skills/pr-review/SKILL.md) | v1 | PR 合併就緒度（sniffer 原創） |
| Ship | [`pipeline-review`](skills/pipeline-review/SKILL.md) | v1 | CI/CD 健全性 |
| Ship | [`migration-risk-review`](skills/migration-risk-review/SKILL.md) | v1 | 棄用／遷移風險 |
| Ship | [`release-readiness-review`](skills/release-readiness-review/SKILL.md) | v1 | 發布就緒度 |

## 快速開始

<details>
<summary><b>Claude Code（推薦）</b></summary>

**Marketplace 安裝：**

```
/plugin marketplace add kirisame-wang/sniffer
/plugin install sniffer@sniffer-marketplace
```

> **SSH 錯誤？** Marketplace 以 SSH 方式 clone 儲存庫。若你尚未在 GitHub 設定 SSH 金鑰，可[新增 SSH 金鑰](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)，或僅對 fetch 改用 HTTPS：
> ```bash
> git config --global url."https://github.com/".insteadOf "git@github.com:"
> ```

</details>

sniffer 技能是帶有 `SKILL.md` 的資料夾（Anthropic Agent Skills 格式）。安裝後，針對某個產出物呼叫技能：

```
> Use design-doc-review on docs/architecture.md
```

Agent 輸出一份精簡的 `*-review.md` 報告，包含**頂部聚集**（跨多個類別的壞味道群）與**覆蓋率**摘要。完整觀察集保留在工作記憶中——提出後續問題（`展開矛盾`、`§3 附近有什麼`、`你會怎麼修 #2？`），agent 即在對話中呈現相符的觀察。

稽核階段隨報告結束。**後續對話是正常互動** — agent 可在被詢問時提供意見、建議與優先排序。

## 貢獻

請見 [`CONTRIBUTING.md`](CONTRIBUTING.md)。兩種貢獻路徑：

- **提交壞味道樣本** — 附加至既有技能的 `bad-smell-samples.md`
- **提交新技能** — 遵循 SKILL.md 範本；合併前須通過 `skill-design-review` 自我稽核

## 靈感來源

- **Matt Pocock — *Design the interface, delegate the implementation*（AI Engineer，2026-04）。** sniffer 延伸了他的「灰箱」理念——模組透過介面和測試來信任，而非逐行——從模組延伸至稽核輸出。
- [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) — 參考實作。sniffer 在有意義的審核對應上鏡射其技能領域（他們的技能*生產*，sniffer *稽核生產過程*）
- [Anthropic Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) — 官方格式規範
- [agentskills.io](https://agentskills.io/home) — 開放的跨廠商標準
