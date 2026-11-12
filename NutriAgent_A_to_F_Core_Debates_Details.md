# NutriAgent 專題解析：A to F 論文主線、5 大核心、3 大爭點、10 項細節與 IEEE 摘要

> **專題名稱**：NutriAgent — 結合穿戴式 IoT、臨床營養規則與大型語言模型之對話式外食決策代理人  
> **英文全名**：*NutriAgent: A Grounded Conversational LLM Agent for Real-Time Dietary Decisions Integrating Wearable IoT and Clinical Nutrition Rules*  
> **組別／成員**：第 10 組 李偉聖、陳建均（指導老師：陳煥 教授）  
> **核心設計理念**：**「規則管安全與數字，LLM 管理解與表達——安全由架構保證，語言交給 LLM，數字只來自資料庫。」**

---

## 一、Project Storyline: A to F（論文摘要與敘事六段主線，融合 2025–2026 最新外部研究與案例）

本專題依循 **A-B-C-D-E-F（Attention Getter / Background $\rightarrow$ But / Need $\rightarrow$ Cure / Task $\rightarrow$ Development / Object $\rightarrow$ Experiments $\rightarrow$ Findings & Value）** 六階段結構展開，並結合 2025–2026 最新臨床文獻與真實新聞案例強化 **Interesting、Innovation、Important** 三大維度：

