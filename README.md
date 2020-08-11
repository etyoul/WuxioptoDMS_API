# WuxioptoDMS API

## 1. Overview

본 문서는 WUXIOPTO DMS를 통해 서비스 중인 디스플레이앱에 메시지 송출하는  API의 규격을 공유하기 위해 작성되었습니다.



WUXIOPTO DMS에 등록된 가입사별 토큰을 이용해 요청할 수 있으며 각 디스플레이는 디스플레이명으로 식별합니다.



## 2. 공통

### 2.1 URL

**프로토콜은 HTTPS(SSL/TLS)이며 포트는 8011입니다.**

```http
https://display.wuxiopto.com:8011/{command}
```


### 2.2 Method

MessageAPI는 **POST**만을 사용합니다.

GET, PUT, DELETE Method는 지원하지 않습니다.


### 2.3 HTTP Header

JSON 포맷의 데이터로 요청, 응답합니다. 따라서 header의 Content-Type은 다음과 같습니다.

```
Content-Type:application/json
```


### 2.4 Request

요청은 body에 JSON 데이터로 작성합니다.


## 3. 팝업 메시지

### 3.1 URL
```http
https://display.wuxiopto.com:8011/popup
```

### 3.2 JSON DATA
```json
{
    "token": "{API 토큰}",
    "target": "{디스플 이름}",
    "message": "{메시지}"
}
```

### 3.3 요청샘플

#### 3.3.1 cURL

```curl
curl --location --request POST 'https://display.wuxiopto.com:8011/popup' \
--header 'Content-Type: application/json' \
--data-raw '{
    "token": "12345678-90ab-cdef-1234-567890abcdef",
    "target": "display12345",
    "message": "12가 3456 차량 입차확인되었습니다."
}'
```


#### 3.3.2 Java

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\r\n    \"token\": \"12345678-90ab-cdef-1234-567890abcdef\",\r\n    \"target\": \"display12345\",\r\n    \"message\": \"12가 3456 차량 입차확인되었습니다.\"\r\n}");
Request request = new Request.Builder()
  .url("https://display.wuxiopto.com:8011/popup")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
  .build();
Response response = client.newCall(request).execute();
```



#### 3.3.3 Javascript

```javascript
var data = JSON.stringify({"token":"12345678-90ab-cdef-1234-567890abcdef","target":"display12345","message":"12가 3456 차량 입차확인되었습니다."});

var xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function() {
  if(this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://display.wuxiopto.com:8011/popup");
xhr.setRequestHeader("Content-Type", "application/json");

xhr.send(data);

```



#### 3.3.4 PHP

```php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://display.wuxiopto.com:8011/popup');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Content-Type' => 'application/json'
));
$request->setBody('{
\n    "token": "12345678-90ab-cdef-1234-567890abcdef",
\n    "target": "display12345",
\n    "message": "12가 3456 차량 입차확인되었습니다."
\n}');
try {
  $response = $request->send();
  if ($response->getStatus() == 200) {
    echo $response->getBody();
  }
  else {
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  }
}
catch(HTTP_Request2_Exception $e) {
  echo 'Error: ' . $e->getMessage();
}

```



#### 3.3.5 Python

```python
import requests

url = "https://display.wuxiopto.com:8011/popup"

payload = "{\r\n    \"token\": \"12345678-90ab-cdef-1234-567890abcdef\",\r\n    \"target\": \"display12345\",\r\n    \"message\": \"12가 3456 차량 입차확인되었습니다.\"\r\n}"
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data = payload)

print(response.text.encode('utf8'))

```


## 4. 동적텍스트 (dynamic text)

### 4.1 URL
```http
https://display.wuxiopto.com:8011/dynamictext
```

### 4.2 JSON DATA
```json
{
  "token": "{API 토큰}",
  "target": "{디스플레이 이름}",
  "texts": [
    {
      "key": "{동적텍스트 key1}",
      "text": "{메시지1}"
    },
    {
      "key": "{동적텍스트 key2}",
      "text": "{메시지2}"
    },
    {
      "key": "{동적텍스트 key3}",
      "text": "{메시지3}"
    }
  ]
}
```

### 4.3 요청샘플

#### 4.3.1 cURL

```curl
curl --location --request POST 'https://display.wuxiopto.com:8011/dynamictext' \
--header 'Content-Type: application/json' \
--data-raw '{
  "token": "12345678-90ab-cdef-1234-567890abcdef",
  "target": "display12345",
  "texts": [
    {
      "key": "title",
      "text": "OO 주차장에 오신 것을 환영합니다."
    },
    {
      "key": "body",
      "text": "현재 56석의 주차공간이 있습니다"
    },
    {
      "key": "footer",
      "text": "지하 1층은 만차입니다."
    }
  ]
}'
```


#### 4.3.2 Java

```java
OkHttpClient client = new OkHttpClient().newBuilder()
  .build();
