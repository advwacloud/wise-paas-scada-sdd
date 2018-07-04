# \[DELETE\] /Alarms/{alarmId}

---

* #### 目的

根據alarmId刪除設定與tags

* #### 參數

  * #### 傳入參數
    * alarmId

  * #### 傳出參數

| HTTP Status Code | Details |
| :--- | :--- |
| 200 | true |

* #### 邏輯說明
1. 檢查
    * 檢查 Token 和 scope (manage_alarm)
    * 根據使用者的scada right和傳入的alarmId去得到alarm是否存在
    * 若找到符合條件的alarm, 取得該alram的scadaId
    * `AlarmDao.deleteAlarm(alarmId, trans);`
    * `alarmManager.deleteAlarm({ alarmId, scadaId: alarmResult.scadaId });`
    * 如果worker task成功，則進行transaction commit, 否則rollback





