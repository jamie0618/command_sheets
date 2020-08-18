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
  npm install -g @vue/cli @vue/cli-init
  vue --version
  ```  
  
4. 在 cmd 執行下列指令建立 vue 專案，輸入後選項 "Install vue-router" 記得選 Y
  ```
  vue init webpack XXX
  ```

5. 建立完成後，cmd 進入該資料夾，啟動伺服器，預設開在 localhost:8080
  ```
  npm run dev
  ```

6. (optional) 執行以下指令安裝 bootstrap-vue (用 boostrap)
  ```
  npm install bootstrap-vue boostrap
  ```
  
7. (optional) 執行以下指令安裝 axios (送 request)
  ```
  npm install axios vue-axios --save
  ```

8. (optional) 執行以下指令安裝 moment (處理時間格式)
  ```
  npm install moment --save
  ```
  
9. (optional) 執行以下指令安裝 chart.js (畫圖表)
  ```
  npm install vue-chartjs chart.js --save
  ```

10. (optional) 執行以下指令安裝 leaflet (openstreet 地圖使用)
  ```
  npm install leaflet
  ```

11. (optional) 執行以下指令安裝 leaflet-ant-path (leaflet 路徑動畫 plugin)
  ```
  npm install leaflet-ant-path --save
  ```
  
12. (optional) 執行以下指令安裝 leaflet time dimension (leaflet 時間軸 plugin)
  ```
  npm install leaflet-timedimension --save
  ```
