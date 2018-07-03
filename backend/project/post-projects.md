# \[POST\] /Projects

---

* #### 目的

建立新的Project，支援一次建立多個。test

* #### 架構說明

```
project.json
```

* #### 參數

  * #### 傳入參數

    ```
    [
      {
        "projectId": "string",  // the unique Id of the defined project
        "description": "string"  //  the description of the defined project
      }
    ]
    ```
  * #### 傳出參數

  | HTTP Status Code | Details |
  | :--- | :--- |
  | 200 | true |
* #### 邏輯說明

1. 檢查 Token 和 user scope, 並取得 userName
2. 檢查 projectObj 是否合法
3. 呼叫UserDao.getUserByUserName取得 userId
4. 開啟 Transaction
5. 呼叫ProjectDao.insertProject\(projectObj, trans\) insert Project
6. 呼叫UserAllowDeviceDao.insertAccessRight\(\[userId, projectId\], trans\) insert Project 存取權限
7. 如果 Transaction 成功, 回傳true, 否則rollback並回傳錯誤



