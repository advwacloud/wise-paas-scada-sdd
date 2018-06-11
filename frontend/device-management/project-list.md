# Project List

---

* #### 目的

使用者在Device Management 頁面可以編輯project的Id及敘述，並可新增新的project項目，另外也可以執行刪除動作。

* #### 架構說明

```
Vue-components
　　src/components/ProjectManger/ProjectList.vue
　　src/components/MenuBar.vue
　　src/components/Common/Modal.vue
　　src/components/Common/ConfigModal.vue
Vue-router
　　src/router/index.js
```

* #### UI

![](https://coolgo0811.gitbooks.io/wise-paas-scada-software-development-document/content/assets/projectlist.PNG)![](https://coolgo0811.gitbooks.io/wise-paas-scada-software-development-document/content/assets/project_edit.png)![](https://coolgo0811.gitbooks.io/wise-paas-scada-software-development-document/content/assets/project_delete.png)![](https://coolgo0811.gitbooks.io/wise-paas-scada-software-development-document/content/assets/newproject.png)

* #### 邏輯說明

```
1. 在Device Management中編輯Project的頁面(Url: /Login/CloudManager/DeviceManagement)  
　1.1 透過src/components/MenuBar.vue按下1號按鈕執行select function，透過src/router/index.js呼叫src/components/ProjectList.vue進入可編輯Project的頁面。  
　1.2 同時在src/components/MenuBar.vue檢查$store.state.user.scope使用者的權限將變數存入scopeEdit。  
2. 一進入Project的頁面即執行$checkLogin和getProjectList function。  
　2.1 執行$checkLogin判斷scopeEdit狀態。  
  　2.1.1 若為true即可顯示4、5號按鈕。  
  　2.1.2 若為false即不顯示4、5、7號按鈕。  
  2.2 執行getProjectList列出所有project相關資訊。  
　　2.2.1 呼叫API: GET /Projects。  
　　　2.2.1.1 若呼叫成功列出project資料，並將要顯示資訊存到projects陣列中。  
　　　2.2.1.2 若呼叫失敗則判斷err.response.status === 404狀態。  
　　　　2.2.1.2.1 若為true，令projects = []。  
　　　　2.2.1.2.2 若為false，執行$checkToken。   
3. 按下2號按鈕執行getProjectList並依使用者選擇或輸入做排序。  
4. 按下3號按鈕執行handleDetailClick。  
　4.1 呼叫API: /Projects/:projectId。  
　　4.1.1 若呼叫成功，令detailVisible = true，並將要顯示的資訊存到tempProject物件中。  
　　　4.1.1.1 呼叫src/components/Common/ConfigModal.vue，並傳入Config、typeEdit、obj、type等參數，分別為:Config="true" :typeEdit="scopeEdit" :obj="tempProject" :type="'project'"。  
　　　　4.1.1.1.1 若呼叫成功則跳出detail視窗  
　　　　　4.1.1.1.1.1 若typeEdit=true即顯示7號按鈕，按下7號按鈕執行handleEditClick。  
　　　　　　4.1.1.1.1.1.1 分別將顯示的參數存入showObj物件，編輯參數存入editObj物件，並判斷obj.alarm!==undefined。  
　　　　　　　4.1.1.1.1.1.1.1 若為true將obj.alarm參數存入showAlarm和editAlarm。  
　　　　　4.1.1.1.1.2 編輯完後按下9號按鈕執行handleSaveClick。  
　　　　　　4.1.1.1.1.2.1 呼叫API: PUT /Projects/{projectId}。  
　　　　　　　4.1.1.1.1.2.1.1 若呼叫成功即完成編輯，並回到一開始detail畫面。  
　　　　　　　4.1.1.1.1.2.1.2 若呼叫失敗回傳errMsg。  
　　　　　4.1.1.1.1.3 若不想儲存編輯按下8號按鈕，令editObj = {}並回到一開始detail畫面。  
　　　　　4.1.1.1.1.4 按下10號按鈕，令tempProject = {}並令detailVisible = False關閉視窗。  
　　　4.1.2 若呼叫失敗執行$checkToken。  
5. 按下4號按鈕執行handleDeleteClick。  
　5.1 將選取的project資訊存入tempProject物件中，並令deleteVisiable = true。  
　　5.1.1 呼叫src/components/Common/Modal.vue，顯示delete視窗。  
　　　5.1.1.1 按下11號按鈕執行handleDeleteSubmit。  
　　　　5.1.1.1.1 執行deleteProject並呼叫API: DELETE /Projects/{projectId}。  
　　　　　5.1.1.1.1.1 若呼叫成功，將畫面上選取的project資訊刪除，並令deleteVisiable = false關閉視窗。  
　　　　　5.1.1.1.1.2 若呼叫失敗，顯示errStr，並令deleteVisiable = false關閉視窗。  
　　　5.1.1.2 按下12號按鈕執行handleCancelClick。  
　　　　5.1.1.2.1 令tempProject = {}，並令deleteVisiable = false關閉視窗。  
6. 按下5號按鈕執行handleAddClick。  
　6.1 令addVisible = true和tempProject = {}。  
　　6.1.1 呼叫src/components/Common/Modal.vue，顯示New Project視窗。  
　　　6.1.1.1 按下13號按鈕，執行handleAddSubmitClick。  
　　　　6.1.1.1.1 判斷tempProject.projectId狀態。  
　　　　　6.1.1.1.1.1 若為flase，顯示errMsg。  
　　　　　6.1.1.1.1.2 若為true，執行createNewProject。  
　　　　　　6.1.1.1.1.2.1 呼叫API: POST /Projects。  
　　　　　　　6.1.1.1.1.2.1.1 若呼叫成功執行getProjectList，並令addVisible = false關閉視窗。  
　　　　　　　6.1.1.1.1.2.1.2 若呼叫失敗顯示errMsg。  
 　　　　6.1.1.2 按下14號按鈕，執行handleCancelClick。  
 　　　　　6.1.1.2.1 令tempProject = {}並令addVisible = false關閉視窗。  
7. 按下6號按鈕執行handleProjectClick。  
　7.1 透過src/router/index.js呼叫src/components/ScadaList.vue進入可編輯SCADA的頁面。 
```



