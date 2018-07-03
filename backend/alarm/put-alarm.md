# \[PUT\] /Alarms/{alarmId}

---

* #### 目的

修改alarm的設定，與該alarm下的tags

* #### 參數

  * #### 傳入參數
    * alarmId
    * data
      ```
      {
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
    * 如果沒有要修改的欄位，就不用傳
    * tags欄位需帶入增減tag後的結果
    * 如果tags沒有要修改，勿帶空陣列"[]"，否則會刪除該使用者有權限的所有tag
    * conditionType/lowerLimit/upperLimit須符合對應關係


  * #### 傳出參數

| HTTP Status Code | Details |
| :--- | :--- |
| 200 | true |

* #### 邏輯說明

1. 檢查 Token 和 scope (manage_alarm)
2. 根據alarmId取得現有的alarm資訊，包含config和tags
3. 這裡取得的tags是沒篩選right的tags (`tagsObject#1`)
3. 將新的config和既有的config做merge
    * 如果有被修改過，就用新的data蓋過去
5. 檢查
    * scada right
    * code是否在同scada下重複
    * 檢查conditionType/lowerLimit/upperLimit的關係
    * 檢查傳入的tags是否都符合device right
6. 取得參數傳入的tags (`tagsObject#2`)
6. 從既有tags裡取出使用者有權限的tags (`tagsObject#3`)
7. 把傳入的tags(`tagsObject#2`)和既有權限的tags(`tagsObject#3`)比較，取出新增與刪除了那些tags
    * 新增的tags (`tagsObject#4`)
    * 刪除的tags (`tagsObject#5`)
8. merge要寫入postgres的tags(`tagsObject#6`) = `tagsObject#1` + `tagsObject#4` - `tagsObject#5`
    * 這樣做是為了避免刪除沒有權限的tags
9. AlarmDao.updateAlarm(alarmId, configUpdateObj, trans);
    * configUpdateObj包含了`tagsObject#6`
10. 如果該alarm的instanceLaunched是true，則更新worker中的alarm instance，分成3種case
    * 若設定有改到
        * alarmManager.updateAlarm(configUpdateObj);
        * configUpdateObj包含了`tagsObject#6`
    * 若單純新增tags
        * alarmManager.insertAlarm({ alarmId, scadaId, tags: newTags });
        * (`tagsObject#4`)
    * 若單純刪除tags
        * alarmManager.deleteAlarm({ alarmId, scadaId, tags: deleteTags });
        * (`tagsObject#5`)
11. 若worker操作都成功，則將transaction commit，否則rollback




