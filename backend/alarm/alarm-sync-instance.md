# \[POST\] /Alarms/syncInstance

---

* #### 目的

開啟或關閉Alarm在worker的instance

* #### 參數

  * #### 傳入參數

```
{
  "alarmId": 0,
  "type": "string" // launch || close
}
```

  * #### 傳出參數

| HTTP Status Code | Details |
| -- | -- |
| 200 | true |

* #### 邏輯說明
* 檢查 Token 和 scope (manage_alarm)
* 更新config裡的instaceLaunched欄位
* 開啟或關閉Alarm在worker的instance






