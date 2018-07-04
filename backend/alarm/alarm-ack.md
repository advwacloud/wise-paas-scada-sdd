# \[POST\] /Alarms/ack

---

* #### 目的

對警報狀態中的測點發送ack

* #### 參數

  * #### 傳入參數

```
{
  "alarmId": 0,
  "scadaId": "string",
  "deviceId": "string",
  "tagName": "string"
}
```

  * #### 傳入參數說明
    * alarmId/scadaId/deviceId/tagName都是必填欄位

  * #### 傳出參數

| HTTP Status Code | Details |
| -- | -- |
| 200 | true |

* #### 邏輯說明
* 檢查 Token 和 scope (manage_alarm)
* 確認alarmId/scadaId/deviceId/tagName存在
* `alarmManager.ackAlarm(ackTag)`






