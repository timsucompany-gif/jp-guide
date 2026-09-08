# 2026 日本退稅新制懶人包

一頁式靜態網站，說明日本 2026/11/1 起實施的「リファンド方式」（先付含稅價、出境海關確認後退款）新制。
純 HTML / CSS / JS，零外部相依，可直接放上 GitHub Pages。
單一淺色版本（無深色模式）；桌機為雙欄編輯式排版，手機自動收合成單欄。

## 資料夾結構

```
jp-guide/                      ← repo 根目錄＝ /jp-guide/
├── index.html                 2026 日本退稅新制懶人包（首篇文章）
├── banner.jpg                 上篇文章的主視覺
├── privacy.html               隱私權政策
│
├── assets/                    全站共用資產
│   ├── favicon.svg / .ico / favicon-32.png / apple-touch-icon.png
│   └── og.png                 備用社群縮圖
│
├── visit-japan-web/           每篇新文章 = 一個資料夾
│   ├── index.html
│   └── banner.jpg             該篇專用的圖片放同層
│
├── ads.txt  robots.txt  sitemap.xml
└── README.md
```

### 新增文章的規則

1. 建一個**英文小寫、用連字號**的資料夾，例如 `jr-pass/`、`japan-esim/`
   資料夾名稱就是網址：`/jp-guide/jr-pass/`
2. 文章寫在該資料夾的 `index.html`
3. **該篇專用的圖片放同一個資料夾**（`banner.jpg` 等），不要丟到根目錄
4. 共用的圖示、字型一律放 `assets/`
5. 完成後三件事：
   - `sitemap.xml` 新增一筆 `<url>`
   - 入口頁（`timsucompany-gif.github.io` repo）新增一張文章卡
   - 跟既有文章互相加連結（對 SEO 幫助很大）

> CSS 目前是每頁內嵌的。這是刻意的選擇 —— 內容站的訪客多半從搜尋結果直接進單一頁面就離開，
> 內嵌可省掉一次外部請求，首屏更快。等文章多到十篇以上再考慮抽成共用 CSS 檔。

## 主視覺圖片

每篇文章最上方的橫幅放在該篇資料夾裡，檔名統一 `banner.jpg`，尺寸 1408×768。

退稅文的原始檔 957KB 已壓縮成 265KB（progressive JPEG，品質 82），對 LCP 分數差很多；
原檔備份為 `banner-original.jpg`（已加入 .gitignore，不會上傳）。

要換圖的話，存成同名 `banner.jpg` 蓋掉即可，並記得同步改 `index.html` 裡
`<img src="banner.jpg" width="1408" height="768">` 的尺寸。
**圖片載入失敗時整個區塊會自動移除**，不會出現破圖。

banner 同時也是該篇的 `og:image`（社群分享縮圖），所以換圖時記得一併確認 `<meta property="og:image">` 的網址與尺寸。

`assets/og.png`（1200×630 的純文字卡）是備用縮圖，目前沒有被引用。

## 部署到 GitHub Pages（方案 A：根目錄）

repo 必須命名為 `timsucompany-gif.github.io`，網站才會在根網址，`ads.txt` 也才讀得到。

1. 到 https://github.com/new 建立 repo，名稱填 `timsucompany-gif.github.io`，**Public**，不要勾任何初始化檔案。
2. 在本資料夾執行：

   ```
   git init
   git add .
   git commit -m "日本退稅新制懶人包"
   git branch -M main
   git remote add origin https://github.com/timsucompany-gif/jp-guide.git
   git push -u origin main
   ```

3. Repo → Settings → Pages，確認 Source 是 `Deploy from a branch`、Branch `main` / `(root)`。
4. 等 1～3 分鐘，網址就是 `https://timsucompany-gif.github.io/`。

之後要更新內容：`git add . && git commit -m "更新" && git push`


## 上線前必做（SEO 用）

