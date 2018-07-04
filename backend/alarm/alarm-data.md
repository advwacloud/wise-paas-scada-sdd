# \[POST\] /Alarms/data

---

* #### 目的

取得警報紀錄

* #### 參數

  * #### 傳入參數

```
{
  "scadaId": "string",
  "deviceId": "string",
  "tagName": "string",
  "startTs": "2018-07-04T02:03:26.982Z",
  "endTs": "2018-07-04T02:03:26.982Z",
  "desc": true,
  "count": 0,
  "index": 0
}
```

  * #### 傳入參數說明
    * scadaId/startTs一定要傳，不可為空，其他欄位則都可不帶

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
      "ts": "2018-07-04T02:37:31.604Z",
      "ackTs": "2018-07-04T02:37:31.604Z",
      "clearTs": "2018-07-04T02:37:31.604Z",
      "code": "string",
      "message": "string",
      "scadaName": "string",
      "deviceName": "string"
    }
  ],
  "totalCount": 0,
  "index": 0,
  "count": 0
}
```

* #### 邏輯說明
* 檢查 Token 和 scope (manage_alarm)
* 整理出要查詢的filter object
    * 如果只有傳scadaId
        * 找出有deviceRight的device list當作篩選條件
        * [{scadaId, deviceId}]
    * 如果只有傳scadaId和deviceId
        * 確認使用者是否有deviceRight
    * 如果scadaId/deviceId/tagName都有傳
        * 確認使用者是否有deviceRight，並確認tag存在
    * 如果傳tagName卻沒傳deviceId, 報錯
* 呼叫`alarmManager.getAlarmLog(condition)`





