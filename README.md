# Group 5 Wi-Fi 房間感知實驗
這是基於開源 RuView 的 Wi-Fi 房間感知實驗，對象是希望了解房間活動狀態的家人。
怎麼跑：啟動上游 sensing-server，在本倉庫執行 `python -m http.server 8099`，開啟 `src/node_monitor.html` 或 `src/wiflow_browser.html`；詳見下方。
技術：ESP32 CSI、MediaPipe Pose、TensorFlow.js；需要已配置的硬件和資料。
分工：王子源開發及隊長、陳洛駿文件與壁報、馮曉琛發表與展位。
現況：已整理節點監視和 WiFlow 介面；本次實機與模型推論尚未驗證，節點監視器沒有靜止提醒。

## 如何運行

此倉庫是小組介面檔案，不包含完整的 RuView 感測伺服器或已訓練模型。需要 Windows、Python、Rust、可上網的瀏覽器，以及已配置並串流 CSI 的 ESP32 硬件。使用者須自行取得並配置設備；以下未經另一台電腦驗證。

1. 取得上游程式及子模組，在獨立資料夾執行：

   ```powershell
   git clone --recurse-submodules https://github.com/ruvnet/RuView.git ruview-upstream
   cd ruview-upstream
   git checkout d42c5581f34fe8e50b053971d387fb8c56a3baf4
   git submodule update --init --recursive
   cd v2
   cargo build -p wifi-densepose-sensing-server
   .\target\debug\sensing-server.exe --ws-port 8765 --udp-port 5005
   ```

2. 在另一個終端機進入本倉庫根目錄，執行 `python -m http.server 8099`。打開 `http://localhost:8099/src/node_monitor.html` 查看節點；`http://localhost:8099/src/wiflow_browser.html` 是校準、採集、訓練及推論介面。
3. 節點介面目前固定顯示 ID 1–6；其他編號不顯示。WiFlow 頁面需有即時 ESP32 CSI；採集標註時需要攝影機、瀏覽器許可及網路載入套件。先空房校準、採集配對資料、訓練模型，再執行推論。沒有資料或模型時不會產生已驗證的姿態結果。

## 目前狀態與限制

- 九月簡報描述了六個 ESP32 的實驗環境和初步骨架展示，並指出偏差及抖動。本次尚未取得可核對的實機錄屏、模型結果或量化準確度。
- `node_monitor.html` 顯示指定節點狀態；它本身不判斷房間是否有人，也沒有靜止超時提醒。有人／靜止判斷、提醒、跨房及穿牆效果均需實測。
- WiFlow 程式包含 MediaPipe Pose 攝影機標註和 TensorFlow.js 多層感知器訓練與推論流程；本次是否成功訓練及推論尚未驗證。不能將程式存在當作實際 AI 演示。
- 待補：60 秒內的三步錄屏、兩份外人測試、換機記錄和 10/14 任務單照片。不要上傳 Wi-Fi 密碼、原始 CSI、個人影像或模型資料。

## 開源來源與分工

上游：[ruvnet/RuView](https://github.com/ruvnet/RuView)，本機檢視的提交 `d42c5581f34fe8e50b053971d387fb8c56a3baf4`。上游授權見 `UPSTREAM-LICENSE`。本倉庫包含本機 WiFlow 頁面的修改版本及節點監視頁；其餘伺服器與韌體依賴上游。上游功能不代表小組原創；具體個人貢獻仍需組員核對。

分工：王子源負責開發及隊長聯繫；陳洛駿負責文件與壁報；馮曉琛負責發表與展位。
