# docker-aspnetcore-test

## 簡介

本專案為 ASP.NET Core Web API 範例，支援 Dev Container 快速建置跨平台開發環境，並自動處理 HTTPS 憑證與 NuGet 套件還原。適用於 VS Code + Docker，開箱即用。

## 開發環境

本專案使用 [Dev Container](https://containers.dev/)，以 Debian GNU/Linux 12 (bookworm) 與 .NET 8.0 SDK 為基礎。

## devcontainer.json 說明

專案根目錄下的 `.devcontainer/devcontainer.json` 是 VS Code Remote - Containers 的主要設定檔，內容重點如下：

- `"image"`：指定開發容器所用的 Docker 映像（本專案為 `mcr.microsoft.com/dotnet/sdk:8.0`）。
- `"appPort"`：自動開放 5000（HTTP）與 5001（HTTPS）埠，方便本機瀏覽器存取 API。
- `"customizations"`：自動安裝 C# 與 Docker 擴充套件，並啟用 .NET Hot Reload。
- `"postCreateCommand"`：容器建立後自動執行 `dotnet dev-certs https`（產生開發用 HTTPS 憑證）與 `dotnet restore`（還原 NuGet 套件）。
- `"remoteUser"`：指定容器預設使用者（本專案為 root）。

這些設定讓你在任何支援 Dev Containers 的環境下，都能快速建立一致的 .NET 開發環境，並自動處理常見的開發前置作業。

## 啟動方式

1. 使用 VS Code 開啟本資料夾，選擇「在容器中重新開啟」。
2. 容器啟動後會自動執行下列指令產生 HTTPS 憑證並還原 NuGet 套件：
   ```bash
   dotnet dev-certs https
   dotnet restore
   ```
3. 建置與執行 API 專案：
   ```bash
   dotnet build DockerAspNetCoreTest.Api
   dotnet run --project DockerAspNetCoreTest.Api
   ```
4. 開啟 API 首頁（HTTP）：
   ```bash
   $BROWSER http://localhost:5000
   ```
   若需 HTTPS：
   ```bash
   $BROWSER https://localhost:5001
   ```

## 連接埠

- HTTP: 5000
- HTTPS: 5001

## 注意事項

- 若遇到 HTTPS 憑證錯誤，可於容器內執行 `dotnet dev-certs https` 重新產生。
- 若只需 HTTP，可將 `Program.cs` 內的 `app.UseHttpsRedirection();` 註解掉，並於 devcontainer 移除 5001 埠。

## 常用指令

- 清除 NuGet 快取：
  ```bash
  dotnet nuget locals all --clear
  ```
- 產生開發憑證（Linux 僅需產生）：
  ```bash
  dotnet dev-certs https
  ```

---