# SSL 憑證

1. 安裝 certbot

```
sudo apt-get install python-certbot-nginx
```

2. 確保 http port 80 是可以 access (certbot 會用來驗證，完成後不一定要保持 port 80 開啟沒關係)

3. 申請憑證

```
sudo certbot --nginx -d XXX.com
```

會出現是否需要將 http 自動轉址成 https 的設定選項，1 是不用，2 是要

4. 申請完成後過約 1~3 分鐘即可在瀏覽器該網站上看到"已建立安全連線"的符號了

