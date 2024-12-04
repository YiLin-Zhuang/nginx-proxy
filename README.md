# 教學
## 步驟 1：**下載 docker-compose.yml 檔案**

在終端機中執行以下指令以下載 docker-compose.yml 檔案，並修改 `DEFAULT_EMAIL`：

```sh
curl -LO https://raw.githubusercontent.com/YiLin-Zhuang/nginx-proxy/master/docker-compose.yml

```

## 步驟 2：**建立網路**

在終端機中執行以下指令以建立名為 "nginx-proxy" 的網路：

```sh
docker network create nginx-proxy

```

## 步驟 3：**啟動 nginx-proxy**

在終端機中執行以下指令以啟動 nginx-proxy：

```sh
docker-compose up -d

```

## 步驟 4：**建立範例網站**

在需要使用 SSL 的 docker-compose.yml 檔案中，加入 `VIRTUAL_HOST`、`LETSENCRYPT_HOST` 和 `networks`。以下範例設定以建立一個範例網站：

```sh
website:
    image: nginx:alpine
    container_name: website
    restart: always
    volumes:
      - ./website:/usr/share/nginx/html:ro
    environment:
      # 將 "www.yourdomain.com,yourdomain.com" 替換為你的網域
      - VIRTUAL_HOST=www.yourdomain.com,yourdomain.com
      - LETSENCRYPT_HOST=www.yourdomain.com,yourdomain.com
      # 如果需要指定端口，可以加入以下環境變數
      #- VIRTUAL_PORT=8080

# 加入 "nginx-proxy" 網路
networks:
  default:
    external: true
    name: nginx-proxy

```

這些步驟將幫助你設定一個 proxy server，並自動更新 SSL 證書。
