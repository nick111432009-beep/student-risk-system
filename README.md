# Student Risk System(v9 部署版)

## 雙模型預警系統(v9-fs,2026-06)

### 模型一:休退學預警
- 視窗:**S1–S2**(大一全年)→ 預測**入學兩年內**休退學(固定觀察窗標籤)
- 特徵數:42(含 cohort 化同儕相對位階)
- 門檻:**預警 ≥0.25(OOF F2)/ 觀察 ≥0.15(Youden's J)**
- Holdout(112 學年):AUC 89.7% / PR-AUC 78.3% / Recall 71.8% / Precision 68.3%
- 十種子平均 F1 63.6%±2.9pp;滾動回測跨屆 F1 64.4%±4.4pp

### 模型二:學習成效預測
- 視窗:**S1–S4** → 預測 **S5–S6 時點班級百分比後 20%**(真未來預測)
- 特徵數:73
- 門檻:**預警 ≥0.16(OOF F2,恰為 Youden 切點 → 實質兩級制)**
- Holdout(111 學年):AUC 97.2% / PR-AUC 88.9% / Recall 96.1% / Precision 64.2%
- 十種子平均 F1 76.5%±0.9pp

## v9 升級重點
- 標籤固定觀察窗(各屆答案定義一致、真未來預測)
- F2 召回導向部署門檻(漏接成本 >> 誤抓成本)+ Youden/量能觀察線
- 前端特徵引擎重寫:**由 features.json 特徵清單驅動**,cohort 統計與
  LabelEncoder 於瀏覽器內由上傳檔即時計算 — 模型更新只需替換 onnx+json
- 已通過「前端 JS vs notebook 特徵工程」逐格交叉驗證(max diff < 1e-6)
  與「ONNX vs sklearn」推論一致性驗證(max diff < 1e-3)

## 檔案說明
- `dropout_model.onnx` — 模型一(42 特徵)
- `performance_model.onnx` — 模型二(73 特徵)
- `*_features.json` — 特徵清單(=向量順序)+ 門檻 + 版本(與訓練同源匯出)
- `index.html` — 前端 UI(本機推論,資料不上傳)

## 注意
- 上傳檔需為全校學籍 Excel(同訓練格式):cohort 相對特徵與類別編碼
  需由整批資料計算,單一學生無法獨立推論
- 模型分數非機率,風險分級依據見 v9 技術報告 §10.3
