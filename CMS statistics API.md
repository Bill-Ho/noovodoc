---
tags: Noovo, QA, API
---
# CMS statistics API

## Statistics of clients
* Type: HTTP GET
* URL: [CMS_SERVER]/v1/group/statistics
* Parameter
	* gp : Group ID, Ex: 中壢區=182
	* regular : daily, weekly 
	* date : 從該日起倒數 * 7 daily/weekly/moonthly
	* limite : 回傳筆數限制
* Parameter Example:
	* 中壢區自 2019-12-25 倒數7日
	* gp=182&regular=daily&date=2019-12-25&limit=7
	* 中壢區自 2019-12-25 倒數4周
	* gp=182&regular=weekly&date=2019-12-25&limit=4
	
* CURL Example: 中壢區自 2019-12-25 倒數3日
```script=
curl --location --request GET 'http://www.satott.com/v1/group/statistics?gp=182&regular=weekly&date=2019-12-25&limit=3' \
--header 'Authorization: Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJOb292byBDbG91ZCIsImV4cCI6MTUzODQ2MTE4MSwianRpIjoiZXhhbXBsZUBub292by5jbyIsImlzcyI6Ik5vb3ZvIENsb3VkIFBvcnRhbCJ9.DjOGIB24NF-ei3NX5jokKFwxiPkDqTukS7JnwkrdhaZ1TfgTozpoabj3JDGdUoGWrxX49gAmgb_2M5TCpO-obA'
```

* Return : JSON
```json=
{
  "token": "",
  "description": "",
  "result": "Succeed",
  "state": 200,
  "data": {
    "traffic": [
      {
        "date": "2019-12-23",
        "total": 198
      },
      {
        "date": "2019-12-24",
        "total": 328
      },
      {
        "date": "2019-12-25",
        "total": 1278
      }
    ],
    "auth_user": [
      {
        "date": "2019-12-23",
        "total": 1
      },
      {
        "date": "2019-12-24",
        "total": 4
      },
      {
        "date": "2019-12-25",
        "total": 9
      }
    ],
    "vod": [
      {
        "date": "2019-12-25",
        "CID": 1705,
        "source": "noovo",
        "views": 1,
        "duration": null,
        "shortTitle": "獅子王預告片",
        "category": "Animation"
      },
      {
        "date": "2019-12-25",
        "CID": 1709,
        "source": "noovo",
        "views": 2,
        "duration": null,
        "shortTitle": "布魯克林孤兒 Motherless Brooklyn",
        "category": "Comedy"
      },
      {
        "date": "2019-12-25",
        "CID": 1713,
        "source": "noovo",
        "views": 2,
        "duration": null,
        "shortTitle": "Last Christmas (2019)",
        "category": "Noovo"
      },
      {
        "date": "2019-12-25",
        "CID": 1772,
        "source": "noovo",
        "views": 2,
        "duration": 1200,
        "shortTitle": "STAR WARS：天行者的崛起",
        "category": "Action"
      },
      {
        "date": "2019-12-25",
        "CID": 1773,
        "source": "noovo",
        "views": 11,
        "duration": 300,
        "shortTitle": "HELLO WORLD",
        "category": "Animation"
      },
      {
        "date": "2019-12-25",
        "CID": 1774,
        "source": "noovo",
        "views": 12,
        "duration": null,
        "shortTitle": "捍衛戰士：獨行俠 Top Gun: Maverick",
        "category": "Action"
      },
      {
        "date": "2019-12-25",
        "CID": 1775,
        "source": "noovo",
        "views": 6,
        "duration": null,
        "shortTitle": "海綿寶寶：奔跑吧",
        "category": "Animation"
      },
      {
        "date": "2019-12-25",
        "CID": 1797,
        "source": "noovo",
        "views": 17,
        "duration": null,
        "shortTitle": "燃燒女子的畫像 Portrait of a Lady on Fire",
        "category": "Romance"
      }
    ],
    "live": [
      {
        "date": "2019-12-25",
        "name": null,
        "views": null,
        "duration": null
      }
    ],
    "ads": [
      {
        "date": "2019-12-23",
        "CID": null,
        "source": null,
        "views": null,
        "duration": null,
        "shortTitle": null,
        "category": null
      }
    ]
  }
}
```

## Client: 取得 token，作為身份認證
* Type: HTTP GET
* URL: 192.168.2.2:40000/token/new
* CURL Example  (需要在map主機，或是連接到map100電腦上執行)
```shell=
curl --location --request GET '192.168.2.2:40000/token/new'
```
* Return : JSON
```json=
{"token":"eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJhcHAiLCJleHAiOjE2MDg3OTY1MTgsImlzcyI6Ik5vb3ZvIE1lZGlhIFNlcnZpY2UifQ.jRKbdX24tlhKKP04a7O0m2fFglNzhEjLYjT7aoRei4YH9VFvDSFocdYCxoLxOcTTfG5Y6l7AWmM95Jwqm7IMRQ"}
```


## VOD Views : 看了一次電影
* Type: HTTP POST
* URL: 192.168.2.2:40000/vod/views
* Payload: json raw-data
	* time: 播放時間
	* id: VOD CID
* 注意：同一部CID，同一個時間，只會當作一次 

