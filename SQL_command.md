## 資料庫相關

- 查詢資料庫清單:

  ```\l ```

- 切換到特定資料庫:

  ```\c XXX```

## table 相關

- 查詢 table 清單:
    
  ```\dt```

- 查詢 table schema:
  
  ```\d+ table```

- 查看 table 中所有資料:

  ```TABLE table;```

- 清空 table: 
	
  ```TRUNCATE table;```
    
- 刪除 table:
	
  ```DROP TABLE table;```
    
- 新增 table column:

  https://www.postgresql.org/docs/9.1/sql-altertable.html
	
  ```ALTER TABLE table ADD COLUMN col_name text NOT NULL```
    
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

- 查詢某 table 總共資料筆數:
	
  ```SELECT COUNT(*) FROM table;```

- 看最新 N 筆資料:
	
  ```SELECT * FROM table ORDER BY col DESC LIMIT n;```

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
