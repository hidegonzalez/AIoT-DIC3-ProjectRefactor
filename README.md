# NutriAgent: Three Core Innovations

本研究提出 **NutriAgent**：一套結合穿戴式物聯網、臨床營養規則與大型語言模型的接地式對話外食決策代理人，旨在突破通用聊天機器人營養數值不穩定、缺乏臨床安全約束、且無法掌握點餐當下情境的限制。

本系統具備三大核心創新：

### 1. Rule-First Safety Gate — 規則優先的臨床安全閘門

依據血壓、血糖、尿酸、LDL-C、eGFR 等健檢指標，以及 TFDA 11 類過敏原，建立確定性的臨床硬約束閘門。候選餐點先通過閘門，LLM 才會進場，確保任何模型輸出都無法讓不安全的餐點出現在推薦中。

### 2. Grounded & Resilient Generation — 接地生成與韌性降級

LLM 只負責理解意圖與生成回覆，所有營養數值一律來自資料庫。回覆中的數字逐一與資料庫比對，不一致即改用確定性模板；LLM 逾時或失效時，自動降級為離線引擎，安全性不受影響。

### 3. IoT-Driven Context Awareness — 物聯網驅動情境感知

整合穿戴裝置活動量、用餐時段、地理位置與歷史回饋，計算帶誤差意識的動態熱量預算，並從步行可達的餐點中，以本地偏好與 LLM 語義評分混合排序出 Top-3。使用者的回饋會即時更新偏好權重，形成 Human-in-the-Loop 閉環。

---

**核心研究價值：** 從流暢但未經驗證的通用 AI 飲食建議（Fluent but Unverified Dietary Advice），邁向安全由架構保證、數值可驗證的情境感知外食決策（Safety-by-Design, Grounded Dietary Decision-Making）。

---

## DIC3-Project Proposal Refactor using OpenSpec 專案導覽

| 作業項目 | 對應檔案路徑 | 內容說明 |
| :--- | :--- | :--- |
| **1. OpenSpec 規格驅動開發環境** | [`openspec/`](./openspec/) | 初始化 OpenSpec 結構與 Antigravity 工具鏈設定 |
| **2. Project A to F + 5 Cores + 3 Trade-offs + 10 Details + IEEE Abstract** | [`NutriAgent_A_to_F_Core_Debates_Details.md`](./NutriAgent_A_to_F_Core_Debates_Details.md)<br>[`myplan/2026-11-11-NutriAgent-conversation-plan.md`](./myplan/2026-11-11-NutriAgent-conversation-plan.md) | 融合 2025–2026 最新外部研究與案例之 A–F 主線、5 大核心貢獻、3 大架構取捨、10 項系統細節、預期貢獻與學術價值、300 字中文摘要與 IEEE Systems Journal Style 英文摘要 |
| **3. Convert `DESIGN.md` to Workflow in OpenSpec** | [`DESIGN.md`](./DESIGN.md)<br>[`openspec/specs/nutriagent-pipeline/spec.md`](./openspec/specs/nutriagent-pipeline/spec.md)<br>[`openspec/changes/nutriagent-workflow/`](./openspec/changes/nutriagent-workflow/) | 將 `DESIGN.md` v2.0.0 之 8 階段流水線與 4 段演算法轉換為 OpenSpec `proposal.md`、`design.md`、`spec.md` 與 `tasks.md` |
| **4. Introduction Refactor（緒論重構）** | [`myplan/NutriAgent_Introduction_RelatedWork_Refactor.md`](./myplan/NutriAgent_Introduction_RelatedWork_Refactor.md) | 結合 A-B-C 敘事主線與 2025–2026 臨床／公衛數據重構之完整緒論（Ch.1） |
| **5. Related Work Refactor（相關研究重構）** | [`myplan/NutriAgent_Introduction_RelatedWork_Refactor.md`](./myplan/NutriAgent_Introduction_RelatedWork_Refactor.md) | 五大文獻軸線、重構後關鍵文獻比較總表（Article Summary Table）與 Research Gap 收斂（Ch.2） |
| **6. A4 橫式高密度 NUMBERING 資訊圖表** | [`NutriAgent_A4_Landscape_Infographic.html`](./NutriAgent_A4_Landscape_Infographic.html)<br>[`NutriAgent_A_to_F_Storyline_Infographic.html`](./NutriAgent_A_to_F_Storyline_Infographic.html) | A2Z 論文寫作規範與 NutriAgent A-to-F 高密度編號資訊圖表 |
| **7. 原始簡報與設計文件擷取索引** | [`myplan/README-attachments.md`](./myplan/README-attachments.md) | 25 頁 Proposal 簡報逐頁文字擷取與 `DESIGN.md` 原始紀錄 |
