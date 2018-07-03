# \[GET\] /Alarms

---

* #### 目的

取得Alarm的細節(config and tags)與列表，且可以做欄位的篩選與、分頁功能。

* #### 參數

  * #### 傳入參數

| Name | Description |
| -- | -- |
| alarmId | Alarm的Id |
| scadaId | 該Alarm屬於哪個scada |
| count | 一次取出的alarm數量, 該值需大於等於1且小於等於1000 |
| index | 取出之list的起始位置, index須大於0 |
| sortby | Sort by the specified property |
| desc | order by desc or not |

  * #### 傳出參數

```
// http status code: 200
{
  "list": [
    {
      "alarmId": 0,
      "scadaId": "string",
      "scadaName": "string",
      "code": "string",
      "message": "string",
      "conditionType": 0,
      "lowerLimit": 0,
      "upperLimit": 0,
      "instanceLaunched": true,
      "tags": [
        {
          "deviceId": "string",
          "deviceName": "string",
          "tagName": "string"
        }
      ]
    }
  ],
  "totalCount": 0,
  "index": 0,
  "count": 0
}
```

* #### 邏輯說明

1. 檢查 Token 和 scope (manage_alarm)
2. 檢查傳入的參數是否正確
3. 得到有scadaRight的Alarm
4. 將沒有deviceRight的Alarm Tags濾掉



