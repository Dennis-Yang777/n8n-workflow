# n8n 本地開發環境

這個專案提供了一個優化的 Docker Compose 配置，用於在本地環境中快速部署和運行 n8n 自動化工作流平台。

## 簡介

[n8n](https://n8n.io/) 是一個功能強大、開源的工作流自動化工具，允許您連接不同的服務和 API，以自動化各種任務和工作流程。此配置使用 Docker 和 Docker Compose 來簡化 n8n 的部署過程。

## 系統需求

- [Docker](https://www.docker.com/get-started) (20.10.0 或更高版本)
- [Docker Compose](https://docs.docker.com/compose/install/) (v2.0.0 或更高版本)

## 快速開始

### 1. 環境配置

首先，複製 `.env.example` 檔案並重命名為 `.env`：

```bash
cp .env.example .env
```

根據需要編輯 `.env` 檔案中的設定。

> **重要提示**：在生產環境中，請務必更改預設的使用者名稱和密碼。

### 2. 啟動服務

執行以下命令啟動 n8n 和 PostgreSQL 服務：

```bash
docker-compose up -d
```

服務啟動後，您可以通過瀏覽器訪問 n8n：

```
http://localhost:5678
```

使用您在 `.env` 檔案中設定的憑證登入：
- 使用者名稱：admin (或您設定的 N8N_BASIC_AUTH_USER)
- 密碼：adminpass (或您設定的 N8N_BASIC_AUTH_PASSWORD)

### 3. 停止服務

要停止並移除所有容器，但保留資料，請執行：

```bash
docker-compose down
```

如果您想要完全移除所有資料（包括資料庫和 n8n 工作流程），請執行：

```bash
docker-compose down -v
```

## 配置說明

### Docker Compose 配置

此配置包含兩個主要服務：

1. **PostgreSQL**：用於儲存 n8n 的資料
   - 使用 PostgreSQL 16 版本
   - 設定了健康檢查，確保資料庫準備就緒

2. **n8n**：主要的工作流自動化平台
   - 使用 n8n 穩定版本
   - 配置了資料庫連接
   - 啟用了基本身份驗證
   - 優化了效能設定

### 資料持久化

所有資料都通過 Docker 卷進行持久化：

- `n8n_postgres_data`：儲存 PostgreSQL 資料庫檔案
- `n8n_data`：儲存 n8n 工作流程、憑證和二進制資料

## 環境變數說明

### 資料庫配置
- `POSTGRES_USER`：PostgreSQL 使用者名稱
- `POSTGRES_PASSWORD`：PostgreSQL 密碼
- `POSTGRES_DB`：PostgreSQL 資料庫名稱
- `POSTGRES_PORT`：PostgreSQL 連接埠

### n8n 配置
- `N8N_PORT`：n8n 網頁界面連接埠
- `N8N_BASIC_AUTH_USER`：n8n 登入使用者名稱
- `N8N_BASIC_AUTH_PASSWORD`：n8n 登入密碼
- `N8N_HOST`：n8n 主機名稱
- `WEBHOOK_URL`：Webhook 回調 URL

### 效能優化
- `N8N_RUNNERS_ENABLED`：啟用工作流執行器
- `N8N_DEFAULT_BINARY_DATA_MODE`：二進制資料模式設定為檔案系統

### 時區設定
- `GENERIC_TIMEZONE`：通用時區設定
- `TZ`：系統時區設定

## 疑難排解

### 無法連接到 n8n
- 確認 Docker 容器是否正在運行：`docker-compose ps`
- 檢查 n8n 容器日誌：`docker-compose logs n8n`
- 確保連接埠 5678 (或您設定的 N8N_PORT) 未被其他服務佔用

### 資料庫連接問題
- 檢查 PostgreSQL 容器是否健康：`docker-compose ps postgres`
- 檢查 PostgreSQL 日誌：`docker-compose logs postgres`
- 確認環境變數設定是否正確

## 更多資源

- [n8n 官方文檔](https://docs.n8n.io/)
- [n8n Docker 安裝指南](https://docs.n8n.io/hosting/installation/docker/)
- [n8n 社群論壇](https://community.n8n.io/)

## 授權

此配置遵循 [MIT 授權](https://opensource.org/licenses/MIT)。n8n 本身的授權請參考 [n8n 授權條款](https://github.com/n8n-io/n8n/blob/master/LICENSE.md)。