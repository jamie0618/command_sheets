## 資料庫相關

- 查詢資料庫清單:

  ```\l ```

- 切換到特定資料庫:

  ```\c XXX```

## 連線相關

- 查詢連線數量及狀態

  ```SELECT count(*),state FROM pg_stat_activity GROUP BY 2;```
  
- 查詢 idle timeout 時間

  ```show idle_in_transaction_session_timeout;```

## 資料格式相關

- 日期

   ```WHERE "update_time" < date'2019-01-01'```

## table 相關

- 查詢 table 清單:
    
  ```\dt```

- 查詢 table schema:
  
  ```\d+ table```

- 查看 table 中所有資料:

  ```TABLE table;```

- 清空 table: 
	
  ```TRUNCATE table;```
- 複製 table (包含格式和資料):

  ```
  CREATE TABLE new_table AS TABLE table; (包含格式和資料)
  CREATE TABLE new_table AS TABLE table WITH NO DATA; (只包含格式，不包含資料)
  ```

- 刪除 table:
	
  ```DROP TABLE table;```

- 刪除 table column:

  ```ALTER TABLE table DROP COLUMN col;```

- 重新命名 table column:
  
  ```ALTER TABLE table RENAME COLUMN col TO new_col;```

- 新增 table column:

  https://www.postgresql.org/docs/9.1/sql-altertable.html
	
  ```ALTER TABLE table ADD COLUMN col_name text NOT NULL;```
    
- 刪除 table 中特定條件資料:
	
  ```DELETE FROM table WHERE col='xxx';```

- 更改 table PRIMARY KEY:
	
  ```
  # 刪去舊的 key，注意 table_pkey 中的 table 要改成 table 名稱
  ALTER TABLE table DROP CONSTRAINT table_pkey; 
  ALTER TABLE table ADD PRIMARY KEY (A,B,C);
  ```
    
- 更改 column 名字:
	
  ```ALTER TABLE table RENAME col_name TO new_col_name;```
  
- 更改資料內容:

  ```UPDATE table SET col='XXX' WHERE condition```

- 查詢某 table 總共資料筆數:
	
  ```SELECT COUNT(*) FROM table;```

- 看最新 N 筆資料:
	
  ```SELECT * FROM table ORDER BY col DESC LIMIT n;```
  
- 搜尋特定條件:

  ```SELECT * FROM table WHERE col='xxx';```

- 搜尋 datetime 欄位:

  ```SELECT * FROM table WHERE col > '2020-07-24 10:00:00.000'```

- 搜尋特定 column 中 unique 的值:

  ```SELECT DISTINCT column_1 FROM table;```
  
- 從現有 table 複製資料和格式並建立新的 table (注意 primary key 需要重新設定):

  ```CREATE TABLE new_table AS SELECT * FROM old_table WHERE XXX=YYY;```
  
- 對現有 table 加入 ID column:

  ```ALTER TABLE table ADD COLUMN col SERIAL;```
  
  用了之後該 table 會另外產生一份 sequence，可以在 \d 中看到
  
  如果要 RESET 該 sequence:
  
  ```ALTER SEQUENCE seq RESTART```

- 刪除特定 column 當中，有重複的 row (每組重複只留下一筆):

  ```DELETE 
  FROM table
  WHERE ctid IN 
  (
    SELECT ctid 
    FROM(
        SELECT 
            *, 
            ctid,
            row_number() OVER (PARTITION BY col ORDER BY ctid) 
        FROM table
    )s
    WHERE row_number >= 2
  );```

- 搜尋 PSQL JSONB 欄位中，key是否存在

  ```
  SELECT ... WHERE json_column ? 'key_name';
  SELECT ... WHERE json_column -> 'key_name' ? 'sub_key_name';  (nested json key)
  ```


## 連線相關

- 設定使用者即可連線IP:

	https://docs.postgresql.tw/server-administration/client-authentication/the-pg_hba.conf-file
	
  修改 PostgreSQL/data/pg_hba.conf 檔案 (修改完須重啟)
  ```
  # 讓使用者 jack 在不限制 IP 情況下連線
  host event_db jack all md5
  ```
 
- 重新啟動 PostgreSQL (Windows):

	在開始中的 PostgreSQL 資料夾中有 Reload Configuration 程式，右鍵以系統管理員執行即可

## 使用者帳號相關

- 查看所有使用者:
  
  ```\du```
  
- 建立使用者帳號:
    
  ```CREATE USER jack WITH ENCRYPTED PASSWORD 'jamie0618';```
	
  **建立新帳號後，這帳號預設沒有權限可以對資料庫做任何事，需額外開放權限**
    
- 設定使用者權限
    
  https://www.postgresql.org/docs/10/sql-grant.html
	
  ```GRANT SELECT, INSERT ON table TO user;```
    
- 刪除使用者
	
  ```
  REVOKE ALL PRIVILEGEG ON TABLE table FROM user;
  DROP USER user;
  ```
