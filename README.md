# 手機號碼隨機產生器｜Supabase 共用版

這個版本使用 GitHub Pages + Supabase。

## 功能

- 一次預設產生 100 筆
- 不提供電信選擇
- 所有使用同一網站的人共用生成紀錄
- 資料庫永久記錄每一批生成的號碼
- 資料庫 UNIQUE 主鍵避免完整手機號碼重複
- 同時多人使用時，由資料庫 RPC 原子處理
- 可查看每一批歷史
- 可複製單批號碼
- 可下載本批 CSV
- 可下載完整歷史 CSV

## 設定步驟

### 1. 建立 Supabase Project

到 Supabase 建立一個免費 Project。

### 2. 執行 database.sql

把本專案的 `database.sql` 全部複製到 Supabase Dashboard → SQL Editor 執行。

### 3. 取得 Supabase API 資訊

Supabase Project Settings → API，取得：

- Project URL
- Publishable key / anon public key

不要使用 `service_role` key。

### 4. 修改 index.html

找到：

const SUPABASE_URL = "YOUR_SUPABASE_URL";
const SUPABASE_ANON_KEY = "YOUR_SUPABASE_ANON_KEY";

改成你的資料。

### 5. 上傳 GitHub

把 `index.html` 與 `database.sql`、`README.md` 上傳到 repository。
GitHub Pages 只需要網站根目錄的 `index.html`。

## 安全說明

前端公開的只能是 Supabase 的 publishable/anon key；不要把 service_role key 放進 HTML。
資料庫透過 RLS 控制前端讀取權限，實際生成由 security definer RPC 執行。

## 資料結構

`generation_batches`
- 每一次生成一筆
- 保存時間、筆數、該批全部號碼

`phone_numbers`
- 每一個完整手機號碼一筆
- `phone_number` 為 primary key，因此永久防重複

## 注意

本工具只依照提供的門號區塊產生「格式符合」的號碼，不代表該號碼實際已由電信業者配發或目前可使用。
