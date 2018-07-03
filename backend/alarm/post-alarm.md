# \[POST\] /Alarms

---

* #### 目的

建立新的Alarm，包含Alarm的設定和Alarm下有哪些測點(Tag)。

* #### 參數

  * #### 傳入參數

    ```
    {
        "scadaId": "string",
        "code": "string",
        "message": "string",
        "conditionType": 0,
        "lowerLimit": 0,
        "upperLimit": 0,
        "tags": [
            {
            "deviceId": "string",
            "tagName": "string"
            }
        ]
    }
    ```
  * #### 傳入參數說明
    * conditionType: `{1: above, 2: below, 3: equal, 4: in range, 5: out range}`
    * conditionType/lowerLimit/upperLimit關聯
        * above(1): 大於等於`upperLimit`，就觸發警報
        * below(2): 小於等於`lowerLimit`，就觸發警報
        * equal(3): 值同時等於`lowerLimit`和`upperLimit`，就觸發警報
        * in range(4): 值大於`lowerLimit`且小於`upperLimit`，則觸發警報
        * out range(5): 值大於`upperLimit`或小於`lowerLimit`，就觸發警報
  * #### 傳出參數

| HTTP Status Code | Details |
| -- | -- |
| 200 | `{"alarmId": 0}` |

* #### 邏輯說明

1. 檢查 Token 和 scope (manage_alarm)
2. 檢查傳入參數是否合法
    * 檢查參數的型態/長度/格式是否正確 
    * 檢查tags裡是否有重複的tag(相同deviceId+tagName)
    * 檢查conditionType與lowerLimit/upperLimit的關係是否正確
3. 檢查scadaRight，檢查使用者對於傳入的scada是否有權限
4. 檢查code是否重複出現在相同的scada下
5. 若有傳入tags，檢查使用者對於tag的device是否有deviceRight，並檢查該tag是否真的存在。
6. 呼叫`AlarmDao.insertAlarm(alarm, trans)`，透過Dao將設定寫入資料庫。
7. 如果 Transaction 成功(commit), 回傳`{"alarmId": 0}`, 否則rollback並回傳錯誤