MediaType mediaType = MediaType.parse("application/json");
RequestBody body = RequestBody.create(mediaType, "{\r\n  \"token\": \"12345678-90ab-cdef-1234-567890abcdef\",\r\n  \"target\": \"display12345\",\r\n  \"texts\": [\r\n    {\r\n      \"key\": \"title\",\r\n      \"text\": \"OO 주차장에 오신 것을 환영합니다.\"\r\n    },\r\n    {\r\n      \"key\": \"body\",\r\n      \"text\": \"현재 56석의 주차공간이 있습니다\"\r\n    },\r\n    {\r\n      \"key\": \"footer\",\r\n      \"text\": \"지하 1층은 만차입니다.\"\r\n    }\r\n  ]\r\n}");
Request request = new Request.Builder()
  .url("https://display.wuxiopto.com:8011/dynamictext")
  .method("POST", body)
  .addHeader("Content-Type", "application/json")
  .build();
Response response = client.newCall(request).execute();
```



#### 4.3.3 Javascript

```javascript
var data = JSON.stringify({"token":"12345678-90ab-cdef-1234-567890abcdef","target":"display12345","texts":[{"key":"title","text":"OO 주차장에 오신 것을 환영합니다."},{"key":"body","text":"현재 56석의 주차공간이 있습니다"},{"key":"footer","text":"지하 1층은 만차입니다."}]});

var xhr = new XMLHttpRequest();
xhr.withCredentials = true;

xhr.addEventListener("readystatechange", function() {
  if(this.readyState === 4) {
    console.log(this.responseText);
  }
});

xhr.open("POST", "https://display.wuxiopto.com:8011/dynamictext");
xhr.setRequestHeader("Content-Type", "application/json");

xhr.send(data);
```



#### 4.3.4 PHP

```php
<?php
<?php
require_once 'HTTP/Request2.php';
$request = new HTTP_Request2();
$request->setUrl('https://display.wuxiopto.com:8011/dynamictext');
$request->setMethod(HTTP_Request2::METHOD_POST);
$request->setConfig(array(
  'follow_redirects' => TRUE
));
$request->setHeader(array(
  'Content-Type' => 'application/json'
));
$request->setBody('{
\n  "token": "12345678-90ab-cdef-1234-567890abcdef",
\n  "target": "display12345",
\n  "texts": [
\n    {
\n      "key": "title",
\n      "text": "OO 주차장에 오신 것을 환영합니다."
\n    },
\n    {
\n      "key": "body",
\n      "text": "현재 56석의 주차공간이 있습니다"
\n    },
\n    {
\n      "key": "footer",
\n      "text": "지하 1층은 만차입니다."
\n    }
\n  ]
\n}');
try {
  $response = $request->send();
  if ($response->getStatus() == 200) {
    echo $response->getBody();
  }
  else {
    echo 'Unexpected HTTP status: ' . $response->getStatus() . ' ' .
    $response->getReasonPhrase();
  }
}
catch(HTTP_Request2_Exception $e) {
  echo 'Error: ' . $e->getMessage();
}

```



#### 4.3.5 Python

```python
import requests

url = "https://display.wuxiopto.com:8011/dynamictext"

payload = "{\r\n  \"token\": \"12345678-90ab-cdef-1234-567890abcdef\",\r\n  \"target\": \"display12345\",\r\n  \"texts\": [\r\n    {\r\n      \"key\": \"title\",\r\n      \"text\": \"OO 주차장에 오신 것을 환영합니다.\"\r\n    },\r\n    {\r\n      \"key\": \"body\",\r\n      \"text\": \"현재 56석의 주차공간이 있습니다\"\r\n    },\r\n    {\r\n      \"key\": \"footer\",\r\n      \"text\": \"지하 1층은 만차입니다.\"\r\n    }\r\n  ]\r\n}"
headers = {
  'Content-Type': 'application/json'
}

response = requests.request("POST", url, headers=headers, data = payload)

print(response.text.encode('utf8'))


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