| 階段 | 角色定位 | 核心論述與內外部關鍵數據（2025–2026） |
| :--- | :--- | :--- |
| **A**<br>**Attention Getter**<br>（背景與日常切身性） | **問題與使用者的距離有多近？** | • **外食普及與慢性病年輕化**：台灣近 **70%** 民眾有外食習慣，逾 **40%** 上班族天天外食 [18]；20 歲以上成年人每 **10 人即有 3 人**符合代謝症候群 [17]，全球高血壓患者達 **14 億人**但控制率僅略高於 1/5 [15]。<br>• **高鈉與隱形腎病危機（外部數據補強）**：國民營養健康調查顯示逾 **63%** 成年國人每日鈉攝取超標 [40]；台灣慢性腎臟病（CKD）盛行率高達 **11.9%**，但患者自我認知率僅 **3.5%** [42]，每年約 **1.2 萬人**新進入透析（且以 40–64 歲中壯年佔比最高）[43, 44]。<br>• **AI 已成日常健康入口**：OpenAI（2026/1）報告指出，全球每天有超過 **4,000 萬人**向 ChatGPT 詢問健康與飲食問題，且約 **70%** 對話發生在診所看診時間以外 [35]。 |
| **B**<br>**But / Need**<br>（痛點與純 LLM 安全危機） | **為何通用 LLM 與既有工具無法安全解決？** | • **外食營養估算高變異與低估風險**：針對台灣 8 款 7-Eleven 即食便當測試 5 款主流聊天機器人，所有模型皆系統性**低估鈉含量**，且重複詢問之變異係數（CV）高達 **20%–70%** [5]；AI 生成之腎臟病飲食亦出現**鉀含量超出安全上限**、無一完全合規之問題 [30]。<br>• **真實醫療中毒案例與權威警告（外部案例補強）**：2025 年 8 月《Annals of Internal Medicine: Clinical Cases》通報一名 60 歲男性聽信 ChatGPT 食鹽替代建議，以溴化鈉替換氯化鈉三個月後中毒住院 [33]；**ECRI 更將「AI 聊天機器人誤用」列為 2026 年頭號醫療科技危害** [34]。<br>• **臨床實證盲點**：2026 年 2 月《Nature Medicine》針對 **N = 1,298** 名受試者之隨機對照試驗（Bean et al.）證實，一般民眾使用 GPT-4o、Llama 3 等模型判斷病況與處置之正確率低於 **34.5%** 與 **44.2%**，未優於傳統對照組 [37]。<br>• **既有方法各缺一角**：飲食記錄 App（事後記錄）、通用 LLM（數值幻覺）、Zero-shot LLM Ranker（位置偏誤 [27]）、學術健康推薦（MORL-A2C 將健康當成可交換軟目標，H-Score 升至 69.57% 但 Recall 降至 23.61% [2]）、穿戴式裝置（活動熱量 MAPE > 30% [10]）。 |
| **C**<br>**Cure / Task**<br>（研究目標與解方定位） | **本研究切入的任務與權威背書為何？** | • **核心研究任務**：回應《Nature Medicine》與溴化鈉中毒案後專家對於「整合醫學知識庫、自動風險標記與人機協同把關」之呼籲，提出 **NutriAgent**——一套將**決定論臨床與過敏原硬約束閘門（Deterministic Gate）**置於 LLM 之前的接地式對話外食決策代理人。<br>• **四大必須同時成立的條件**：① **安全硬約束**（LLM 任何輸出皆不可推翻）；② **數值接地**（熱量、蛋白質、鈉、價格、距離與資料庫 100% 一致）；③ **情境適配與個人化**（結合穿戴式預算與口語偏好）；④ **韌性與即時**（LLM 斷線或逾時仍安全運作）。 |
| **D**<br>**Development / Object**<br>（系統設計與方法實作） | **系統如何透過 8 模組與 4 段演算法實作？** | • **產業對照與差異化**：相較於 Google Health Coach（2026/5）[38]、ChatGPT Health（2026/1）[36] 或以小型模型做安全防護的 FoodGuardBench / FoodGuard-4B（arXiv 2604.01444）[39]，NutriAgent 採用**可單元測試、可稽核、不可越獄的決定論規則閘門**。<br>• **8 模組流水線與 4 段核心演算法**：<br>  1. **Algorithm 1（Agent 主迴圈）**：Planner 解析意圖（`recommend` / `feedback` / `explain_violation`），過敏原只增不減，調度 IoT、LBS、臨床閘門與混合排序。<br>  2. **Algorithm 2（臨床與 TFDA-11 硬約束閘門）**：依血壓、空腹血糖、尿酸、LDL-C、eGFR 與台灣食藥署 11 類過敏原逐條過濾候選餐點 $C$，產出安全池 $S$ 與拒絕紀錄 $V$；若 $S=\emptyset$ 絕不放寬規則，回傳 `NO_COMPLIANT_MEAL` 與點餐調整建議。<br>  3. **Algorithm 3（混合排序與數值驗證回覆）**：對亂序後的安全池計算本地偏好 $p$ 與 Gemini 語義分數 $g$（$q = 0.5p + 0.5g$），加權營養適配度與距離選出 Top-3；對 Gemini 回覆執行逐字數值驗證，不一致或逾時即退回確定性模板。<br>  4. **Algorithm 4（Human-in-the-Loop 線上偏好更新）**：依使用者回饋（採納 `+1.0`、太油 `-0.6`、跳過 `-0.2`）更新三維口味權重 $w$ 並存入互動記憶 Buffer $D$。 |
| **E**<br>**Experiments**<br>（實驗設計與原型檢核） | **如何驗證有無 LLM 都一樣安全？** | • **測試規模**：50 組合成臨床使用者（含 eGFR）、$\ge 200$ 筆台灣外食餐點（含鉀磷等級）、$\ge 200$ 句人工標註中文外食對話測試集，以及 10–15 人連續 5 個工作日午餐實測。<br>• **5 組基準對照（Baselines）**：B1 最近距離/熱門、B2 靜態熱量規則、**B3 Gemini Zero-Shot（不經閘門）**、**B4 消融實驗（移除閘門）**、B5 離線 NutriAgent。<br>• **原型初步功能檢核（9/9 單元測試通過）**：在 5 組合成檔案、45 次評估中，**臨床違規率（CVR）為 0.0%**、**過敏原違規率（AVR）為 0.0%**、**熱量平均絕對誤差（MAE）為 114.48 kcal**（優於 $\le 180\text{ kcal}$ 目標），離線核心延遲平均 **0.08 ms**（P95 **0.18 ms**）。 |
| **F**<br>**Findings & Value**<br>（預期成果與政策價值） | **這項研究帶來什麼實務與政策影響？** | • **政策無縫接軌**：呼應食藥署與國健署 2026 年 4 月推動之包裝與外食「紅黃綠」營養標示分級政策 [40]、國健署 2026「營養及健康飲食促進方案」[41]，以及 2026 國家慢性腎臟病防治政策建言書「降低新發透析人數」核心 KPI [42]。<br>• **核心研究價值**：將靜態營養標示轉化為點餐當下的個人化安全決策，示範「當 AI 已成 4,000 萬人的健康入口，安全不能交給提示詞，要交給架構」之 **Safety-by-Design** 領域代理人典範。 |

---

## 二、5 Cores（專題 5 大核心創新與貢獻）

對照 Proposal 簡報 Slide 11（五項創新）與 Slide 24（五項核心貢獻）：

1. **Core 1 — 閘門先於 LLM 的「規則管安全、LLM 管語言」解耦架構（Rule-First Safety Gate Architecture）**
   * 確立「安全判斷與營養數值完全由決定論規則與資料庫負責，意圖理解、語義評分與自然語言生成由 LLM 負責」的架構分工（含 4 段完整演算法 Pseudocode），確保 LLM 在任何情況下都只看得到已通過安全過濾的安全池 $S$。
2. **Core 2 — 可稽核、可單元測試的臨床與 TFDA 11 類過敏原硬約束規則庫（Auditable Clinical & TFDA-11 Allergen Gate）**
   * 整合 2025 AHA/ACC 高血壓指引 [12]、ADA 2026 糖尿病標準 [13]、2020 ACR 痛風指引 [14]、KDIGO 2024 慢性腎臟病指引 [31] 與衛福部食藥署 11 類強制過敏原標示（含複合詞別名展開，如「海鮮」自動展開為魚類＋甲殼類）[16]。每條排除規則皆記錄拒絕原因代碼至拒絕紀錄 $V$，支援向使用者解釋「為什麼這道不能吃」（`explain_violation`）。
3. **Core 3 — 數值接地、逐字驗證與確定性模板備援機制（Grounded Generation & Verbatim Numerical Verification）**
   * 要求 LLM 生成回覆時逐字引用資料 JSON 中的營養數值；系統於輸出前自動執行數值一致性比對（`verify_explanation_consistency`：檢查回覆中所有數字是否為資料庫數值之子集），一旦發現幻覺或竄改立即無縫切換為確定性模板回覆。
4. **Core 4 — 帶誤差意識的穿戴式 IoT 動態預算與抗偏誤混合排序（Error-Aware IoT Budgeting & De-biased Hybrid Ranking）**
   * 針對穿戴裝置熱量高估問題（MAPE > 30% [10]），以 `0.65` 阻尼係數折減活動熱量、設定單餐 `900 kcal` 上限，並設計跨感測器合理性檢查（步數 > 10,000 但平均心率 $\le 70\text{ bpm}$ 判定為行車震動假訊號）。在排序階段先對安全池進行隨機打亂（Shuffle Bootstrapping）消除 LLM 位置偏誤 [27]，再以本地偏好 $p$ 與 Gemini 語義分數 $g$ 各半混合（$q = 0.5p + 0.5g$）結合 Human-in-the-Loop 即時回饋更新。
5. **Core 5 — 離線韌性與台灣外食資料集／可重現開源原型（Offline Resilience, Local Dataset & Reproducible Prototype）**
   * 當無 API 金鑰、網路斷線或 Gemini API 超過 12 秒逾時，系統自動降級為離線引擎（$q = p$ 加上確定性模板），安全指標（CVR = 0%、AVR = 0%）絲毫不受影響；並建立台灣外食營養空間資料集、中文外食對話測試集與具備推理軌跡（Reasoning Trace）之開源原型。

---

## 三、3 Trade-offs（專題 3 大核心爭點與架構取捨）

1. **Trade-off 1：臨床安全性 vs. 推薦召回率與生成自由度（Safety Hard Constraints vs. Recall & Expressive Freedom）**
   * **爭點**：既有健康感知推薦（如 MORL-A2C [2]）將健康視為可與準確率交換的軟性獎勵（H-Score 提升但 Recall 下降），而純 LLM 雖能對任何餐點給出流暢建議，卻會在高血壓或腎病情境下產生致命超標建議 [5, 30, 33]。
   * **本專題取捨**：**絕對優先保障臨床與過敏原安全（Never Relax Rules）**。寧可承擔嚴格過濾後安全池 $S$ 為空的風險（此時透過 `NO_COMPLIANT_MEAL` 與 `generate_order_tips()` 提供「醬料減半、去皮、不喝湯」等點餐調整建議），也絕不放寬硬約束或允許 LLM 越過閘門推薦違規餐點。
