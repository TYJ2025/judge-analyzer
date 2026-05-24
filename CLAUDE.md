# CLAUDE.md — Judge Analyzer 法官行為分析器

> 給下一個 Claude session 的 30 秒上手文件。

---

## 1. 專案是什麼

單檔 HTML 工具，做台灣法官的判決行為剖析（judicial behavioral profiling）。輸入 1 份或多份裁判全文，輸出 5 維度視覺化儀表板：裁判傾向、量刑/金額模式、法律哲學、語言風格、異常案件偵測，另加「假設案件預測」。前端純 vanilla JS + Chart.js 3.9.1（CDN），無後端、無 build step。

---

## 2. 檔案結構

```
Judge Analyzer/
├── index.html            # 全部都在這 1 檔（CSS + JS 內嵌, ~55 KB / 1400+ 行）
├── CLAUDE.md             # 本檔
├── README.md             # 公開說明
├── .gitignore            # .DS_Store / *.swp 等
└── .git/                 # 連到 github.com:TYJ2025/judge-analyzer (HTTPS)
```

（檔名為 `index.html` 以讓 GH Pages 的 `/` 直接服務。原 `judge-analyzer.html` 在 2026-05-24 改名）

---

## 3. 怎麼用

1. 線上：https://tyj2025.github.io/judge-analyzer/（**public** GitHub Pages，免 setup 即可分享）。
2. 本機：用瀏覽器開 `index.html`（雙擊即可，不用 server）。
2. 三種輸入：
   - 貼裁判全文到 textarea（支援多份，自動 parse）
   - 上傳 JSON / CSV / XLSX（司法院 judgment.judicial.gov.tw 格式）
   - 點「載入範例」用 demo data
3. （可選）填 Claude 或 OpenAI API Key → 跑 `enrichWithAPI()` 做 AI 摘要。
4. 按「分析」→ 下方出現 5 個 Chart.js canvas + 案件詳情 tabs。

關鍵 function（找位置時用）：`parseRawJudgment`、`extractSentences`、`extractAwards`、`convertChineseNumber`（壹貳參…→ 阿拉伯數字）、`detectOutliers`、`createCharts`、`calculatePrediction`。

---

## 4. 部署 / GitHub

- Repo：https://github.com/TYJ2025/judge-analyzer（**public**，HTTPS remote）
- **GH Pages 已開**（main / root，2026-05-24 啟用）。push 後約 60 秒線上版自動更新。
- 改完 `index.html` 要手動 `git add / commit / push`（沒有 auto-push watcher）。要驗證部署：
  ```bash
  gh run list --repo TYJ2025/judge-analyzer --limit 3
  curl -sI "https://tyj2025.github.io/judge-analyzer/" | head -3
  ```

---

## 5. 紅線（DO NOT）

- ❌ **不要把 API Key 寫進 HTML 後 commit**。目前設計是使用者每次貼進 `<input id="apiKey">`，純 client-side 用 `fetch()` 直接打 Anthropic / OpenAI endpoint，**key 不會上傳到任何後端**（也沒有後端）。若要改成預填，務必先把 repo 轉 private。
- ❌ **不要假設 `fetch('https://api.anthropic.com/...')` 在所有瀏覽器都能通**——這是 client-side 直接呼叫 Anthropic API，吃 CORS。Anthropic 目前對 browser direct call 有限制（要 `anthropic-dangerous-direct-browser-access` header 或會被擋）。改 AI 整合邏輯前先在 console 看實際 response，不要只信 silent catch（`enrichWithAPI` 把錯誤吞掉只 `console.log`）。
- ❌ **Chart.js 版本鎖 3.9.1**（CDN）。升到 4.x 會 breaking（scale config、legend API 都改了）。要升先全 chart 重測。
- ❌ **不要搬進 iCloud Drive 路徑**（共通紅線，見 `~/CLAUDE.md`）。

---

## 6. 已知限制（從 code 看到的）

- `parseRawJudgment` 用 regex 抓欄位，遇到非典型格式（例如自製摘要、舊版判決排版）會 silently 失敗——caseCount 會偏低。
- `enrichWithAPI` 沒有 rate limit / retry，多案會連續打 N 次 API。
- `convertChineseNumber` 只覆蓋大寫數字，沒處理「萬」「億」級組合的所有 edge case；金額抽取以「新臺幣」為錨點，其他寫法（NT$、元）可能漏。
- 沒有任何 test。

---

_Last updated: 2026-05-24 by Claude（從 codebase 反推）_
