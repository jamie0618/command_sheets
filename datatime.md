- datetime 修正時間:

  ```python
  from datetime import datetime
  t = datetime.now() # datetime.datetime(2019, 12, 30, 14, 24, 2, 292525)
  t = t.replace(microsecond) # datetime.datetime(2019, 12, 30, 14, 24, 2)
  ```

- 時間運算:

  ```python
  from datetime import datetime, timedelta
  t1 = datetime.now() 
  t2 = t1 + timedelta(seconds=10) 
  # t1: datetime.datetime(2019, 12, 30, 14, 27, 27, 888010)
  # t2: datetime.datetime(2019, 12, 30, 14, 27, 37, 888010)
  ```