2. **Trade-off 2：穿戴式熱量感測誤差 vs. 動態能量補償的實用性（Wearable Sensor Noise vs. Dynamic Calorie Compensation）**
   * **爭點**：若完全忽略穿戴式裝置活動量，高活動量當天的靜態熱量預算會導致使用者飢餓而放棄飲食控制（違反 DIETFITS 強調的長期依從性原則 [20]）；但若直接 1:1 採信穿戴裝置活動熱量（MAPE > 30% [10, 11]），又會造成熱量補償過度與暴食。
   * **本專題取捨**：採用**「保守阻尼折減 + 跨感測器防呆 + 單餐硬頂」**的工程折衷——僅將穿戴式活動熱量乘以 `0.65` 納入動態 TDEE，加上單餐最高 `900 kcal` 上限與「高步數低心率」行車震動濾波，在動態適應與防範感測器高估之間取得平衡。
3. **Trade-off 3：LLM 深度語義理解 vs. 線上延遲、位置偏誤與外部 API 依賴（Semantic Intelligence vs. Latency, Positional Bias & API Dependency）**
   * **爭點**：呼叫雲端 Gemini API 進行語義評分與自然語言生成會增加數秒延遲、產生候選順序偏誤 [27]，且面臨 API 逾時或健檢資料上傳雲端的隱私疑慮。
   * **本專題取捨**：採用**「本地與雲端各半混合（$q = 0.5p + 0.5g$）+ 12 秒逾時優雅降級 + 衍生旗標去識別化」**。LLM 僅在安全池內影響一半的偏好分數，且候選名單送入前先隨機打亂；一旦 API 異常或無金鑰，離線引擎可在 `< 1 ms` 內完成安全推薦。

---

## 四、10 Details（支撐系統運作的 10 項關鍵實作、演算法與驗證細節）

1. **跨感測器合理性檢查（Cross-Sensor Plausibility Check）**：於 `iot_pipeline.py` 中，若偵測到累計步數 `> 10,000 步` 但平均心率 `<= 70 bpm`，系統自動判定為搭乘交通工具之震動假訊號並予以校正；若穿戴資料讀取逾時或被拒，則退回靜態估計並標記 `LOW_CONF`。
2. **動態 TDEE 0.65 阻尼公式與單餐 900 kcal 上限**：基礎代謝率（BMR）採用 Mifflin-St Jeor 公式 [22]，動態總消耗為 $\text{TDEE} = \text{BMR} + 0.65 \times \text{ActiveKcal}$；再依早 25%、午 40%、晚 35% 分配當餐預算，並強制套用 $\min(\text{TDEE} \times \text{ratio}[t], 900\text{ kcal})$ 單餐上限。
3. **Haversine 兩段式空間檢索（1.2 km $\rightarrow$ 2.5 km）**：於 `spatial_lbs.py` 中，先以 Haversine 公式檢索使用者座標半徑 `1.2 km` 內之步行可達餐點；若候選餐點數 $|C| < 4$，自動擴大搜尋半徑至 `2.5 km` 一次。
4. **過敏原只增不減與 TFDA 11 類別名展開（Monotonic Allergen Union & Alias Expansion）**：對話過程中解析出的新過敏原只會與既有設定取聯集（$A \leftarrow A \cup I.\text{allergens}$，絕不因對話移除已宣告過敏原），並透過 `ALIAS_MAP` 將口語「海鮮」自動展開為衛福部 TFDA 11 類強制標示中的「魚類＋甲殼類」。
5. **五大慢性病臨床硬約束門檻（Deterministic Clinical Thresholds）**：於 `clinical_rules.py` 實作不可違反之排除條件：
   * **高血壓（HTN）**：$\text{SBP} \ge 130$ 或 $\text{DBP} \ge 85\text{ mmHg} \rightarrow$ 排除單餐鈉 $> 700\text{ mg}$（每日 $2,000\text{ mg} \div 3$ 餐取整之設計決策）或油炸餐點。
   * **糖尿病前期（PREDIABETES）**：空腹血糖 $\ge 100\text{ mg/dL} \rightarrow$ 排除高 GI 或添加糖 $> 5\text{ g}$ 餐點。
   * **痛風（GOUT）**：尿酸 $\ge 7.0\text{ mg/dL} \rightarrow$ 排除高普林餐點。
   * **高血脂（LDL）**：$\text{LDL-C} \ge 130\text{ mg/dL} \rightarrow$ 排除飽和脂肪 $> 5\text{ g}$ 或油炸餐點。
   * **慢性腎臟病（CKD）**：$\text{eGFR} < 60\text{ mL/min/1.73m}^2 \rightarrow$ 排除高鉀或高磷餐點。
