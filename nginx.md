# Reference

1. https://blog.gtwang.org/linux/nginx-create-and-install-ssl-certificate-on-ubuntu-linux/

2. requests 套件如何發送請求到私人 SSL 認證網站
https://stackoverflow.com/questions/47208466/sslerrorbad-handshake-when-trying-to-access-resources-custom-certificates-an

# Nginx 安裝

```
sudo apt-get update 
sudo apt-get install nginx
```

Linux 預設安裝到 /etc/nginx

# Nginx 指令

- 啟動 Nginx

```
sudo service nginx start
```

- 停止 Nginx

```
sudo service nginx stop
```

- 查看 Nginx 狀態

```
sudo service nginx status
```

- 查看 Nginx configuration syntax 是否正確，以及 configuration 檔案位置

```
sudo nginx -t
```

- 重啟 Nginx 

```
sudo service nginx restart
```

- 重新載入 Nginx configuration

```
sudo service nginx reload
```

# 建立 SSL 認證

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginx.key -out /etc/nginx/ssl/nginx.crt
```

# Nginx configuration

1. 在 /etc/bginx/sites-enabled 底下建立檔案 (Ex: sudo vim XXX)
2. 寫入 configuration
3. 儲存後 sudo nginx -t 確認 configuration 內容
4. sudo service nginx reload 重新載入 configuration

# Nginx configuration參數

## server 相關

寫在 server {} 內，每行結尾記得加分號 

```
server {
    ...
}
```

- 設定聽哪個 port，如果要用 https 後面要加上 ssl

```
listen 5000; # 聽 port 5000
listen 443 ssl; # 用 https 聽 port 443
```

- 設定要處理哪個 domain name

```
server_name XXX.XXX.com
```

- 設定 request body 最大大小，預設為 1 MB

```
client_max_body_size 100M;
```

- 讓外部連結(Excel/Word等)可以讀到 cookie 的設定

https://stackoverflow.com/questions/2653626/why-are-cookies-unrecognized-when-a-link-is-clicked-from-an-external-source-i-e/55631734#55631734

```
# 放在 server 中
if ($http_user_agent ~* Word|Excel|PowerPoint|ms-office) {
    return 200 '<html><head><meta http-equiv="refresh" content="0"/></head><body></body></html>';
}
```

- 設定 ssl 檔案路徑

```
ssl_certificate /path/to/.crt;
ssl_certificate_key /path/to/.key;
```

- proxy 

```
location / {
        proxy_set_header   X-Forwarded-For $remote_addr;
        proxy_set_header   Host $http_host;
        proxy_pass         "http://127.0.0.1:5000"; # 要 redirect 到的位置
    }
```

- 封鎖所有 IP 連線

```
deny all;
```

- 新增 IP 白名單

```
allow 111.111.111.0/24;
```

也可以將所有的 allow 寫在一個 .conf 當中，並於 server {} 中 include

```
# AWS 放在 /etc/nginx/https-allow 底下
include /path/to/XXX.conf;
```

- http 轉 https

```
# 把所有 http 導向 https
server {
    listen 80; 
    listen [::]:80;
 
    server_name XXX;
 
    rewrite ^ https://XXX$request_uri? permanent;
}
```

## 範例

網站 A 架設於 port 5000，網站 B 架設於 port 8000
將 https request (port 443) 根據 domain name 轉向兩個網站
並限制只有在 allow.conf 中有加入的 IP 才可以訪問

```
server {
    listen 443 ssl; 
    ssl_certificate /path/to/.crt;
    ssl_certificate_key /path/to/.key;

    server_name A;

    include /path/to/allow.conf;
    deny all;

    location / {
        proxy_set_header   X-Forwarded-For $remote_addr;
        proxy_set_header   Host $http_host;
        proxy_pass         "http://127.0.0.1:5000";
    }
}
server {
    listen 443 ssl; 
    ssl_certificate /path/to/.crt;
    ssl_certificate_key /path/to/.key;

    server_name B;

    include /path/to/allow.conf;
    deny all;

    location / {
        proxy_set_header   X-Forwarded-For $remote_addr;
        proxy_set_header   Host $http_host;
        proxy_pass         "http://127.0.0.1:8000"; 
    }
}
```




