# 普生互動複習題卡：GitHub Pages 上線教學

這個資料夾已經是可發布的完整網站，不必安裝程式，也不用修改題目資料。

目前包含：

- **考古原題 212 題**：題幹、選項與選項順序逐字保留；原始 218 張卡中跨年份完全相同的 6 題已合併，並保留所有出處。
- **統整練習 292 題**：考古延伸、筆記重點與《普生筆記－中》新增重點；已排除與考古原題重複的題目。
- **英文單字 533 張**：完全依《普生筆記上_藍筆英文總整理.pdf》製作；正面只顯示英文，翻面顯示中文、功能聯想與來源頁。
- 深色手機版、星號練習、自動把客觀題錯題加星號、題目總覽、作答與答錯紀錄。
- 單字卡可按「待加強」自動加入星號，或按「已會」取消星號並進到下一張。
- 不登入時使用瀏覽器本機儲存；完成 Supabase 設定後，可用電子郵件＋密碼跨手機與電腦同步。

## 一、先發布到 GitHub Pages

1. 登入 [GitHub](https://github.com/)，右上角按 **＋ → New repository**。
2. Repository name 可輸入 `biology-review`。若使用 GitHub Free，請選 **Public**；Private repository 的 Pages 需要支援該功能的付費方案。再按 **Create repository**。
3. 在新 repository 頁面按 **uploading an existing file**。
4. 把本資料夾內的所有內容拖進上傳區；要拖入 `index.html`、`app.js`、`styles.css`、`data` 資料夾等內容，不要只拖最外層資料夾。
5. 頁面下方按 **Commit changes**。
6. 進入 repository 的 **Settings → Pages**。
7. 在 **Build and deployment**：Source 選 **Deploy from a branch**，Branch 選 **main**、資料夾選 **/(root)**，按 **Save**。
8. 等候約 1–5 分鐘，回到同一頁會看到網址，通常是：
   `https://你的GitHub帳號.github.io/biology-review/`

做到這裡，網站已能在手機使用；進度會保存在該手機的瀏覽器。接著的 Supabase 是「跨裝置同步」才需要。

> GitHub Pages 網站本身會公開在網路上，即使付費方案允許從 private repository 發布也是如此。登入功能只保護個人進度，不會把題目檔案變成私密內容。若考古題不適合公開，請先不要發布到 GitHub Pages。

## 二、啟用安全的帳號與雲端同步（建議）

單純用一組公開數字當登入編號，任何猜到編號的人都可能讀寫進度。因此本網站改用免費的 **Supabase 電子郵件＋密碼登入**，並用資料庫規則確保每個帳號只能讀寫自己的資料。

1. 到 [Supabase](https://supabase.com/) 登入，建立一個新 project。
2. 左側開啟 **SQL Editor → New query**。
3. 打開本資料夾的 `supabase-schema.sql`，複製全部內容到 SQL Editor，按 **Run**。
4. 在 Supabase 找到 **Project Settings / API**（新介面也可能在 **Connect**）：複製：
   - Project URL
   - Publishable key（若介面只有 anon key，也可使用 anon key；絕對不要放 service_role key）
5. 回到 GitHub repository，點開 `config.js`，按鉛筆圖示編輯，改成：

   ```js
   window.BIO_REVIEW_CONFIG = {
     supabaseUrl: "你的 Project URL",
     supabasePublishableKey: "你的 Publishable key"
   };
   ```

6. 按 **Commit changes**，等待 GitHub Pages 重新部署。
7. 在 Supabase 的 **Authentication → URL Configuration**：
   - Site URL 填入你的 GitHub Pages 網址。
   - Redirect URLs 也新增同一個網址，結尾建議保留 `/`。
8. 手機重新整理網站，按右上角 **帳號／同步 → 建立帳號**。
9. 若 Supabase 要求驗證信箱，先到信箱點驗證連結，再回網站登入。

完成後，每次作答、加星號、取消星號或切換題庫都會先存本機，再自動同步雲端。

## 三、之後更新題庫

若日後重新產生 `data/exam.json` 或 `data/practice.json`：

1. 在 GitHub 打開相同路徑的檔案。
2. 右上角選單可刪除舊檔，再用 **Add file → Upload files** 上傳新檔。
3. Commit 後 GitHub Pages 會自動重新部署；既有使用者進度仍會保留，題目以穩定 `id` 對應。

## 注意

- 不要直接雙擊 `index.html` 測試，瀏覽器可能禁止讀取 `data/*.json`。GitHub Pages 上線後即可正常使用。
- `config.js` 只能放 Supabase publishable/anon key；不要放 service role key 或任何私人密鑰。
- 清除瀏覽器網站資料會刪除未登入的本機進度；有登入並完成同步者可從雲端恢復。
