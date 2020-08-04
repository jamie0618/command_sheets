# 環境安裝

## Windows

[Reference](https://ithelp.ithome.com.tw/articles/10224406)

1. 安裝 node.js，到 [node.js 下載](https://nodejs.org/en/) 下載並安裝

2. 安裝完後開 cmd 執行下列指令確定 node.js、npm 有成功安裝
  ```
  node -v
  npm -v
  ```

3. 在 cmd 執行下列指令安裝 vue.cli，並確認是否成功安裝
  ```
  npm install -g @vue/cli
  vue --version
  ```
  
4. 在 cmd 執行下列指令建立 vue 專案，輸入後選擇預設選項即可
  ```
  vue create XXX
  ```

5. 建立完成後，cmd 進入該資料夾，啟動伺服器，預設開在 localhost:8080
  ```
  npm run serve
  ```

# Vue.cli 專案架構

```
project/
  |--- node_modules/ (用來放 npm 下載管理的 modules)
  |--- public/
        |--- index.html (SPA 只有一個 html 檔案，由 webpack 幫忙載入所有的元件)
        |--- (全域圖片，無法依照 moudule 的，甚至是要在 web-applicaiton 起來之前就要使用的圖片，如 favicon) 
  |--- src/ (主要的程式碼)
        |--- assets/ (會由 webpack 處理的資源)
                |--- (例如網站中會用到的圖片)
        |--- components/ (Vue 元件，XXX.vue 放這)
                |--- ....
        |--- main.js (app 入口檔案，初始化 Vue 以及載入 Vue 元件)
        |--- App.vue (main app 元件)
```
