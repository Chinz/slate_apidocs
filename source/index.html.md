---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# About This Guide

This document provides reference
information for the REST API requests
that you can send to FODIM RESTful API.
This document is intended for the
technical personnel who customize and
deploy this optimized solution according
to the requirements of the end customer.

# Introduction to Framework for Open Distributed Manageability

The FODIM \(Framework for Open Distributed Manageability\) RESTful API for HPE FODIM is a programming interface enabling management of HPE Telco NFV infrastructure.
FODIM combines with Redfish aggregation function to allow northbound clients to interact with the system and manage the southbound infrastructure using Redfish compliant APIs.

With modern scripting languages, you can easily write simple REST clients for RESTful APIs.

# List of supported APIs

FODIM Redfish supports the following APIs:

|API URI|Operation Applicable|
|-------|--------------------|
|/redfish|GET|
|/redfish/v1|GET|
|/redfish/v1/odata|GET|
|/redfish/v1/$metadata|GET|

|API URI|Operation Applicable|
|-------|--------------------|
|/redfish/v1/SessionService|GET|
|/redfish/v1/SessionService/Sessions|POST, GET|
|redfish/v1/SessionService/Sessions/\{session id\}|GET, DELETE|
|/redfish/v1/AccountService|GET|
|/redfish/v1/AccountService/Accounts|POST, GET|
|/redfish/v1/AccountService/Accounts/\{Account id\}|GET, DELETE, PATCH|
|/redfish/v1/AccountService/PrivilegeRegistry|GET|
|/redfish/v1/AccountService/Roles|POST, GET|
|/redfish/v1/AccountService/Roles/\{Roles id\}|GET, DELETE, PATCH|

|API URI|Operation Applicable|
|-------|--------------------|
|/redfish/v1/AggregationService/Actions/AggregationService.Add|POST|
|/redfish/v1/AggregationService/Actions/AggregationService.Remove|POST|
|/redfish/v1/AggregationService/Actions/AggregationService.Reset|POST|
|/redfish/v1/AggregationService/Actions/AggregationService.SetDefaultBootOrder|POST|

|API URI|Operation Applicable|
|-------|--------------------|
|/redfish/v1/Systems|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}/Memory|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}/MemoryDomains|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}/NetworkInterfaces|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}/EthernetInterfaces|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}/bios|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}/SecureBoot|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}/Storage|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}/Processors|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}/Memory/\{MemoryId\}|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}/EthernetInterfaces/\{Id\}|GET|
|/redfish/v1/Systems/\{ComputerSystemId\}/Processors/\{Id\}|GET|
|/redfish/v1/Chassis|GET|
|/redfish/v1/Chassis/\{ComputerSystemId\}|GET|
|/redfish/v1/Chassis/\{ComputerSystemId\}/Thermal|GET|
|/redfish/v1/Chassis/\{ComputerSystemId\}/NetworkAdapters|GET|
|/redfish/v1/Chassis/\{ComputerSystemId\}/Power|GET|
|/redfish/v1/Systems?filter=\{searchKeys\}%20\{conditionKeys\}%20\{value\}|GET|

|API URI|Operation Applicable|
|-------|--------------------|
|/redfish/v1/EventService/Subscriptions|POST|

|API URI|Operation Applicable|
|-------|--------------------|
|/redfish/v1/Fabrics|GET|
|/redfish/v1/Fabrics/\{fabricID\}|GET|
|/redfish/v1/Fabrics/\{fabricID\}/Switches|GET|
|/redfish/v1/Fabrics/\{fabricID\}/Switches/\{switchID\}|GET|
| /redfish/v1/Fabrics/\{fabricID\}/Switches/\{switchID\}/Ports<br> |GET|
| /redfish/v1/Fabrics/\{fabricID\} /Switches/\{switchID\}/Ports/\{portid\}<br> |GET|

|API URI|Operation Applicable|
|-------|--------------------|
|/redfish/v1/TaskService|GET|
|/redfish/v1/TaskService/Tasks|GET|
|/redfish/v1/TaskService/Tasks/\{TaskID\}|GET|

|API URI|Operation Applicable|
|-------|--------------------|
|/taskmon/\{TaskID\}|GET|


NOTE:

<ComputerSystemId\> is ORCA specified unique id of the server. It is represented as "UUID:<n\>" in FODIM \( UUID:<n\> Eg : ba0a6871-7bc4-5f7a-903d-67f3c205b08c:1 \)

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete