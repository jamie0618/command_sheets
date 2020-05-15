# Reference

1. https://blog.gtwang.org/linux/nginx-create-and-install-ssl-certificate-on-ubuntu-linux/

2. requests 套件如何發送請求到私人 SSL 認證網站
https://stackoverflow.com/questions/47208466/sslerrorbad-handshake-when-trying-to-access-resources-custom-certificates-an

# Nginx 安裝

```
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

# 設定 Nginx configuration

1. 在 /etc/bginx/sites-enabled 底下建立檔案
2. 檔案內寫入以下內容

```
server {
    listen 1234; # 用 http 聽 port 1234
    listen 443 ssl default_server; # 用 https 聽 port 443

    # 預設上限為 1MB
    client_max_body_size 100M;

    # ssl 檔案路徑
    ssl_certificate /etc/nginx/ssl/nginx.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx.key;

    location / {
        proxy_set_header   X-Forwarded-For $remote_addr;
        proxy_set_header   Host $http_host;
        proxy_pass         "http://127.0.0.1:5000"; # 要 redirect 到的位置
    }
}
```

3. 儲存後重啟 Nginx 即可，之後傳送到該 IP port 1234 (using HTTP) 的請求，以及用 https 傳送 (default 443) 的請求都會被轉送給 port 5000 的 application



