建立專案
```bash=
dotnet new mvc --name myWeb -f net8.0 --use-program-main
```

在專案根目錄建立 `Dockerfile`

利用docker cli 初始化
```bash=
docker init
```

修改`Dockerfile`
```dockerfile=
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["myWeb.csproj", "./"]
RUN dotnet restore "myWeb.csproj"
COPY . .
WORKDIR "/src/"
RUN dotnet build "myWeb.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "myWeb.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "myWeb.dll"]
```

建置image
```bash=
docker build -t my-web
```

啟動容器
```bash=
docker run -p 5555:8080 --name my-web1 my-web
```

## LAB: 使用 Docker Compose

修改 compose.yml
```ymal=
services:
  myweb:
    image: myweb
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      - ASPNETCORE_URLS=http://+:80
      - ASPNETCORE_ENVIRONMENT=Production
    ports:
      - "5050:80"

```

透過docker compose 建置 image
```bash=
docker compose build
```

透過docker compose 運行容器
```bash=
docker compose up -d
```

