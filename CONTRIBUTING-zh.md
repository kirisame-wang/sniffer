# 貢獻指南

兩條貢獻路徑，選擇符合你情況的那條：

- **你遇到了某個既有技能沒抓到的真實壞味道** → [提交壞味道樣本](#提交壞味道樣本)
- **你想新增一個目前沒有技能涵蓋的稽核領域** → [提交新技能](#提交新技能)

閱讀本文前，建議先讀 [`README-zh.md`](README-zh.md) 了解專案背景與技能路線圖。

---

## 提交壞味道樣本

最快速的貢獻路徑。你只需將一個通用辨識模式附加至既有技能的 `bad-smell-samples.md`。

1. 確認應由哪項技能捕捉到這個壞味道（參見 [`README-zh.md`](README-zh.md) 的技能列表）
2. 開啟 `skills/<skill-name>/bad-smell-samples.md`
3. 附加一列描述該模式：
   - **Pattern 欄**：對此壞味道的通用、可移植描述（不含特定專案的事件或具名產出物）
   - **Smell category 欄**：對應該技能 Smell Profile 中的具名類別

每列應描述一個對應單一具名類別的通用形態。避免專案回憶錄（具體事件、有時間戳記的變更、具名產出物）——模式必須能跨專案移植。避免與既有列近似重複——若你的模式與現有列共享相同的底層形態，目錄已經涵蓋它。多個壞味道是否在共同證據上聚集形成 Layer 1 集中點，是執行期判斷，不在此預先撰寫。

4. 開一個 PR，包含新增的列，以及一段簡短說明你在哪裡看到這個例子（說明動機用；列本身保持通用）

---

## 提交新技能

較重型的貢獻。新技能在合併前必須遵循專案規範並通過自我稽核。

### 步驟

1. **確認稽核領域沒有重疊**，與現有或規劃中的技能不衝突（參見 [`README-zh.md`](README-zh.md) 的技能列表）。若你的技能會捕捉的壞味道已被涵蓋，請改為提議擴充既有技能。

2. **建立技能資料夾**：`skills/<your-skill-name>/SKILL.md`。命名規則：小寫 + 連字號，動名詞或名詞短語形式，不使用保留字（`anthropic`、`claude`）。

3. **撰寫 SKILL.md**，遵循既有技能使用的 7 節範本。必要章節：Frontmatter（`name`、`description`、`phase`）、Overview、When to Use、Smell Profile、Input Contract、Output Format、Out of Scope、References。以任何已收斂的 v1 技能作為參考（例如 [`design-doc-review`](skills/design-doc-review/SKILL.md)）。

4. **通過 DoD 自我稽核**——三道硬性關卡，全部必須通過：
   - **Gate 1** — 對你的 `SKILL.md` 執行 `skill-design-review`，並附上嗅探報告
   - **Gate 2** — 通過五項自我驗證標準：介面清晰度／內部封裝／單一職責／獨立執行／低耦合
   - **Gate 3** — 對真實（或最小合成）產出物呼叫該技能，確認輸出符合 `## Output Format` 規格

5. **填充 `bad-smell-samples.md`**，Smell Profile 中每個具名類別至少填入一列標準範例。通用模式；不含專案回憶錄；不含近似重複。

6. **開一個 PR**，包含：`SKILL.md`、`bad-smell-samples.md`、自我稽核報告，以及簡短說明（若有鏡射 agent-skills 的來源，請標明；若為 sniffer 原創亦請說明）。

### 會被拒絕的情況

技能在以下 DoD 項目失敗時將被拒絕：

- **範圍蔓延** — 一個技能涵蓋多個稽核領域（應拆分為兄弟技能）
- **HOW 過度指示** — 教導 agent 逐步操作方法，而非宣告職責；表面訊號：全篇充斥全大寫的 `ALWAYS`／`NEVER`／`MUST`
- **跨技能硬性依賴** — 執行期需要另一個兄弟技能
- **邊界洩漏** — Out of Scope 缺失、含糊，或延伸至後續對話的互動
- **規範違反** — frontmatter 格式、命名、參考路徑、行數限制

---

## 規範

- **Markdown 連結** — 使用標準 `[label](path)` 形式導覽，不使用 `@<path>` 前綴
- **正斜線** — 始終使用正斜線；不使用 Windows 風格的反斜線
- **description 欄位** — 第三人稱，同時包含*做什麼*與*何時觸發*；描述需夠具體，能在 100+ 技能競爭觸發中脫穎而出（泛化措辭即使範圍正確也會導致漏觸發）
- **參考文件一層深** — 從 SKILL.md 直接連結到支援文件；避免巢狀連結鏈
- **超過 100 行的參考文件** — 頂部加入目錄
- **SKILL.md 軟性目標 100 行／硬性警告 250 行** — sniffer 比 Anthropic 的 500 行指引更嚴格，原因是灰箱的注意力預算限制

跨廠商權威規範：[Anthropic Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) + [agentskills.io 開放標準](https://agentskills.io/home)。

---

## 有問題嗎？

專案設計問題，請開標記 `design` 的 issue。技能撰寫問題，標記 `skill-author`。壞味道校準爭議，標記 `calibration`。
