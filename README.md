# WuxioptoDMS API

## 1. Overview

본 문서는 WUXIOPTO DMS를 통해 서비스 중인 디스플레이앱에 메시지 송출하는  API의 규격을 공유하기 위해 작성되었습니다.



WUXIOPTO DMS에 등록된 가입사별 토큰을 이용해 요청할 수 있으며 각 디스플레이는 디스플레이명으로 식별합니다.



## 2. 공통

### 1) URL

**프로토콜은 HTTPS(SSL/TLS)이며 포트는 8011입니다.**

```http
https://display.wuxiopto.com:8011/{command}
```


### 2) Method

MessageAPI는 **POST**만을 사용합니다.

GET, PUT, DELETE Method는 지원하지 않습니다.


### 3) HTTP Header

JSON 포맷의 데이터로 요청, 응답합니다. 따라서 header의 Content-Type은 다음과 같습니다.

```
Content-Type:application/json
```


### 4) Request

요청은 body에 JSON 데이터로 작성합니다.



### 5.1 요청샘플

#### 5.1.1 cURL

```curl
curl -X POST -H "Content-Type: application/json" 
  -d '{"target":"display15982", "message":"주차장에 오신 것을 환영합니다.\n00거 0000차량이 입차되었습니다."}'
  "http://localhost:9496/display/message"
```



#### 5.1.2 Java

```java
OkHttpClient client = new OkHttpClient();

MediaType mediaType = MediaType.parse("application/json");

RequestBody body = RequestBody.create(mediaType, "{\"target\":\"display15982\", \"message\":\"주차장에 오신 것을 환영합니다.\\n00거 0000차량이 입차되었습니다.\"}");

Request request = new Request.Builder()
 .url("http://localhost:9496/display/message")
 .post(body)
 .addHeader("content-type", "application/json")
 .build();
 
Response response = client.newCall(request).execute();
```



#### 5.1.3 Javascript

```javascript
var data = JSON.stringify({
  "target": "display15982",
  "message": "주차장에 오신 것을 환영합니다.\n00거 0000차량이 입차되었습니다."
});

var xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function () {
  if (this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "http://localhost:9496/display/message");
xhr.setRequestHeader("content-type", "application/json");

xhr.send(data);

```



#### 5.1.4 PHP

```php
<?php

$request = new HttpRequest();
$request->setUrl('http://localhost:9496/display/message');
$request->setMethod(HTTP_METH_POST);

$request->setHeaders(array(
  'content-type' => 'application/json'
));

$request->setBody('{"target":"display15982", "message":"주차장에 오신 것을 환영합니다.\\n00거 0000차량이 입차되었습니다."}');

try {
  $response = $request->send();

  echo $response->getBody();
} catch (HttpException $ex) {
  echo $ex;
}

```



#### 5.1.5 Python

```python
import requests

url = "http://localhost:9496/display/message"

payload = "{\"target\":\"display15982\", \"message\":\"주차장에 오신 것을 환영합니다.\\n00거 0000차량이 입차되었습니다.\"}"
headers = {
    'content-type': "application/json"
    }

response = requests.request("POST", url, data=payload, headers=headers)

print(response.text)
```



## 6. Response

JSON포맷이며 키는 다음과 같습니다.

* result: success 또는 failure
* resultCode: 0000(성공), 그 외는 실패
* errorMessage: resultCode가 0000이 아닌 경우 오류 메시지가 포함됩니다.



### 6.1 응답예시

#### 6.1.2 성공

```json
{
    "result": "success",
    "resultCode": "0000",
    "errorMessage": ""
}
```



#### 6.1.2 잘못된 데이터로 요청한 경우

```json
{
    "result": "failure",
    "resultCode": "9001",
    "errorMessage": "잘못된 요청입니다."
}
```



#### 6.1.3 등록된 디스플레이가 없는 경우

```json
{
    "result": "failure",
    "resultCode": "1001",
    "errorMessage": "display0000 디스플레이가 없습니다."
}
```



#### 6.1.4 디스플레이가 오프라인인 경우

```json
{
    "result": "failure",
    "resultCode": "1002",
    "errorMessage": "display0000 디스플레이가 연결되어 있지 않습니다."
}
```

