# 高息ETF配息雷達｜臺灣高股息 ETF 追蹤清單

## 🔔 人工確認紀錄（排程保活）

GitHub 規定：公開 repository 連續 60 天沒有活動，自動排程會被停用。
機器人的自動提交與手動 Run workflow 都不保證算數，
所以**每 6 週左右**請在這裡加一列並 Commit。

| 日期 | 網站資料時間 | 收盤價參考日期 | 檔數 | 狀態 | 備註 |
| --- | --- | --- | --- | --- | --- |
| 2026-10-20 | 2026-10-20 03:17 | 2026-10-19 | 35 | ✅ 正常 | 建立保活機制 |

### 📅 下一次建議加一列：**2026-12-01**

（每次加完一列，把這個日期往後推 6 週再改掉。6 週是刻意留的緩衝，不要壓到 60 天。）

**加一列的步驟**：

1. 打開網站 <https://stockgujon.github.io/etf-radar/>，看三個地方：
   - 右上角的**資料時間**
   - 上方的**顯示 N 檔**
   - 任何一列收盤股價下面的**參考日期**
2. 回到 GitHub，打開 README.md，按右上角鉛筆圖示 ✏️
3. 在表格最後一列下面，照格式加一行（今天日期＋上面抄到的三個數字＋狀態）
4. **順手把上面那個「下一次建議加一列」的日期往後推 6 週**
5. 按右上角綠色「Commit changes」→ 再按一次確認

**順手檢查的重點**（有異常就在備註寫一下）：

- **資料時間**停在好幾天前 → 排程可能出事了，去 Actions 看看
- **檔數**從 35 變成差很多的數字 → 證交所的分類標籤可能改了
- **收盤價參考日期**比資料時間舊很多（非連假期間）→ 收盤行情那段有問題

**萬一還是被停用了**：GitHub 會寄信通知。到 **Actions** → 左邊選 **更新 ETF 資料**
→ 按 **Enable workflow** 即可恢復，資料不會遺失。

---

只收錄臺灣掛牌、投資國內股票，且由證交所列為「高股息」或「高息低波動」因子的 ETF。

- 網頁本身：`index.html`（純靜態，放在 GitHub Pages）
- 資料：`data.json`，由 GitHub Actions 每天自動去證交所抓好寫回來
- 抓資料的程式：`scripts/fetch.mjs`
- 排程設定：`.github/workflows/update.yml`

---

## 架設步驟（全部在瀏覽器裡完成，不用裝任何軟體）

### 步驟 1　註冊 GitHub

到 <https://github.com/signup> 用 email 註冊，免費。

---

### 步驟 2　建立一個倉庫（repository）

1. 右上角 **＋** → **New repository**
2. **Repository name** 填 `etf-radar`（這會變成網址的一部分）
3. 選 **Public**（公開。GitHub Pages 免費版需要公開）
4. 按 **Create repository**

---

### 步驟 3　上傳檔案

在新倉庫的畫面上按 **uploading an existing file**（或 **Add file → Upload files**）。

把解壓縮後資料夾裡的東西**全部**拖進去：

```
index.html
data.json          ← 可以先不上傳，第一次跑完 Actions 會自動產生
README.md
scripts/fetch.mjs
.github/workflows/update.yml
```

拖完後，下方按 **Commit changes**。

> **如果 `.github` 資料夾拖不上去**（有些瀏覽器會跳過開頭是點的資料夾）：
> 按 **Add file → Create new file**，在檔名欄位直接輸入
> `.github/workflows/update.yml`
> （打斜線它會自動變成資料夾），然後把 `update.yml` 的內容整個貼進去，按 Commit。

---

### 步驟 4　打開 GitHub Pages

1. 倉庫上方 **Settings** → 左邊選單 **Pages**
2. **Source** 選 **Deploy from a branch**
3. **Branch** 選 `main`，資料夾選 `/ (root)`，按 **Save**

等 1～2 分鐘，這一頁上方就會出現你的網址：

```
https://你的帳號.github.io/etf-radar/
```

---

### 步驟 5　跑第一次資料抓取

1. 倉庫上方 **Actions** 分頁
2. 如果出現綠色的 *I understand my workflows, go ahead and enable them*，按下去
3. 左邊點 **更新 ETF 資料** → 右邊 **Run workflow** → 再按一次綠色的 **Run workflow**
4. 等 1 分鐘左右，出現綠色勾勾就成功了

打開步驟 4 的網址，資料就出來了。**加入書籤**，或在手機瀏覽器「加到主畫面」，用起來就像一個 App。

---

## 資料多久更新一次？

GitHub 會在**台北時間每天 09:05、14:05、16:05、19:05、21:05** 自動去證交所抓一次，
抓完存回 `data.json`。其中 **16:05 與 21:05 是備援** —— GitHub 的排程常常延遲十幾分鐘、
偶爾整次跳過或分配不到機器，多兩班確保當天一定補得上。

網頁這端：

- 每次打開網頁就讀最新的 `data.json`
- 切回這個分頁、且超過 10 分鐘沒更新時，會自動再讀一次
- 頁面右上角的 **↻ 重新同步** 可以隨時手動讀一次
- 網頁開著不關的話，每天 19:00 也會自動讀一次

你完全不用管它。

> 台股收盤價一天只會有一筆，所以「排程抓好放著」和「每次打開才抓」看到的資料是一樣新的。

---

## 常見狀況

**網頁顯示「同步異常」**
多半是 `data.json` 還沒產生。回到 **Actions** 分頁手動跑一次（步驟 5），再回網頁按「再試一次」。

**Actions 跑出紅色叉叉**
點進去看錯誤訊息。通常是證交所網站改版或當下維護。把錯誤訊息告訴我，我改 `scripts/fetch.mjs`，你再更新那一個檔案就好。

**GitHub 寄信說排程被停用**
如果倉庫連續 60 天沒有人為操作，GitHub 會自動關掉排程。信裡會有一個 **Enable workflow** 的連結，點一下就恢復了。

**想改文字或顏色**
直接在 GitHub 上編輯 `index.html`（點檔案 → 鉛筆圖示 → 改 → Commit），存檔後網站幾十秒內就會更新，網址不變。

---

## 資料來源

- [證交所 ETF e添富商品篩選器](https://www.twse.com.tw/zh/ETFortune-institute/products)
- [證交所 ETF 配息資訊](https://www.twse.com.tw/zh/ETFortune-institute/dividendList)
- [證交所上市個股日成交資訊](https://www.twse.com.tw/zh/trading/historical/mi-index.html)

## 免責聲明

投資一定有風險，配息金額與頻率均可能變動。年化殖利率為「單次配息 × 每年配息次數 ÷ 現價」的簡單換算，不代表基金報酬率，也不是配息保證。本頁僅為公開資料整理，不構成投資建議。
