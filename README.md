# 隨身翻譯（iPhone 網頁型 App）

語音／文字／拍照翻譯、面對面對話翻譯，以及會議錄音的即時逐字稿、即時翻譯和會議記錄整理。

裝好後從 iPhone 主畫面打開，用起來和一般 App 一樣，不需要上架 App Store。

---

## 檔案內容

| 檔案 | 用途 |
|---|---|
| `index.html` | App 本體（所有畫面與功能都在這一個檔案） |
| `manifest.webmanifest` | 讓 iPhone 能「加入主畫面」的設定 |
| `sw.js` | 快取 App，讓它開啟更快 |
| `icons/` | App 圖示 |

---

## 第 1 步：申請 Claude API 金鑰

1. 到 https://console.anthropic.com 註冊並登入。
2. 在 **Billing** 儲值。
3. 在 **Limits** 設定每月花費上限，避免意外超支。
4. 到 **API Keys** 按「Create Key」，複製產生的金鑰（`sk-ant-` 開頭）。

> 金鑰不要寫進程式碼或上傳到 GitHub。它只需要在第 4 步輸入到手機的 App 設定裡。

---

## 第 2 步：放到 GitHub Pages（免費、自帶 https）

麥克風只能在 https 網址下使用，GitHub Pages 剛好提供。

**方法 A：用網頁上傳（不需要指令）**

1. 登入 https://github.com ，右上角「+」→ **New repository**。
2. Repository name 輸入 `pocket-translate`，選 **Public**，按 **Create repository**。
3. 在新頁面點 **uploading an existing file**，把這個資料夾裡的所有檔案（含 `icons` 資料夾）拖進去，按 **Commit changes**。
4. 到 **Settings** → 左側 **Pages** → Source 選 **Deploy from a branch**，Branch 選 `main`、資料夾選 `/ (root)`，按 **Save**。
5. 等 1～2 分鐘，網址會是：`https://你的帳號.github.io/pocket-translate/`

**方法 B：用 Git 指令（PowerShell）**

```powershell
cd 這個資料夾的路徑
git init
git add .
git commit -m "隨身翻譯 v1"
git branch -M main
git remote add origin https://github.com/你的帳號/pocket-translate.git
git push -u origin main
```

接著照方法 A 的第 4 步開啟 Pages。

---

## 第 3 步：安裝到 iPhone

1. 用 **Safari**（不是 Chrome）打開上面的網址。
2. 按下方的「分享」按鈕 →「加入主畫面」→「新增」。
3. 主畫面會出現「隨身翻譯」圖示。

---

## 第 4 步：第一次使用

1. 打開 App，按右上角齒輪，貼上 API 金鑰，按「儲存並測試」。
2. 第一次按麥克風時，iPhone 會詢問是否允許使用麥克風和語音辨識，請選「允許」。
3. 如果按麥克風沒反應：
   - 「設定」>「一般」>「鍵盤」→ 開啟「聽寫」
   - 「設定」>「Safari」>「麥克風」→ 選「允許」
   - 從主畫面圖示開啟時語音辨識無法使用的話，改用 Safari 直接開網址試試（部分 iOS 版本對主畫面 App 的語音辨識支援較差）。

---

## 功能說明

**翻譯**
- 按麥克風說話，說完自動翻譯。選好「原文」語言，辨識會比「自動偵測」準。
- 「拍照翻譯」可以拍菜單、看板、文件、簡報。
- 點譯文可以全螢幕放大給對方看；「對方回覆」會反轉翻譯方向。

**對話**
- 設定「我說」和「對方說」的語言，按對應的大按鈕說話，會翻譯並自動朗讀。
- 右上角「商務」語氣會讓日文使用敬語。

**會議**
- 選「會議中說的語言」、「即時翻譯成」、「會議記錄語言」，按「開始錄音」。
- 說的話會即時變成逐字稿，每句下方顯示譯文。
- 結束後按「整理會議記錄」，產生摘要、討論重點、決議、待辦（負責人／期限）、待確認問題。
- 可以分享、複製、下載 .txt。最近 20 場會議存在手機裡。
- App 意外關掉時，下次打開會自動找回逐字稿。

---

## 使用限制（請先知道）

- **一次只能辨識一種語言。** 會議中如果中日文混著說，請選主要的語言；另一種語言的部分可能辨識不準。
- **螢幕要保持開啟、App 要在前景。** iPhone 鎖定螢幕或切到其他 App 時，網頁會被暫停，辨識會中斷（回到 App 會自動繼續）。長會議建議接上充電。
- **「同時保存錄音檔」** 在部分 iPhone 上會讓即時辨識變不穩，第一次請先用短時間測試。
- 語音辨識由 iPhone 內建的 Apple 聽寫服務處理；翻譯和整理由 Claude API 處理，費用從你的 API 餘額扣除。
- API 金鑰存在這支手機的瀏覽器裡。手機若給別人使用，對方也能用你的額度，所以請在 console 設好花費上限。

---

## 之後修改程式

1. 修改 `index.html`。
2. 把 `sw.js` 第一行附近的 `VERSION = "pt-v1"` 改成 `pt-v2`（每次改都加 1），手機才會抓到新版。
3. 重新上傳到 GitHub。手機上關掉 App 再打開即可更新。

模型名稱若之後改版，可在 App 設定的「進階」直接修改，不必改程式。
