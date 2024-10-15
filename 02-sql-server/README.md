# 使用 Docker 執行 SQL Server Linux 容器映像
## 操作步驟
### 建立容器：
```
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=MyPass@word" \
   -p 1433:1433 --name sql1 \
   --platform linux/amd64 \
   -d \
   mcr.microsoft.com/mssql/server:2022-latest
```
### 開啟 SSMS (Azure Data Studio) 建立一個測試的DB
```
user:sa
password:MyPass@word
```
### 刪除 sql1 容器
```
docker container stop sql1
docker container rm sql1
```
### 重新建立容器
```
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=MyPass@word" \
   -p 1433:1433 --name sql1 \
   --platform linux/amd64 \
   -d \
   mcr.microsoft.com/mssql/server:2022-latest
```
### 查看原本建立的DB是否還存在

### 建立 volume
```
docker volume create sql1-db-data
```
### 調整啟動參數
```
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=MyPass@word" \
   -p 1433:1433 --name sql1 \
   --platform linux/amd64 \
   -v sql1-db-data:/var/opt/mssql \
   -d \
   mcr.microsoft.com/mssql/server:2022-latest
```
### 再建立DB，測試容器刪除後重啟是否還會存在

### 掛載主機目錄版本
需要先建立預計要掛載的目錄 `db-data`
確認當前目錄後執行 docker 命令
```
docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=MyPass@word" \
   -p 1433:1433 --name sql1 \
   --platform linux/amd64 \
   -v ./db-data:/var/opt/mssql \
   -d \
   mcr.microsoft.com/mssql/server:2022-latest
```

## 小結
- Docker 內部 Volume 生命週期
  - 與容器生命週期不同，在 volume 刪除前都會存在。
- 將Volume 掛載在主機
  - 注意路徑寫法(是否有先加斜線)

## 參考資料
- [使用 Docker 執行 SQL Server Linux 容器映像](https://learn.microsoft.com/zh-tw/sql/linux/quickstart-install-connect-docker?view=sql-server-ver16)