6. **空安全池不放寬原則與點餐調整建議（Zero-Relaxation & Order Tips）**：當臨床閘門過濾後安全池 $S = \emptyset$ 時，系統嚴禁自動放寬醫療門檻，而是回傳 `NO_COMPLIANT_MEAL`，並由 `generate_order_tips()` 針對被拒原因提供外食實務調整建議（如去皮、醬料減半、不喝湯）。
7. **消除位置偏誤之混合排序公式（De-biased Hybrid Ranking Formula）**：送入 Gemini 進行 JSON 語義評分（`temperature = 0.1`）前，先對安全池執行 `shuffle(S)` 打亂順序；偏好分數為 $q = 0.5p + 0.5g$（離線時 $q = p$），最終綜合評分為：
   $$\text{Score} = 0.50 \cdot q + 0.35 \cdot \text{NutriFit} - 0.15 \cdot \min\left(1, \frac{\text{distance}}{1.5}\right)$$
   其中 $\text{NutriFit} = 1 - (0.6 \cdot \text{calorie\_error} + 0.4 \cdot \text{protein\_error})$。
8. **逐字數值驗證守門員（Verbatim Number Consistency Verification）**：於 Algorithm 3 第 15–16 行，系統擷取 Gemini 生成回覆（`temperature = 0.25`，附最近 4 輪對話）中的所有數值，檢驗是否為資料庫候選餐點與預算數值之子集（`numbers_in(Y_llm) subset_of db_values(R, budget)`）；若出現未授權數字或 API 超過 12 秒逾時，立即替換為確定性模板回覆。
9. **Human-in-the-Loop 三維口味權重更新（Online Preference Learning）**：於 Algorithm 4 中維護三維口味權重 $w = [\text{light}, \text{savory}, \text{protein}]$：
   * **採納（`accept`, $r = +1.0$）**：對應風味權重 $+0.08$（若蛋白質佔比 $\ge 0.22$ 則同步提升高蛋白權重）。
   * **太油（`too_oily`, $r = -0.6$）**：重口味權重 $w.\text{savory} \leftarrow \max(0.1, w.\text{savory} - 0.15)$，清淡權重 $w.\text{light} \leftarrow \min(1.0, w.\text{light} + 0.12)$。
   * **跳過（`skip`, $r = -0.2$）**：整體權重微幅衰減 $\text{clip}(0.95 \cdot w, 0.1, 1.0)$，並將轉移序列 $(s_t, a, r, s_{t+1}, f)$ 寫入互動記憶 $D$ 供離線 Contextual Bandit 擬合。
10. **十項量化評估指標與五組對照基準（10 KPIs & 5 Baselines）**：以 CVR（目標 0%）、AVR（目標 0%）、數值接地率（最終輸出 100%）、意圖正確率（$\ge 90\%$）、熱量 MAE（$\le 180\text{ kcal}$）、延遲（離線 $\le 50\text{ ms}$、Gemini P95 $\le 5\text{ s}$）、降級率、位置一致性、偏好收斂步數（$\le 5$ 次）與 SUS（$\ge 68$）進行全面量化驗證。

---

## 五、研究主題之預期貢獻與學術價值（Expected Contributions & Academic Value）

### 1. 學術理論價值（Academic & Methodological Value）
* **從「軟性獎勵權衡」邁向「硬約束優先（Hard-Constraint-First）」的健康推薦範式轉移**：突破過往多目標強化學習健康推薦（如 MORL-A2C [2]、MOPI-HFRS [1]）將臨床安全當作可與準確率交換之軟性目標的限制，證明透過前置決定論閘門可在維持個人化排序的同時達成 **0% 臨床與過敏原違規率**。
* **可驗證接地生成（Verifiable Grounded Generation）之防幻覺機制**：針對醫療飲食場域中最危險的「數值幻覺」（如便當鈉含量 CV 20–70% [5]、腎病鉀超標 [30]），提出結合 JSON 結構化注入、輸出後數值子集檢驗（Post-generation Numerical Subset Verification）與確定性模板回退的閉環機制，確保最終輸出數值接地率達 **100%**。
* **感知誤差感知（Error-Aware Sensing）與抗偏誤人機閉環**：將穿戴式生理訊號的不確定性（MAPE > 30% [10]）與 LLM 零樣本排序的位置偏誤 [27] 正式納入代理人數學模型中，提供具高度可解釋性與推理軌跡（Reasoning Trace）的跨領域 AIoT 代理人架構。

