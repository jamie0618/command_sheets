
- 隨機將 dataframe 切分成 train/test:

  ```
  df_train = df.sample(frac=0.8, random_state=0)
  df_test = df.drop(df_train.index)
  ```

- 隨機 shuffle dataframe 並更改 index:
  ```
  df = df.sample(frac=1).reset_index(drop=True)
  ```
