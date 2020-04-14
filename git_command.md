
- 查看目前專案下檔案情況(哪些檔案 modified / 已經 add 等等):

  ```git status```
  
- 一次 add 整個專案底下所有被改動過的檔案:

  ```git add .```

- 清除 git add 檔案

  ```git reset```

- 強制更新

  ```
  git fetch --all
  git reset --hard origin/master
  ```

- 多人協作:

```
git fetch 
git checkout origin/master

git add
git commit
git branch XXX
git checkout XXX
git push origin HEAD:XXX
於 github 頁面選擇 compare & pull request
create pull request
等其他人 code review
rebase and merge
delete branch 
git fetch -p
git checkout origin/master
gitg 刪除 local branch
```
