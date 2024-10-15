# 使用官方的Node.js映像作為基礎映像
FROM node:20-alpine AS build-stage

# 設定工作目錄
WORKDIR /app

# 複製 package.json 和 package-lock.json，並安裝依賴
COPY sakai-vue/package*.json ./

# 安裝專案的依賴項目
RUN npm install

# 複製所有專案檔案到容器內
COPY sakai-vue/ .

# 執行 Vite 的建置指令，生成靜態檔案
RUN npm run build

# 使用 Nginx 作為伺服器來部署靜態檔案
FROM nginx:alpine AS production-stage

# 複製生成的靜態檔案到 Nginx 預設靜態檔案目錄
COPY --from=build-stage /app/dist /usr/share/nginx/html
# 將自定義 Nginx 設定檔覆蓋到容器中的 Nginx 配置
COPY nginx/nginx.conf /etc/nginx/conf.d/default.conf
# 暴露Nginx預設的HTTP端口
EXPOSE 80

# 啟動Nginx
CMD ["nginx", "-g", "daemon off;"]
