---
title: Datacooee API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript
  - python
  - ruby
  - php
  - go

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:


search: true
---

# Introduction

This API documentation is to give you an overview for connecting inputs to the Datacooee http API.

We will cover:

* Header details
* json string

_(These are just a few tips to get you started. We will be expanding the API over time)_

# You will need

 * Set up your **Input** in Datacooee

 * Select **Data Source** "API" in the Input setup

 * Make a note of your *Organisation ID* and *Organisation API Key* for the header

 * For json string you will need the next:
   - *input id* - id of your created input
   - *value* - required field for uploaded value
   - *timestamp* - required field for uploading time. Time string with ISO format
   - *latitude* - extra field for uploading latitude value
   - *longitude* - extra field for uploading latitude value

# Header

The header data that needs to be sent includes both an Organisation ID and Organisation API Key.
Example:
ORGANIZATION_ID = "ZnlvcWXXXXXXXXXXXXXXXXXXXX5ieXVieHgudng="
ORGANIZATION-API-KEY = "f13d34bxxxxxxxxxxxxxxxxxxxa5da135d61"

```json
{
  "Content-Type": "application/vnd.api+json",
  "ORGANIZATION-ID": "{{ ORGANIZATION_ID}}",
  "ORGANIZATION-API-KEY": "{{ ORGANIZATION-API-KEY }}"
}
```

# JSON

The data we post to Datacooee is a JSON string this is consistent no matter what language you are using.

Example:
DATA_VALUE = 12.5
TIMESTAMP = "2020-10-13T14:02:39.489Z"
LATITUDE_VALUE = 40.730610
LONGITUDE_VALUE = -73.935242
INPUT_ID = "senxxxxxxxxxx"

```json
{
  "data": {
    "type": "SensorData",
    "id": "null",
    "attributes": {
      "value": "{{ DATA_VALUE }}",
      "timestamp": "{{ TIMESTAMP }}",
      "latitude": "{{ LATITUDE_VALUE }}",
      "longitude": "{{ LONGITUDE_VALUE }}"
    },
    "relationships": {
      "sensor": {
        "data": {
          "type": "Sensor",
          "id": "{{ INPUT_ID }}"
        }
      }
    }
  }
}
```

  * Base information
  * Keep formatting in this order

You should now have data going into the Datacooee platform. You are welcome to utilise whichever language you're happy with.

# Endpoint

This endpoint is to post data.

### HTTP Post

Upload data into the Datacooee platform.

`POST http://cloud.mydata.management`

`URI /api/external/v1/inputs/{input_pk}/inputdata/`

### Parameters

Parameter | Required | Location | Type | Definition
--------- | ------- | ----------- | ----- | ----------
input_pk | true | Path (URL) | String |
data | true | Request Body | Object | Sensor Data

### Responses

Parameter | http code | Content
--------- | ----------- | --------
created | 201 | Empty

### Definitions

Parameter | Required | Location | Type
--------- | ------- | ----------- | -----
timestamp | true | Request Body | String
longitude | false | Request Body | String
latitude | false | Request Body | String
value | true | Request Body | String
sensor | true | Request Body | String

# Examples

Here's are some example snippets to get to started.
Examples use mocked data below.
ORGANIZATION_ID = "ZnlvcWXXXXXXXXXXXXXXXXXXXX5ieXVieHgudng="
ORGANIZATION-API-KEY = "f13d34bxxxxxxxxxxxxxxxxxxxa5da135d61"
DATA_VALUE = 12.5
TIMESTAMP = "2020-10-13T14:02:39.489Z"
INPUT_ID = "senxxxxxxxxxx" 

```shell
curl --request POST \
  --url https://cloud.mydata.management/api/external/v1/inputs/senxxxxxxxxxx/inputdata/ \
  --header 'content-type: application/vnd.api+json' \
  --header 'organization-id: ZnlvcWXXXXXXXXXXXXXXXXXXXX5ieXVieHgudng=' \
  --header 'organization-api-key: f13d34bxxxxxxxxxxxxxxxxxxxa5da135d61' \
  --data '{"data":{"type":"SensorData", "id": "null", "attributes": { "value": "12.5", "timestamp": "2020-10-13T14:02:39.489Z"}, "relationships": { "sensor": { "data": { "type": "Sensor", "id": "senxxxxxxxxxx" }}}}}'
```

