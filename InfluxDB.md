# Influx DB 安裝 (Ubuntu 18.04)

預設 DB port 為 8086，admin UI 介面為 8083

## 安裝流程

參考: https://docs.influxdata.com/influxdb/v1.8/introduction/install/

```
wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -

source /etc/lsb-release

echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/influxdb.list

sudo apt-get update && sudo apt-get install influxdb

sudo service influxdb start

influx (成功的話可進入 influx db 介面)
```

## 設定資料庫 config

config 內容參考: https://docs.influxdata.com/influxdb/v1.8/administration/config/

```
sudo vim /etc/influxdb/influxdb.conf
```

## 用 Influx API 測試資料庫

```
(預設 localhost 跟本機都可以連)
curl -sL -I localhost:8086/ping
curl -sL -I 0.0.0.0:8086/ping
```

在 linux shell 執行以上指令應該要能看到 HTTP 204 No Content response

# Influx shell 操作指令

- 查看

  ```
  SHOW DATABASES (查看所有資料庫) (第一次安裝後是沒有任何資料庫的，除了系統自帶的_internal)
  SHOW MEASUREMENTS (查看所有 measurement)
  ```

- 建立資料庫

  ```
  CREATE DATABASE XXX
  ```

- 使用特定資料庫

  ```
  USE XXX
  ```

- 在 Influx shell 上用 RFC3339 顯示時間，而不是用 timestamp

  ```
  precision rfc3339
  ```

- insert

  可以指定 timestamp，如沒有指定的話就以系統當下時間 insert
  
  ```
  INSERT <measurement>,[tag1]=xx,[tag2]=yy <field1>=123,<field2>=456 [Unix timestamp in nanosecond]
  ```

# Influx DB 概念 

## 資料格式

InfluxDB 裡儲存的資料稱為時間序列資料。資料包括: 

- time 
- measurement
- field (至少一個) (key-value格式)
- tag (可有可無，可多個) (key-value格式)

## 重點名詞

- database: 
  - 資料庫
  - InfluxDB 中可以包含多個獨立的 database

- measurement: 
  - 類似 SQL 的 table
  - 不需要先建立，也不用先設定資料欄位，insert 以後就可以用了

- time: 
  - 每筆紀錄一定會有
  - 有 index
  - 用 Unix timestamp 儲存

- field: 
  - 資料
  - 每筆紀錄一定會有
  - 為 key-value 格式
  - 無 index(故不應該包含常常會查詢的資料)
  - 類似 SQL 中沒有設 index 的 column

- tag: 
  - 資料
  - 不一定要有
  - 為 key-value 格式
  - 有 index(可包含常常會查詢的資料)
  - 類似 SQL 中有設 index 的 column
  - 一個 measurement 中不同筆資料 tag 的格式可以不一樣
    |time|field1|field2|tag1|tag2|tag3|
    |---|---|---|---|---|---|
    |1|123|456|xx|yy||
    |2|123|789|||zz|
    |3|123|456||yy|zz|

- series: 
  - 為共同 retention policy、measurement、tag set 的集合

  例如某個 measurement A，retention policy 為B，當中的 tag 有四種可能的組合(x=1or2, y=1or2)

  |series 編號|retention policy|measurement|tag set|
  |---|---|---|---|
  |1|B|A|x=1,y=1|
  |2|B|A|x=1,y=2|
  |3|B|A|x=2,y=1|
  |3|B|A|x=2,y=2|
  
- point: 
  - 相同 time、相同 series 的 field，是唯一存在的(同一個時間點，相同的 series，只會存在一筆資料)

# Reference

https://jasper-zhang1.gitbooks.io/influxdb/content/Introduction/getting_start.html

https://docs.influxdata.com/influxdb/v1.8/query_language/manage-database/