網址佔位符已替換完成，目前指向 `https://timsucompany-gif.github.io/`。
若日後改用自訂網域，把三個檔案裡的舊網址換掉即可：

- `index.html`（canonical、hreflang、og:url、og:image、JSON-LD）
- `sitemap.xml`
- `robots.txt`

一行指令搞定（Git Bash）：

```
sed -i 's#https://timsucompany-gif.github.io#https://新網域#g' index.html sitemap.xml robots.txt
```

接著：

- 到 [Google Search Console](https://search.google.com/search-console) 驗證網站所有權，送出 `sitemap.xml`。
- 用 [複合式搜尋結果測試](https://search.google.com/test/rich-results) 檢查 Article / HowTo / FAQ 結構化資料。
- 用 [PageSpeed Insights](https://pagespeed.web.dev/) 跑一次分數。

## 網址與 repo 命名

**建議 repo 直接命名為 `timsucompany-gif.github.io`**，網站就會在 `https://timsucompany-gif.github.io/` 根目錄。
原因：AdSense 的 `ads.txt` 只認網域根目錄，放在子路徑（`/japan-tax-refund/ads.txt`）不會被讀到。

若之後接自訂網域，在本資料夾新增一個 `CNAME` 檔，內容只寫網域本身（例如 `jptaxrefund.com`），
再到 DNS 設定 A 記錄指向 GitHub Pages 的四組 IP，或用 CNAME 指向 `timsucompany-gif.github.io`。

## 接 Google AdSense

需要準備的 ID：

| 項目 | 格式 | 數量 | 哪裡拿 |
| --- | --- | --- | --- |
| 發布商 ID | `pub-` + 16 位數字 | 1 | AdSense →「帳戶」→「帳戶資訊」 |
| 廣告單元 ID | 10 位數字 | 3 | AdSense →「廣告」→「按廣告單元」→ 建立「多媒體廣告 / 回應式」 |

三個版位分別在：文章上方、文章中段、試算器前（搜尋 `ad-slot` 就找得到）。

設定步驟：

```
# 發布商 ID 已填入 6600085221791202
sed -i 's/AD_SLOT_1/第一個單元ID/;s/AD_SLOT_2/第二個單元ID/;s/AD_SLOT_3/第三個單元ID/' index.html
```

然後把 `index.html` `<head>` 裡「Google AdSense」那段的 `<!--` `-->` 拿掉即可。

> 版位已預留 `min-height:280px`，避免版面位移（CLS）影響 Core Web Vitals。
> AdSense 審核要求網站有實質原創內容與隱私權政策頁；若審核被退，補一頁 `privacy.html` 再送審。

## 看流量數據

`<head>` 裡已預留兩段，填好 ID 後把 `<!--` `-->` 拿掉：

- **Google Search Console** — 把 `VERIFICATION_CODE` 換成驗證碼，之後送出 `sitemap.xml`。看的是搜尋曝光次數、點擊率、關鍵字排名。
- **Google Analytics 4** — 把 `G-MEASUREMENT_ID` 換成評估 ID（`G-` 開頭）。看的是實際訪客數、來源、停留時間、哪一段最多人看。


## 資料來源

- [日本觀光廳 — 新制度（リファンド方式）旅客專頁](https://www.mlit.go.jp/kankocho/tax-free/page01_000001_00021.html)
- [日本觀光廳 — 旅客常見問題 Q&A](https://www.mlit.go.jp/kankocho/tax-free/page01_000001_00023.html)
- [日本國稅廳 — 輸出物品販賣場制度のリファンド方式への見直し](https://www.nta.go.jp/publication/pamph/shohi/menzei/201805/format/002.htm)
- [財務省・國稅廳・經產省・觀光廳 — 制度檢討資料 PDF](https://www.mlit.go.jp/kankocho/content/001858413.pdf)

內容整理於 2026-09-08。制度細節仍可能微調，請以官方公告為準。
