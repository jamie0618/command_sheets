# Influx Database

## 資料庫安裝 (Ubuntu 18.04)

預設 DB port 為 8086，admin UI 介面為 8083

### 安裝流程

參考: https://docs.influxdata.com/influxdb/v1.8/introduction/install/

```
wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -

source /etc/lsb-release

echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/influxdb.list

sudo apt-get update && sudo apt-get install influxdb

sudo service influxdb start

influx (成功的話可進入 influx db 介面)
```

### 設定資料庫 config

config 內容參考: https://docs.influxdata.com/influxdb/v1.8/administration/config/

```
sudo vim /etc/influxdb/influxdb.conf
```

### 用 Influx API 測試資料庫

```
(預設 localhost 跟本機都可以連)
curl -sL -I localhost:8086/ping
curl -sL -I 0.0.0.0:8086/ping
```

在 linux shell 執行以上指令應該要能看到 HTTP 204 No Content response

## Influx shell 操作指令

- 查看所有資料庫

```
show databases
```

- 建立資料庫

```
create database XXX
```

