---
title: API Reference

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

This API documentation is to give you an overview for connecting with the Autumn http API. The Autumn platform is expecting to receive a json string so we will look at that also.

_(Here are just a few quick tips to get you started)_

# You will need

 * Set up your **Input** in Autumn

 * Select **Data Source** "Send to Autumn API" in the Input setup

 * Make a note of your *Organisation ID* and *API KEY* for the header

 * and the *device id* to go in your json string

# Header

The header data that needs to be sent includes both an Oganisation ID and Organisation API Key.

```json
{
  "Content-Type": "application/vnd.api+json",
  "Accept": "application/vnd.api+json",
  "ORGANIZATION-ID": "{{ORG ID}}",
  "ORGANIZATION-API-KEY": "{{API KEY}}"
}
```

# JSON

The data we post to Autumn is a JSON string this is consistant no matter what langauage you are using.

```json
{
  "data": {
    "type": "SensorData",
    "id": "null",
    "attributes": {
      "value": "{{DATA VALUE}}"
    },
    "relationships": {
      "sensor": {
        "data": {
          "type": "Sensor",
          "id": "{{DEVICE ID}}"
        }
      }
    }
  }
}
```

  * Base information
  * Keep formating in this order

This should get data into the Autumn platform. You are welcome to utilise whichever language you're happy with.

### Extra data that can be sent

* Latitude and Longitude

# Endpoint

This endpoint is to post data.

### HTTP Post

`POST http://cloud.autumnapp.com`

`URI /api/external/v1/sensors/{sensor_pk}/sensordata/`

### Parameters

Parameter | Required | Location | Type | Definition
--------- | ------- | ----------- | ----- | ----------
sensor_pk | true | Path (URL) | String |
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

## Post data

```shell
curl --request POST \
  --url https://cloud.autumnapp.com/api/external/v1/api/external/v1/sensors/foo/sensordata/ \
  --header 'accept: application/vnd.api+json' \
  --header 'content-type: application/vnd.api+json' \
  --header 'organization-id: YOUR ORG ID HERE' \
  --header 'organization-api-key: YOUR API KEY HERE' \
  --data '{"x_axis_value":"foo","timestamp":"foo","longitude":"foo","related_file":"foo","value":"foo","raw_data":"foo","latitude":"foo","is_public":true,"sensor":"foo"}'
```

```javascript
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "cloud.autumnapp.com",
  "port": null,
  "path": "/api/external/v1/api/external/v1/sensors/foo/sensordata/",
  "headers": {
    "accept": "application/vnd.api+json",
    "organization-id": "YOUR ORG ID HERE",
    "organization-api-key": "YOUR API KEY HERE",
    "content-type": "application/vnd.api+json"
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

req.write(JSON.stringify({ x_axis_value: 'foo',
  timestamp: 'foo',
  longitude: 'foo',
  related_file: 'foo',
  value: 'foo',
  raw_data: 'foo',
  latitude: 'foo',
  is_public: true,
  sensor: 'foo' }));
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

	url := "https://cloud.autumnapp.com/api/external/v1/api/external/v1/sensors/foo/sensordata/"

	payload := strings.NewReader("{\"x_axis_value\":\"foo\",\"timestamp\":\"foo\",\"longitude\":\"foo\",\"related_file\":\"foo\",\"value\":\"foo\",\"raw_data\":\"foo\",\"latitude\":\"foo\",\"is_public\":true,\"sensor\":\"foo\"}")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("accept", "application/vnd.api+json")
	req.Header.Add("organization-id", "YOUR ORG ID HERE")
	req.Header.Add("organization-api-key", "YOUR API KEY HERE")
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

conn = http.client.HTTPSConnection("cloud.autumnapp.com")

payload = "{\"x_axis_value\":\"foo\",\"timestamp\":\"foo\",\"longitude\":\"foo\",\"related_file\":\"foo\",\"value\":\"foo\",\"raw_data\":\"foo\",\"latitude\":\"foo\",\"is_public\":true,\"sensor\":\"foo\"}"

headers = {
    'accept': "application/vnd.api+json",
    'organization-id': "YOUR ORG ID HERE",
    'organization-api-key': "YOUR API KEY HERE",
    'content-type': "application/vnd.api+json"
    }

conn.request("POST", "/api/external/v1/api/external/v1/sensors/foo/sensordata/", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://cloud.autumnapp.com/api/external/v1/api/external/v1/sensors/foo/sensordata/")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["accept"] = 'application/vnd.api+json'
request["organization-id"] = 'YOUR ORG ID HERE'
request["organization-api-key"] = 'YOUR API KEY HERE'
request["content-type"] = 'application/vnd.api+json'
request.body = "{\"x_axis_value\":\"foo\",\"timestamp\":\"foo\",\"longitude\":\"foo\",\"related_file\":\"foo\",\"value\":\"foo\",\"raw_data\":\"foo\",\"latitude\":\"foo\",\"is_public\":true,\"sensor\":\"foo\"}"

response = http.request(request)
puts response.read_body
```

```php
<?php

$curl = curl_init();

curl_setopt_array($curl, array(
  CURLOPT_URL => "https://cloud.autumnapp.com/api/external/v1/api/external/v1/sensors/foo/sensordata/",
  CURLOPT_RETURNTRANSFER => true,
  CURLOPT_ENCODING => "",
  CURLOPT_MAXREDIRS => 10,
  CURLOPT_TIMEOUT => 30,
  CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_POSTFIELDS => "{\"x_axis_value\":\"foo\",\"timestamp\":\"foo\",\"longitude\":\"foo\",\"related_file\":\"foo\",\"value\":\"foo\",\"raw_data\":\"foo\",\"latitude\":\"foo\",\"is_public\":true,\"sensor\":\"foo\"}",
  CURLOPT_HTTPHEADER => array(
    "accept: application/vnd.api+json",
    "content-type: application/vnd.api+json",
    "organization-id: YOUR ORG ID HERE",
    "organization-api-key: YOUR API KEY HERE"
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
  "timestamp": "foo",
  "longitude": "foo",
  "latitude": "foo",
  "value": "foo",
  "sensor": "foo"
}
```
