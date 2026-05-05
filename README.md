# student-risk-system
# 學生學習風險評估系統

## 系統說明
本系統提供三種學生學習風險分析方式：
- **風險評估（規則式）**：依據成績、出缺席、獎懲、補助等規則評分（總分 1～12）
- **學習成效預測（AI 模型）**：使用 XGBoost 機器學習模型預測學生最終學習成效落於班級後 50%（含休退學者）的機率（49 特徵；CV ROC-AUC 84.7%、F1 77.0%）
- **休退學預警（AI 模型）**：使用 XGBoost 早期預警模型，僅以前 2 學期 30 特徵預測學生未來休學或退學的機率，提供 S2 結束前的早期介入名單（CV ROC-AUC 77.7%、Recall 56.5%；已採用 scale_pos_weight 處理類別不平衡）

## 使用方式
1. 開啟網站：https://nick111432009-beep.github.io/student-risk-system/
2. 切換到想用的分頁
3. 上傳學生 Excel 資料（含學務及成績資料）
4. 選擇入學學年，查看風險名單
5. 可下載 Excel 名單交付各處室

## 模型檔案
- `performance_model.onnx` / `performance_model_features.json`：學習成效模型（49 特徵）
- `dropout_model.onnx` / `dropout_model_features.json`：休退學預警模型（30 特徵，前 2 學期）
- `xgboost_model.onnx` / `model_features.json`：舊版單一模型（保留作為對照，目前已不被網頁呼叫）

## 注意事項
所有資料在使用者電腦本機處理，不會上傳至任何伺服器。