### 2. 臨床與公共衛生實務貢獻（Clinical & Public Health Impact）
* **填補診間外 70% 日常飲食決策的安全缺口**：回應全球每日 4,000 萬人向 AI 諮詢健康問題 [35] 及 2025 溴化鈉中毒案 [33]、2026《Nature Medicine》RCT [37] 揭露的臨床風險，提供一般民眾與亞健康族群在日常點餐當下安全可靠的決策支援。
* **直接賦能國家健康政策落地**：將食藥署 2026「紅黃綠」營養標示 [40] 與慢性腎臟病防治建言書 [42] 的靜態衛教指標，轉化為結合個人健檢數據與即時位置的主動式外食防護網。

---

## 六、課堂三階段指定產出：300 字中文摘要 & IEEE Systems Journal Abstract

### 1. 課堂第二段收尾：擷取重要文字 重新產生 300 字中文摘要

> **摘要**——外食已成為國人日常飲食型態，惟國民營養調查顯示逾 63% 成年人每日鈉攝取超標，台灣慢性腎臟病盛行率達 11.9% 但自我認知率僅 3.5%。同時，大型語言模型（LLM）已成為每日逾四千萬人取得健康資訊的新入口，然既有研究與臨床案例指出，通用 LLM 普遍低估外食便當鈉含量（變異係數達 20%–70%）、生成之腎病飲食常有鉀超標風險，且一般民眾使用純 LLM 評估健康狀況之表現並未優於對照組。為此，本研究提出 NutriAgent，一套結合穿戴式物聯網、決定論臨床營養規則與 Google Gemini 之接地式對話外食決策代理人。系統採「規則負責安全與數值，LLM 負責理解與表達」之架構分工：透過跨感測器合理性檢查與 0.65 阻尼係數計算動態熱量預算，並將涵蓋五大慢性病與食藥署 11 類過敏原之硬約束閘門置於 LLM 之前；LLM 僅對安全池進行抗位置偏誤之混合排序與回覆生成，所有輸出數值須經資料庫逐字驗證，不一致或逾時即自動降級為確定性離線引擎。初步測試顯示，系統臨床與過敏原違規率均為 0.0%，熱量平均絕對誤差為 114.48 kcal，成功實現安全由架構保證之智慧外食決策。（448 字）
>
> **關鍵詞**—大型語言模型、對話式代理人、健康感知推薦系統、臨床決策支援、穿戴式物聯網、接地生成。

---

### 2. 課堂第三段：IEEE Transactions / IEEE Systems Journal Style Abstract

**Title—NutriAgent: A Grounded Conversational LLM Agent for Real-Time Dietary Decisions Integrating Wearable IoT and Clinical Nutrition Rules**

**Abstract—Eating out has become a dominant daily dietary pattern, yet over 63% of adults in Taiwan exceed recommended sodium intake, while chronic kidney disease affects 11.9% of the population with an awareness rate of only 3.5%. Meanwhile, large language models (LLMs) have emerged as a primary gateway for public health inquiries; however, their dietary recommendations suffer from severe numerical instability and insufficient clinical safety guarantees. Empirical studies and clinical reports demonstrate that general-purpose LLMs systematically underestimate meal-box sodium content with coefficients of variation reaching 20%–70%, generate renal diets exceeding safe potassium limits, and fail to improve lay users' medical decision-making in randomized controlled trials. To address these limitations, this paper proposes NutriAgent, a grounded conversational agent for real-time dining-out decisions that synergizes wearable Internet of Things (IoT) sensing, deterministic clinical nutrition rules, and LLMs. NutriAgent enforces a strict architectural decoupling principle: deterministic rules govern clinical safety and numerical truth, whereas the LLM handles intent understanding and natural language expression. Specifically, an error-aware IoT module computes dynamic meal calorie budgets using cross-sensor plausibility checks and a 0.65 activity damping factor. Prior to any LLM invocation, a hard-constraint gate enforces medical nutrition therapy guidelines across five chronic conditions and 11 mandatory food allergen categories without relaxation. Subsequently, a hybrid ranker combines local flavor preferences with de-biased LLM semantic scores over the safe candidate pool. All numerical values in LLM-generated responses are verified verbatim against database records, automatically falling back to deterministic templates upon mismatch or API timeout. Preliminary functional evaluations demonstrate 0.0% clinical and allergen violation rates, a calorie mean absolute error of 114.48 kcal, and sub-millisecond offline fallback latency.**

