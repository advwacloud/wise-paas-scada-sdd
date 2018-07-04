# \[POST\] /Alarms/status

---

* #### 目的

取得目前警報狀態，可以知道有哪些測點正處於警報狀態，可篩選該警報是否有ack過

* #### 參數

  * #### 傳入參數

```
{
  "scadaId": "string",
  "deviceId": "string",
  "tagName": "string",
  "count": 0,
  "index": 0,
  "acked": true
}
```

  * #### 傳入參數說明
    * 所有欄位都可不傳，會取出使用者有權限的所有警報點狀態
    * 若有帶acked欄位，則只會篩選出有ack過與否的狀態

  * #### 傳出參數

```
{
  "list": [
    {
      "alarmId": 0,
      "scadaId": "string",
      "deviceId": "string",
      "tagName": "string",
      "value": 0,
      "ts": "2018-07-04T03:12:41.688Z",
      "acked": true,
      "ackTs": "2018-07-04T03:12:41.688Z",
      "scadaName": "string",
      "deviceName": "string",
      "code": "string",
      "message": "string"
    }
  ],
  "totalCount": 0,
  "index": 0,
  "count": 0,
  "id": 0
}

```

* #### 邏輯說明
* 檢查 Token 和 scope (manage_alarm)
* 整理出要查詢的filter object
    * 如果scadaId/deviceId/tagName三個欄位都沒帶
        * 整理出使用者有權限的所有device, [{scadaId, deviceId}]
    * 如果只有傳scadaId
        * 找出有deviceRight的device list當作篩選條件
        * [{scadaId, deviceId}]
    * 如果只有傳scadaId和deviceId
        * 確認使用者是否有deviceRight
    * 如果scadaId/deviceId/tagName都有傳
        * 確認使用者是否有deviceRight，並確認tag存在
    * 如果傳tagName卻沒傳deviceId, 報錯
* 呼叫`alarmManager.getAlarmLog(condition)`





