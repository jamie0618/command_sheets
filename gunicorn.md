
- Connection in use error:

  用 ```ps -A``` 列出所有的 process，再用 ```sudo kill 12345``` (12345 為 gunicorn 的 process 編號)關掉即可
  https://stackoverflow.com/questions/25230704/why-cant-i-start-my-gunicorn-server-connection-in-use