***Index Terms*—Large language models, conversational agents, health-aware recommender systems, clinical decision support, wearable IoT, grounded generation.**

---

## 七、外部補充研究報告與新聞案例對照表（Perplexity 2025–2026 證據庫，編號接續簡報 [33] 起）

| 文獻編號 | 主題與關鍵數據 | 一手來源／出處 | 查證連結 |
| :---: | :--- | :--- | :--- |
| **[33]** | **ChatGPT 無鹽飲食建議致溴化鈉中毒住院案**（60 歲男性聽信 AI 以溴化鈉代鹽三個月中毒） | *Annals of Internal Medicine: Clinical Cases* (2025/8) | https://www.foxnews.com/health/chatgpt-dietary-advice-sends-man-hospital-dangerous-chemical-poisoning |
| **[34]** | **AI 聊天機器人誤用列為 2026 頭號醫療科技危害** | *ECRI Top 10 Health Technology Hazards 2026* | https://home.ecri.org/blogs/ecri-news/examining-the-greatest-health-technology-threats-of-2026 |
| **[35]** | **每日逾 4,000 萬人用 ChatGPT 問健康、70% 在診間外** | OpenAI Report, *"AI as a Healthcare Ally"* (2026/1) | https://www.medicaleconomics.com/view/40-million-people-now-use-chatgpt-daily-for-health-questions-openai-report-finds |
| **[36]** | **ChatGPT Health 發布（串接病歷與健康 App）** | OpenAI Announcement (2026/1/7) | https://www.medicaleconomics.com/view/openai-launches-chatgpt-health-directly-linking-patient-portals-to-the-ai-chatbot |
| **[37]** | **1,298 人臨床試驗：民眾用 LLM 查病況表現未優於對照組**（找病況 $<34.5\%$、處置 $<44.2\%$） | Bean et al., *Nature Medicine*, 32(2):609–615 (2026) | https://pmc.ncbi.nlm.nih.gov/articles/PMC12920132/ |
| **[38]** | **Google Health Coach 發布（Gemini 驅動整合穿戴與營養）** | Google / TechCrunch (2026/5) | https://techcrunch.com/2026/05/07/googles-9-99-per-month-ai-health-coach-launches-may-19/ |
| **[39]** | **FoodGuardBench / FoodGuard-4B（飲食安全防護基準）** | Luo et al., *arXiv:2604.01444* (2026/4) | https://arxiv.org/html/2604.01444v1 |
| **[40]** | **逾 63% 國人鈉攝取超標 & 食藥署「紅黃綠」營養標示分級** | 衛福部國健署國民營養調查；食藥署 (2026/4) | https://health.tvbs.com.tw/nutrition/362259 |
| **[41]** | **國健署 2026「營養及健康飲食促進方案」結合數位科技** | 衛福部國民健康署 (2026) | https://health.ltn.com.tw/article/breakingnews/5404478 |
| **[42]** | **台灣 CKD 盛行率 11.9%、認知率僅 3.5% & 降低新發透析 KPI** | 《2026 國家慢性腎臟病防治政策建言書》 | https://health.ltn.com.tw/article/breakingnews/5459312 |
| **[43]** | **台灣末期腎病發生率與每年約 1.2 萬人新進入透析** | 厚生基金會引述腎病統計資料 | https://udn.com/news/story/7266/9542803 |
| **[44]** | **腎病健保醫療費用與新增透析患者以 40–64 歲佔比最高** | 健保署；《2024 台灣腎病年報》 | https://www.nownews.com/news/6890615 |