```json=
{ "time": "2019-12-25T05:42:24.791Z", "id": 1797, "source": "noovo"}
```
* Return: N/A
* CURL Example: 觀看 1797, 燃燒女子的畫像 ，
	* 需要在map主機，或是連接到map100電腦上執行
	* 每次測試時請修改時間點，微秒即可

```shell=
curl --location --request POST '192.168.2.2:40000/vod/views' --header 'Content-Type: application/json' --data-raw '{ "time": "2019-12-25T05:42:24.791Z", "id": 1797, "source": "noovo"}'
```


## VOD Star View: 開始看VOD
* Type: HTTP POST
* URL: 192.168.2.2:40000/vod/duration/start
* Payload: json raw-data
	* token: 請由前一個 newtoken 取得
	* id: VOD CID
	* source: noovo
* CURL Example  (需要在map主機，或是連接到map100電腦上執行)
```shell=
curl --location --request POST '192.168.2.2:40000/vod/duration/start' \
--header 'Content-Type: application/json' \
--data-raw '{
"token":"eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJhcHAiLCJleHAiOjE2MDg3OTQzODcsImlzcyI6Ik5vb3ZvIE1lZGlhIFNlcnZpY2UifQ.YOtC9XRfysFpuxJkosFeYDZJglaaTNcZ2ppjX17LPsNFSn9OyeMstjz9CqoDLyDJEpZ7FuOoJ9mvvhB2q0RM5Q", "time": "2019-12-25T07:25:24.781Z", "id": 1773, "source": "noovo" }'
```
* 無回應表示正確

## VOD End View: VOD看完了
* Type: HTTP POST
* URL: 192.168.2.2:40000/vod/duration/end
* Payload: json raw-data
	* token: 請由前一個 newtoken 取得，與duration/start 成對
	* id: VOD CID
	* source: noovo
* CURL Example  (需要在map主機，或是連接到map100電腦上執行)
```shell=
curl --location --request POST '192.168.2.2:40000/vod/duration/end' \
--header 'Content-Type: application/json' \
--data-raw '{
"token":"eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJhcHAiLCJleHAiOjE2MDg3OTQzODcsImlzcyI6Ik5vb3ZvIE1lZGlhIFNlcnZpY2UifQ.YOtC9XRfysFpuxJkosFeYDZJglaaTNcZ2ppjX17LPsNFSn9OyeMstjz9CqoDLyDJEpZ7FuOoJ9mvvhB2q0RM5Q", "time": "2019-12-25T07:25:24.781Z", "id": 1773, "source": "noovo" }'
```
* 無回應表示正確

## Live Views : 看了一次Live
* Type: HTTP POST
* URL: 192.168.2.2:40000/live/views
* Payload: json raw-data
	* time: 播放時間
	* id: 無意義
	* name: Live 名稱，唯一辨識方式
* CURL Example  (需要在map主機，或是連接到map100電腦上執行)
```shell=
curl --location --request POST 'localhost:40000/live/views' --header 'Content-Type: application/json' --data-raw '{ "time": "2019-12-25T08:11:24.785Z", "id": 0,"name": "David 告白秀 II" }'
```

## Live Star View: 開始看Live
* Type: HTTP POST
* URL: 192.168.2.2:40000/live/duration/start
* Payload: json raw-data
	* token: 請由前一個 newtoken 取得
	* id: 無意義
	* name: Live 名稱，唯一辨識方式
* CURL Example  (需要在map主機，或是連接到map100電腦上執行)
```shell=
curl --location --request POST '192.168.2.2:40000/live/duration/start' \
--header 'Content-Type: application/json' \
--data-raw '{
"token":"eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJhcHAiLCJleHAiOjE2MDg3OTQzODcsImlzcyI6Ik5vb3ZvIE1lZGlhIFNlcnZpY2UifQ.YOtC9XRfysFpuxJkosFeYDZJglaaTNcZ2ppjX17LPsNFSn9OyeMstjz9CqoDLyDJEpZ7FuOoJ9mvvhB2q0RM5Q", "time": "2019-12-25T08:11:24.785Z", "id": 0, "name": "David 告白秀 III" }'
```

## Live End View: Live看完了
* Type: HTTP POST
* URL: 192.168.2.2:40000/live/duration/end
* Payload: json raw-data
	* token: 請由前一個 newtoken 取得，與duration/start 成對
	* id: 無意義
	* name: Live 名稱，唯一辨識方式
* CURL Example  (需要在map主機，或是連接到map100電腦上執行)
```shell=
curl --location --request POST '192.168.2.2:40000/live/duration/end' \
--header 'Content-Type: application/json' \
--data-raw '{
"token":"eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJhdWQiOiJhcHAiLCJleHAiOjE2MDg3OTQzODcsImlzcyI6Ik5vb3ZvIE1lZGlhIFNlcnZpY2UifQ.YOtC9XRfysFpuxJkosFeYDZJglaaTNcZ2ppjX17LPsNFSn9OyeMstjz9CqoDLyDJEpZ7FuOoJ9mvvhB2q0RM5Q", "time": "2019-12-25T08:31:24.785Z", "id": 0, "name": "David 告白秀 III" }'
```