```javascript
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "cloud.mydata.management",
  "port": null,
  "path": "/api/external/v1/inputs/senxxxxxxxxxx/inputdata/",
  "headers": {
    "organization-id": "ZnlvcWXXXXXXXXXXXXXXXXXXXX5ieXVieHgudng=",
    "organization-api-key": "f13d34bxxxxxxxxxxxxxxxxxxxa5da135d61",
    "content-type": "application/vnd.api+json"
  }  
};

var data = {
  "data": {
    "type": "SensorData",
    "id": "null",
    "attributes": {
      "value": "12.5",
      "timestamp": "2020-10-13T14:02:39.489Z"
    },
    "relationships": {
      "sensor": {
        "data": {
          "type": "Sensor",
          "id": "senxxxxxxxxxx"
        }
      }
    }
  }
};


var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify(data));
req.end();
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://cloud.mydata.management/api/external/v1/inputs/senxxxxxxxxxx/inputdata/"

	payload := strings.NewReader('{"data":{"type":"SensorData", "id": "null", "attributes": { "value": "12.5", "timestamp": "2020-10-13T14:02:39.489Z"}, "relationships": { "sensor": { "data": { "type": "Sensor", "id": "senxxxxxxxxxx" }}}}}')

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("accept", "application/vnd.api+json")
	req.Header.Add("organization-id", "ZnlvcWXXXXXXXXXXXXXXXXXXXX5ieXVieHgudng=")
	req.Header.Add("organization-api-key", "f13d34bxxxxxxxxxxxxxxxxxxxa5da135d61")
	req.Header.Add("content-type", "application/vnd.api+json")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

```python
import http.client

conn = http.client.HTTPSConnection("cloud.mydata.management")

payload = '{"data":{"type":"SensorData", "id": "null", "attributes": { "value": "12.5", "timestamp": "2020-10-13T14:02:39.489Z"}, "relationships": { "sensor": { "data": { "type": "Sensor", "id": "senxxxxxxxxxx" }}}}}'

headers = {
    'accept': "application/vnd.api+json",
    'organization-id': "ZnlvcWXXXXXXXXXXXXXXXXXXXX5ieXVieHgudng=",
    'organization-api-key': "f13d34bxxxxxxxxxxxxxxxxxxxa5da135d61",
    'content-type': "application/vnd.api+json"
    }

conn.request("POST", "/api/external/v1/inputs/senxxxxxxxxxx/inputdata/", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://cloud.mydata.management/api/external/v1/inputs/senxxxxxxxxxx/inputdata/")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["accept"] = 'application/vnd.api+json'
request["organization-id"] = 'ZnlvcWXXXXXXXXXXXXXXXXXXXX5ieXVieHgudng='
request["organization-api-key"] = 'f13d34bxxxxxxxxxxxxxxxxxxxa5da135d61'
request["content-type"] = 'application/vnd.api+json'
request.body = '{"data":{"type":"SensorData", "id": "null", "attributes": { "value": "12.5", "timestamp": "2020-10-13T14:02:39.489Z"}, "relationships": { "sensor": { "data": { "type": "Sensor", "id": "senxxxxxxxxxx" }}}}}'

response = http.request(request)
puts response.read_body
```

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://cloud.mydata.management/api/external/v1/inputs/senxxxxxxxxxx/inputdata/",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => '{"data":{"type":"SensorData", "id": "null", "attributes": { "value": "12.5", "timestamp": "2020-10-13T14:02:39.489Z"}, "relationships": { "sensor": { "data": { "type": "Sensor", "id": "senxxxxxxxxxx" }}}}}',
  CURLOPT_HTTPHEADER => array(
    "accept: application/vnd.api+json",
    "content-type: application/vnd.api+json",
    "organization-id: ZnlvcWXXXXXXXXXXXXXXXXXXXX5ieXVieHgudng=",
    "organization-api-key: f13d34bxxxxxxxxxxxxxxxxxxxa5da135d61"
  ),
));

$response = curl_exec($curl);
$err = curl_error($curl);

curl_close($curl);

if ($err) {
  echo "cURL Error #:" . $err;
} else {
  echo $response;
}
```

> The above command returns JSON structured like this:

```json
{
    "data": {
        "type": "SensorData",
        "id": "123456789",
        "attributes": {
            "timestamp": "2020-10-13T14:02:39.489000Z",
            "value": "12.5"
        },
        "relationships": {
            "sensor": {
                "data": {
                    "type": "Sensor",
                    "id": "senxxxxxxxxxx"
                }
            }
        }
    }
}
```
