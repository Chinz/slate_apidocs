---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - curl
  


toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  

search: true
---
# About This Guide

This document provides helpful reference
information for the REST API requests
that you can send to FODIM RESTful API.
This document is intended for the
technical personnel who customize and
deploy this optimized solution according
to the requirements of the end customer.


# Introduction to Framework for Open Distributed Manageability

  Managing a multitude of IT infrastructure devices of different make and type is something of a task, especially, when they exist across multiple data centers.

The FODIM \(Framework for Open Distributed Manageability\) RESTful API offers a simple and effective solution which significantly reduces this workload: it virtually brings all the devices(compute, storage, and fabric) to be managed in one place with the help of Redfish compliant APIs and vendor specific plugins.
 
 The FODIM RESTful API is a programming interface enabling easy and secure management of wide range of IT infrastructure equipment distributed across multiple data centers.

The RESTful APIs exposed by the FODIM are designed as per Redfish Scalable Platforms API specification v1.4.0 DSP0266.

## Key benefits of FODIM

- **Simplifies lifecycle management of southbound infrastructure**: FODIM allows you to group southbound resources into one aggregate and modify them collectively. It also performs a detailed inventory of southbound resources and offers an aggregated access it. 

- **Scalable**: leveraging the resource aggregation capability of FODIM, you can manage a wide range of different southbound devices(from stand-alone servers to rack mount and bladed environments to large scale server environments).

- **Simplifies interaction of northbound clients with southbound systems**: FODIM offers a protocol independent eventing mechanism using which northbound clients get notified of alarms from southbound equipment.

##  FODIM logical architecture

The FODIM based on Redfish standards uses RESTful API to create an environment that is designed to be implemented on many different models of servers and other IT infrastructure devices for years to come. These devices may be quite different from one another. For this reason, the Redfish API does not specify the URIs to various resources.



# Using the FODIM RESTful API

The FODIM RESTful API is available on version __ or later.

To access the RESTful API, you need an HTTPS-capable client, such as a web browser with a REST Client plugin extension, or a Desktop REST Client application (such as Postman), or cURL (a popular, free command line utility).
You can also easily write simple REST clients for RESTful APIs using modern scripting languages.

**Tip:**
It is good to use a tool, such as cURL or Postman initially to perform requests. Later, you will want to write your own scripting code to do this. 

**Note:**
The examples shown in this document use cURL to make HTTP requests.

Curl is a command line tool which helps you get or send information through URLs using supported protocols(`HTTP` in the case of FODIM).
It is available at http://curl.haxx.se/ .

All the cURL examples use the following options(flags):
-    `--insecure` 
       bypasses validation of the HTTPS certificate. In actual usage, the FODIM RESTful API should be configured to use a  user-supplied  certificate and this option is not necessary. 
	   
-    `-i` 
       returns HTTP response headers.


#  List of supported APIs

FODIM Redfish supports the following APIs:

|URI|Operation Applicable|
|-------|--------------------|
|/redfish|GET|
|/redfish/v1|GET|
|/redfish/v1/odata|GET|
|/redfish/v1/$metadata|GET|
|/redfish/v1/SessionService|GET|
|/redfish/v1/SessionService/Sessions|POST, GET|
|/redfish/v1/SessionService/Sessions/\{session id\}|GET, DELETE|
|/redfish/v1/AccountService|GET|
|/redfish/v1/AccountService/Accounts|POST, GET|
|/redfish/v1/AccountService/Accounts/\{Account id\}|GET, DELETE, PATCH|
|/redfish/v1/AccountService/PrivilegeRegistry|GET|
|/redfish/v1/AccountService/Roles|POST, GET|
|/redfish/v1/AccountService/Roles/\{Roles id\}|GET, DELETE, PATCH|
|/redfish/v1/AggregationService/Actions/AggregationService.Add|POST|
|/redfish/v1/AggregationService/Actions/AggregationService.Remove|POST|
|/redfish/v1/AggregationService/Actions/AggregationService.Reset|POST|
|/redfish/v1/AggregationService/Actions/AggregationService.SetDefaultBootOrder|POST|
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
|/redfish/v1/EventService/Subscriptions|POST|
|/redfish/v1/Fabrics|GET|
|/redfish/v1/Fabrics/\{fabricID\}|GET|
|/redfish/v1/Fabrics/\{fabricID\}/Switches|GET|
|/redfish/v1/Fabrics/\{fabricID\}/Switches/\{switchID\}|GET|
| /redfish/v1/Fabrics/\{fabricID\}/Switches/\{switchID\}/Ports<br> |GET|
| /redfish/v1/Fabrics/\{fabricID\} /Switches/\{switchID\}/Ports/\{portid\}<br> |GET|
|/redfish/v1/TaskService|GET|
|/redfish/v1/TaskService/Tasks|GET|
|/redfish/v1/TaskService/Tasks/\{TaskID\}|GET|
|/taskmon/\{TaskID\}|GET|


**NOTE:**

*ComputerSystemId is FODIM specified unique id of the server. It is represented as "{UUID}:{n}" in FODIM \( Example : ba0a6871-7bc4-5f7a-903d-67f3c205b08c:1 \)


#  HTTP Request Methods, Responses, and Error Codes for FODIM REST API

Following are the Redfish defined HTTP methods that you can use to implement various actions:

|HTTP Request Methods|Description|
|--------------------|-----------|
|`GET` \[Read Requests\]|Use this method to request a representation of a specified resource \(single resource or collection\).|
|`PATCH` \[Update\]|Use this method to apply partial modifications to a resource.|
|`POST` \[Create\] \[Actions\]|Use this method to create a new resource. Submit this request to the resource collection to which you want to add the new resource. You can also use this method to initiate operations on an object.|
|`DELETE` \[Delete\]|Use this method to delete a resource.|

FODIM Redfish supports the following responses:

|Responses|Description|
|---------|-----------|
|Metadata responses|Describes the resources and types exposed by the service to generic clients.|
|Resource responses|Response in JSON format for an individual resource.|
|Resource collection responses|Response in JSON format for a collection of a resources.|
|Error responses|In case of an HTTP error, a high level JSON response is provided with additional information|

Following are the HTTP Status codes with their descriptions:

|Status Code<br> |Description|
|----------------|-----------|
|200 OK|Successful completion of the request with representation in the body|
|201 Created|A new resource is successfully created with the Location header set to well-defined URI for the newly created resource. The response body includes the representation of the newly created resource.|
|202 Accepted|The request has been accepted for processing but not processed. The Location header is set to URI of a task monitor that can be queried later for the status of the operation.|
|204 No content|The request was a success but no content was returned in the body of the response.|
|301 Moved Permanently|The requested resource resides in a different URI given by the Location headers.|
|302 Found|The requested resource temporarily resides in a different URI.|
|304 Not Modified|When the service has performed conditional GET request but the resource content has not changed.|
|400 Bad Request|The request could not be performed due to missing or invalid information. An extended error message is returned in the response body.|
|401 Unauthorized|Missing or invalid authentication credentials included with a request.|
|403 Forbidden|The server recognizes the credentials to be not having the necessary authorization to perform the operation.|
|404 Not Found|The request specified the URI of a non-existing resource.|
|405 Method Not Allowed|When the HTTP verb specified in the request \(GET, PATCH, DELETE, and so on\) is not supported for a particular request URI. The response includes "Allow" header that lists the supported methods.|
|406 Not Acceptable|When the resource fails to generate a response that matches the media types specified in the Accept header.|
|409 Conflict|A creation or an update cannot be completed because it would conflict with the current state of the resources supported by the platform.|
|410 Gone|When the requested resource is no longer available in the server and there is no forwarding address available as well. This condition is expected to be considered permanent. Clients with hyperlink editing capabilities SHOULD delete references to the Request-URI after user approval. If the server does not know, or has no facility to determine, whether or not the condition is permanent, the status code 404 \(Not Found\) SHOULD be used instead. This response is cacheable unless indicated otherwise.|
|411 Length Required|When the content of the length is not specified using the Content-Length header.|
|412 Precondition Failed|When pre-condition \(OData-version\) checks fail.|
|415 Unsupported Media Type|When the Content-type specified for the body does not match.|
|428 Precondition Required|When the request does not provide the pre-condition such as an If-Match|
|431 Request Header Field Too Large|When the individual header or all the header fields in a request are too large, the server does not fulfils the request.|
|500 Internal Server Error|When the server encounters and unexpected condition that prevents it from fulfilling the request.|
|501 Not Implemented|When the server does not recognize or supports the method for any resource.|
|503 Service Unavailable|When the server is currently unable to service the request due to temporary overloading or maintenance.|
|507 Insufficient Storage|When the size of the response is too big for the server to build the response.|



#  Authenticating requests when using the FODIM REST APIs

 Following authentication methods can be implemented to authenticate requests to the FODIM REST API:

-   **HTTP BASIC authentication**

    You can implement HTTP BASIC authentication by providing base64 encoded FODIM user name and password in an HTTP `Authorization: Basic` header \( Basic Auth\).

-   **Redfish session login authentication**

    You can implement Redfish Session Login Authentication by creating a Redfish login session through session management interface. To learn more about a session, See [Creating and managing sessions](#) section.

    Every session that is created has an authentication token called `X-AUTH-TOKEN` associated with it.

    To authenticate subsequent requests, provide this token in the `X-AUTH-TOKEN` request header.


    NOTE:
     -  An `X-AUTH-TOKEN` is valid and a session is open only for 30 minutes, unless the user continues to send requests to a Redfish service using this token.

     -  An idle session is automatically terminated after the timeout interval.



NOTE:

-   When Basic Auth is used, creating a session is not required.

-   You must authenticate all write requests\(POST, PATCH, and DELETE \) on the resources except for The POST operation on the Sessions service/object needed for authentication.

-   You must authenticate all read requests\(GET \) on the resources except for:

     -   The service root

     -   The $metadata

     -   The OData Service Document

     -   The Redfish version



# Creating and managing sessions

A session represents a window of user's login with a Redfish service and contains details about the user and the user activity. You can run multiple sessions simultaneously on FODIM.

FODIM offers Redfish SessionService interface for managing sessions and exposes APIs to achieve the following:

-   Fetching the SessionService root

-   Creating a session

-   Listing active sessions

-   Deleting a session


##  Supported APIs

-   `/redfish/v1/SessionService`

-   `/redfish/v1/SessionService/Sessions`

-   `/redfish/v1/SessionService/Sessions/{session id}`




#  Configuring roles and privileges for a user

FODIM supports role-based authorization of requests - the roles and privileges control which users have what access to resources.

##  Roles

A role represents a set of operations that a user is allowed to perform and is determined by a defined set of privileges. The privileges of a role are configurable - you can choose a privilege or a set of privileges to be assigned to a role at the time of role creation.

See [Creating a role](#).

With FODIM, there are two kinds of defined roles:

-   **Redfish pre-defined roles**

    Redfish pre-defined roles come with a pre-defined set of privileges. Following are the Redfish pre-defined roles:

    -   Administrator

    -   Operator

    -   ReadOnly

-   **OEM\(Original Equipment Manufacturer\) roles**

    OEM roles are the custom roles that you can create and assign to a user. Following are the available OEM roles:

    -    admin
 :   The admin is an OEM pre-defined role that has all the privileges needed to manage all services. The admin can perform any actions such as adding and removing equipments, adding or removing users or privileges, and so on.

    -    Client
 :   The client is an OEM role that uses the northbound APIs to manage the southbound infrastructure.

    -    Monitor
 :   The Monitor is an OEM role that is used typically by northbound monitoring solutions, this user role performs actions such as setting up subscriptions to be notified on Alerts from southbound infrastructure.



**NOTE:**

-   Every user is assigned one role at the time of user account creation; ensure that the role to be assigned to a user is created before creating a user account.

-   The Redfish pre-defined roles can include OEM privileges.

-   OEMs can create custom roles and assign the same to a user at the time of user creation.



##  Privileges

A privilege is a permission to perform an operation or a set of operations within a defined management domain. There are Redfish specified set of privileges as well as OEM-defined privileges.

The following privileges can be assigned to any user in FODIM:

-    ConfigureComponents
 :   Users with this privilege can configure components managed by the services in FODIM Redfish API.

-    ConfigureManager
 :   Users with this privilege can configure manager resources.

-    ConfigureSelf
 :   Users with this privilege can change the password for their account.

-    ConfigureUsers
 :   Users with this privilege can configure users and their accounts. This privilege is assigned to an administrator.

-    Login
 :   Users with this privilege can log into the service and read the resources.



**NOTE:**

The privileges set for a Redfish pre-defined role cannot be modified.


##  Mapping of privileges to roles in FODIM

|Roles|Assigned privileges|
|-----|-------------------|
|Administrator \(Redfish pre-defined\)| -   Login<br>-   ConfigureManager<br>-   ConfigureUsers<br>-   ConfigureComponents<br>-   ConfigureSelf<br>
 |
|Operator \(Redfish pre-defined\)| -   Login<br>-   ConfigureComponents<br>-   ConfigureSelf<br>
 |
|ReadOnly \(Redfish pre-defined\)| -   Login<br>-   ConfigureSelf<br>
 |
|admin \(OEM\)| |
|Client \(OEM\)| |
|Monitor \(OEM\)| |

#  Creating and managing user accounts

FODIM allows users to have accounts to configure their actions and restrictions.

A user account contains the following details:

-   User name

-   Password

-   A role assigned to the user.


It exposes Redfish AccountsService APIs to manage user accounts. Use these endpoints to perform the following operations:

-   Creating, modifying, and deleting account details.

-   Fetching account details.


**Supported APIs**:

-   `/redfish/v1/AccountService`

-   `/redfish/v1/AccountService/Accounts`

-   `/redfish/v1/AccountService/Accounts/{Account Id}`

-   `/redfish/v1/AccountService/Roles`

-   `/redfish/v1/AccountService/Roles/{Role id}`


##  Authentication

X-AUTH-TOKEN authentication or Basic authentication.



#  Aggregating and managing southbound infrastructure

One of the state-of-the-art features of FODIM is that it allows users to add and group southbound infrastructure\(servers, storage, or fabrics\) into one aggregate for easy manageability. It offers Redfish AggregationService as an interface and exposes endpoints to achieve the following:

-   Adding a resource in the inventory to manage.

-   Resetting one or more resources.

-   Changing the boot path of one or more resources to default settings.

-   Removing a resource from the inventory which is no longer managed.


Using these endpoints, you can add or remove only one resource at a time. You can group the resources into one collection and perform actions such as reset and setdefaultbootorder in combination on that group.

The next section contains the use case for server aggregation.

##  Prerequisites

-   To access Redfish AggregationService endpoints, ensure that the user has `ConfigureComponents` privilege.


##  Authentication

X-AUTH-TOKEN authentication or Basic authentication.

##  Supported Endpoints

-   `/redfish/v1/AggregatorService/Actions/AggregationService.Add`

-   `/redfish/v1/AggregatorService/Actions/AggregationService.Remove`

-   `/redfish/v1/AggregationService/Actions/AggregationService.Reset`

-   `/redfish/v1/AggregationService/Actions/AggregationService.SetDefaultBootOrder`

## The aggregation service root



|**Method**|`GET` |
|-----|-------------------|
|**URI** |`redfish/v1/AggregationService` |
|**Description** |The URI for the Aggregation service root.|
|**Returns** |Properties for the service and a list of actions you can perform using this service.|
|**Response Code** |`200` on success|

 

### Usage 

```
curl --insecure -X GET \
   -H "X-Auth-Token:{X-Auth-Token}" \
 'https://{FODIM_IP_address}/redfish/v1/AggregationService'


```

### Sample Request body 

None

### Sample Response header 

```
allow:	GET
cache-control:	no-cache
content-length:	1 kilobytes
content-type:	application/json; charset=utf-8
date:	Mon, 18 Nov 2019 14:52:21 GMT+5h 37m
etag:	W/"12345"
link:	/v1/SchemaStore/en/AggregationService.json>; rel=describedby
odata-version:	4.0
status:	200
x-frame-options:	sameorigin

```

### Sample Response body 

```
{ 
   "@odata.context":"/redfish/v1/$metadata#AggregationService.AggregationService",
   "@odata.etag":"W/\"979B45E7\"",
   "Id":"AggregationService",
   "@odata.id":"/redfish/v1/AggregationService",
   "@odata.type":"#AggregationService.v1_0_0.AggregationService",
   "Name":"AggregationService",
   "Description":"AggregationService",
   "Actions":{ 
      "#AggregationService.Add":{ 
         "target":"/redfish/v1/AggregationService/Actions/AggregationService.Add/",
         "@Redfish.ActionInfo":"/redfish/v1/AggregationService/AddActionInfo"
      },
      "#AggregationService.Remove":{ 
         "target":"/redfish/v1/AggregationService/Actions/AggregationService.Remove/",
         "@Redfish.ActionInfo":"/redfish/v1/AggregationService/RemoveActionInfo"
      },
      "#AggregationService.Reset":{ 
         "target":"/redfish/v1/AggregationService/Actions/AggregationService.Reset/",
         "@Redfish.ActionInfo":"/redfish/v1/AggregationService/ResetActionInfo"
      },
      "#AggregationService.SetDefaultBootOrder":{ 
         "target":"/redfish/v1/AggregationService/Actions/AggregationService.SetDefaultBootOrder/",
         "@Redfish.ActionInfo":"/redfish/v1/AggregationService/SetDefaultBootOrderActionInfo"
      }
   },
   "ServiceEnabled":true,
   "Status":{ 
      "Health":"OK",
      "HealthRollup":"OK",
      "State":"Enabled"
   }
}
```



# Use case for server aggregation

##  Adding a Server

|**Method** |`POST`|
|-----|-------------------|
|**URI**|`/redfish/v1/AggregatorService/Actions/AggregationService.Add` |
|**Returns** |Location URI of the task monitor associated with this operation in the response header.|
|**Response Code** |`202` on success|

### Description

This action adds a single server in the inventory.
On success, ORCA specified Computer System Id is assigned to the added server.

This action is performed as a task. To know the status of the progress of this action, perform GET on the URI of the task monitor returned in the Location header. See [How to monitor a task](#). If the server is successfully added in the inventory, its endpoint having the System Id is returned as response.


**NOTE:**

Make note of the Computer System Id as it is required to perform subsequent actions such as delete, reset, and setdefaultbootorder on this server.





###  Usage

```
curl -i --insecure -X POST \
   -H "X-Auth-Token:{X-Auth-Token}" \
   -H "Content-Type:application/json" \
   -d \
'{"ManagerAddress":"{Server_IP_Address}","UserName":"{Account_UserName}","Password":"{Account_Password}","Oem":{"PluginID":"{Redfish_PluginId}"}}' \
 'https://{FODIM_IP_address}/redfish/v1/AggregationService/Actions/AggregationService.Add'


```

### Sample Request body

```
{
	"ManagerAddress": "10.24.0.6",
	"Username": "username",
	"Password": "password",
	"Oem": {
		"PluginID": "GRF"
	}
}
```

### Request Parameters

|Parameter|Type|Description|
|---------|----|-----------|
|ManagerAddress| |A valid IP address or hostname of a baseboard management controller \(BMC\)|
|Username|string|The username for this user account|
|Password|string|The password for this user account|
|PluginID|string|The plugin id Example : GRF \(Generic Redfish Plugin\)<br> |

###  Sample Response header

```
content-length:0 byte
content-type:application/json; charset=utf-8
date:Thu,
07 Nov 2019 09:29:36 GMT+7m 8s
**location:/taskmon/taskb297ff4e-0d2d-485c-8217-63d888cba7d2**
odata-version:4.0
status:202
x-frame-options:sameorigin
```

###  Sample Response body

None

## Resetting one or more servers

|**Method** |`POST` |
|-----|-------------------|
|**URI** |`/redfish/v1/AggregatorService/Actions/AggregationService.Reset` |
|**Returns** |Location URI of the Task Monitor associated with this operation in the response header.|
|**Response Code** |`202` on success|

###  Description

This action shuts down, powers up, and restarts one or more servers.

You can perform reset on a group of servers by specifying multiple target URIs in the request.

To know the status of the progress of this operation, perform GET on the URI of the task monitor returned in the Location header. See [How to monitor a task](#). If the servers are successfully reset, 200 response code along with a success message in the response body is returned.

###  Usage

```
curl --insecure -X POST \
   -H "X-Auth-Token:{X-Auth-Token}" \
   -H "Content-Type:application/json" \
   -d \
'{
"@Redfish.Copyright": "Copyright 2018-2019 DMTF. All rights reserved.",
"@odata.context": "/redfish/v1/$metadata#ActionInfo.ActionInfo",
"@odata.id": "/redfish/v1/AggregationService/ResetActionInfo",
"@odata.type": "#ActionInfo.v1_0_3.ActionInfo",
"Id": "AddActionInfo",
"Name": "Reset Action Info",

 "Oem": {

  },

"parameters": {
"ResetCollection": {
"description": "Collection of ResetTargets",
"ResetTarget": [{
"ResetType": "ForceRestart",
"TargetUri": "/redfish/v1/Systems/{ComputerSystemId}"
},
{
"ResetType": "ForceOff",
"TargetUri": "/redfish/v1/Systems/{ComputerSystemId}"
}
]
}
}
}' \
 'https://{FODIM_IP_address}/redfish/v1/AggregationService/Actions/AggregationService.Reset'


```

###  Sample Request body

```
{ 
   "@Redfish.Copyright":"Copyright 2018-2019 DMTF. All rights reserved.",
   "@odata.context":"/redfish/v1/$metadata#ActionInfo.ActionInfo",
   "@odata.id":"/redfish/v1/AggregationService/ResetActionInfo",
   "@odata.type":"#ActionInfo.v1_0_3.ActionInfo",
   "Id":"AddActionInfo",
   "Name":"Reset Action Info",
   "Oem":{ 

   },
   "parameters":{ 
      "ResetCollection":{ 
         "description":"Collection of ResetTargets",
         "ResetTarget":[ 
            { 
               "ResetType":"ForceRestart",
               "TargetUri":"/redfish/v1/Systems/97d08f36-17f5-5918-8082-f5156618f58d:1"
            },
            { 
               "ResetType":"ForceOff",
               "TargetUri":"/redfish/v1/Systems/24b243cf-f1e3-5318-92d9-2d6737d6b0b9:1"
            }
         ]
      }
   }
}

```

###  Request Parameters

|Parameter|Type|Description|
|---------|----|-----------|
|@odata.context|string|Refer to Common Redfish Schema parameters section.|
|@odata.id|string|Refer to Common Redfish Schema parameters section.|
|@odata.type|string|Refer to Common Redfish Schema parameters section.|
|Id|string|Refer to Common Redfish Schema parameters section.|
|Name|string|Refer to Common Redfish Schema parameters section.|
|Oem|object|Refer to Common Redfish Schema parameters section.|
|parameters\[ \{|array|The parameters associated with the Reset Action.|
|ResetType|string|The type of reset to be performed. See "Reset Type" section below, for the possible values of this property.|
|TargetUri \} \] |string|The URI of the target for Reset - `"/redfish/v1/Systems/<ComputerSystemId>"` |

###  Reset Type

|string|Description|
|------|-----------|
|ForceOff|Turn the unit off immediately \(non-graceful shutdown\).|
|ForceOn|Turn the unit on immediately.|
|ForceRestart|Perform an immediate \(non-graceful\) shutdown, followed by a restart.|
|GracefulRestart|Perform a graceful shutdown followed by a restart of the system.|
|GracefulShutdown|Perform a graceful shutdown. Graceful shutdown involves shutdown of the operating system followed by the power off of the physical server.|
|Nmi|Generate a Diagnostic Interrupt \(usually an NMI on x86 systems\) to cease normal operations, perform diagnostic actions and typically halt the system.|
|On|Turn the unit on.|
|PowerCycle|Perform a power cycle of the unit.|
|PushPowerButton|Simulate the pressing of the physical power button on this unit.|

###  Sample Response header

```
content-length:	0 byte
content-type:	application/json; charset=utf-8
date:	Fri, 08 Nov 2019 07:49:42 GMT+7m 9s
**location:	/taskmon/taska9702e20-884c-41e2-bd9c-d779a4dd2e6e**
odata-version:	4.0
status:	202
x-frame-options:	sameorigin
```

###  Sample Response body

None

##  Changing the boot order of a set of servers to default settings

|**Method** |`POST`|
|-----|-------------------|
|**URI** |`/redfish/v1/AggregatorService/Actions/AggregationService.SetDefaultBootOrder` |
|**Returns** |Location URI of the Task Monitor associated with this operation in the response header.|
|**Response Code** |`202` on success|

###  Description

This action changes the boot order of one or more servers to default settings.

You can perform setDefaultBootOrder on a group of servers by specifying multiple server URIs in the request.

To know the status of the progress of this operation, perform GET on the URI of the Task Monitor returned in the Location header. See [How to monitor a task](#). If the setDefaultBootOrder action is successful, 200 response code along with a success message in the response body is returned.

###  Usage

```
curl --insecure -X POST \
   -H "X-Auth-Token:{X-Auth-Token}" \
   -H "Content-Type:application/json" \
   -d \
'{ 

"@Redfish.Copyright": "Copyright 2018-2019 DMTF. All rights reserved.", 

 "@odata.context": "/redfish/v1/$metadata#ActionInfo.ActionInfo", 

 "@odata.id": "/redfish/v1/AggregationService/SetDefaultBootOrderActionInfo", 

                 "@odata.type": "#ActionInfo.v1_0_3.ActionInfo", 

                 "Id": "AddActionInfo", 

                 "Name": "SetDefaultBootOrder Action Info", 

                  "parameters": { 

                  "ServerCollection": [ 

"/redfish/v1/Systems/{ComputerSystemId}", 

"/redfish/v1/Systems/{ComputerSystemId}" 

] 

} 

}' \
 'https://{FODIM_IP_address}/redfish/v1/AggregationService/Actions/AggregationService.SetDefaultBootOrder'


```

###  Sample Request body

```
{ 
   "@Redfish.Copyright":"Copyright 2018-2019 DMTF. All rights reserved.",
   "@odata.context":"/redfish/v1/$metadata#ActionInfo.ActionInfo",
   "@odata.id":"/redfish/v1/AggregationService/SetDefaultBootOrderActionInfo",
   "@odata.type":"#ActionInfo.v1_0_3.ActionInfo",
   "Id":"AddActionInfo",
   "Name":"SetDefaultBootOrder Action Info",
   "parameters":{ 
      "ServerCollection":[ 
         "/redfish/v1/Systems/97d08f36-17f5-5918-8082-f5156618f58d:1",
         "/redfish/v1/Systems/76632110-1c75-5a86-9cc2-471325983653:1"
      ]
   }
}

```

###  Request Parameters

|Parameter|Type|Description|
|---------|----|-----------|
|@odata.context|string|Refer to Common Redfish Schema parameters.|
|@odata.id|string|Refer to Common Redfish Schema parameters.|
|@odata.type|string|Refer to Common Redfish Schema parameters.|
|Id|string|Refer to Common Redfish Schema parameters.|
|Name|string|Refer to Common Redfish Schema parameters.|
|parameters\[ \{|array|The parameters associated with the SetDefaultBootOrder Action.|
|ServerCollection \} \]<br> | |Target servers for setDefaultBootOrder|

###  Sample Response header

```
status:	202
content-type:	application/json; charset=utf-8
**location:	/taskmon/task7c1b4b6c-8e46-4fb6-a66a-6f629c10d827**
odata-version:	4.0
x-frame-options:	sameorigin
content-length:	0 byte
date:	
Fri, 08 Nov 2019 08:13:40 GMT+7m 10s
```

###  Sample Response body

None



##  Deleting a Server

|**Method** |`POST` |
|------|-----------|
|**URI** |`/redfish/v1/AggregatorService/Actions/AggregationService.Remove` |
|**Returns** |Location URI of the Task Monitor associated with this operation in the response header.|
|**Response Code** |`202` on success|

###  Description

This action removes a specific server from the inventory.

To know the status of the progress of this operation, perform GET on the URI of the task monitor returned in the Location header. See [How to monitor a task](#). If the server is successfully removed from the inventory, 200 response code is returned.

###  Usage

```
curl --insecure -X POST \
   -H "X-Auth-Token:{X-Auth-Token}" \
   -H "Content-Type:application/json" \
   -d \
'{
"@Redfish.Copyright": "Copyright 2018-2019 DMTF. All rights reserved.",
"@odata.context": "/redfish/v1/$metadata#ActionInfo.ActionInfo",
"@odata.id": "/redfish/v1/AggregationService/RemoveActionInfo",
"@odata.type": "#ActionInfo.v1_0_3.ActionInfo",
"Id": "RemoveActionInfo",
"Name": "Remove Action Info",
"Oem": "",
"parameters": [
{
"Name": "/redfish/v1/Systems/{ComputerSystemId}"
}
]
}' \
 'https://{FODIM_IP_address}/redfish/v1/AggregationService/Actions/AggregationService.Remove/'


```

###  Sample Request body

```
{
	"@Redfish.Copyright": "Copyright 2018-2019 DMTF. All rights reserved.",
	"@odata.context": "/redfish/v1/$metadata#ActionInfo.ActionInfo",
	"@odata.id": "/redfish/v1/AggregationService/RemoveActionInfo",
	"@odata.type": "#ActionInfo.v1_0_3.ActionInfo",
	"Id": "AddActionInfo",
	"Name": "Remove Action Info",
	"Oem": "",
	"parameters": [
		{
			"Name": "/redfish/v1/Systems/97d08f36-17f5-5918-8082-f5156618f58d:1"
        }
	]
}
```

###  Request Parameters

|Parameter|Type|Description|
|---------|----|-----------|
|@odata.context|string|Refer to Common Redfish Schema parameters.|
|@odata.id|string|Refer to Common Redfish Schema parameters.|
|@odata.type|string|Refer to Common Redfish Schema parameters.|
|Id|string|Refer to Common Redfish Schema parameters.|
|Name|string|Refer to Common Redfish Schema parameters.|
|Oem|object|Refer to Common Redfish Schema parameters.|
|parameters\[ \{|array|The parameters associated with the Delete Action.|
|Name \} \]<br> |string|The URI of the target to be removed - `"/redfish/v1/Systems/{ComputerSystemId}"` |

###  Sample Response header

```
content-length:	0 byte
content-type:	application/json; charset=utf-8
date:	Fri, 08 Nov 2019 05:49:05 GMT+7m 10s
**location:	/taskmon/taskd5bba4ec-35a3-43cc-a058-9634c488a488**
odata-version:	4.0
status:	202
x-frame-options:	sameorigin
```

###  Sample Response body

None



#  Managing tasks

A task represents an operation that takes more time than a user typically wants to wait and is carried out asynchronously.

An example of a task is resetting an aggregate of servers. Resetting all the servers in a group is a time-consuming operation; the user waiting for the result would be blocked from performing other operations. FODIM creates Redfish tasks for such long-duration operations and offers an interface for the users to manage them while performing other operations.

FODIM exposes Redfish TaskService APIs and Task monitor API. Use these endpoints to manage and poll tasks for their completion.

##  Prerequisites

-   Ensure that the user has `Login` privilege at the minimum.


##  Authentication

X-AUTH-TOKEN authentication or Basic authentication.

##  Supported Endpoints

-   `/redfish/v1/TaskService`

-   `/redfish/v1/TaskService/Tasks`

-   `/redfish/v1/TaskService/Tasks/{TaskID}`

-   `/taskmon/{TaskID}`

#  Subscribing northbound clients to southbound events

FODIM offers an eventing interface that allows northbound clients to interact and receive notifications such as alarms from southbound equipment. It exposes Redfish EventService APIs as an interface for managing events.

Events are messages provided by the service to asynchronously notify the client of some significant state change or error condition, usually of a time critical nature.

Use these APIs to subscribe northbound clients to southbound events by creating a subscription entry in the service.

##  Prerequisites

-   Ensure that Kafka is installed.

-   Ensure that the user has an `administrator` role or a `client` role.


##  Authentication

X-AUTH-TOKEN authentication or Basic authentication.

##  Supported Endpoints

-   `/redfish/v1/EventService/Subscriptions`

-   `redfish/v1/EventService`




# Accessing the inventory of a managed southbound resource

FODIM allows northbound clients to monitor telemetry data from southbound compute, network, and storage resources including chassis by exposing Redfish SystemService endpoints. It also offers the capability to search southbound resources by applying filters.

Use these endpoints to discover information on all the telemetry you want to monitor such as CPU utilization, memory utilization, processor power consumption, thermal health.

##  Prerequisites

-   Ensure that the user has a minimum privilege of "Login".


##  Authentication

X-AUTH-TOKEN authentication or Basic authentication.

##  Supported Endpoints

-    `/redfish/v1/Systems`

-   `/redfish/v1/Systems/<ComputerSystemId>`

-   `/redfish/v1/Systems/<ComputerSystemId>/Memory`

-   `/redfish/v1/Systems/<ComputerSystemId>/MemoryDomains`

-   `/redfish/v1/Systems/<ComputerSystemId>/EthernetInterfaces`

-   `/redfish/v1/Systems/<ComputerSystemId>/bios`

-   `/redfish/v1/Systems/<ComputerSystemId>/SecureBoot`

-   `/redfish/v1/Systems/<ComputerSystemId>/Storage`

-   `/redfish/v1/Systems/<ComputerSystemId>/LogServices`

-   `/redfish/v1/Systems/<ComputerSystemId>/Processors`

-   `/redfish/v1/Systems/<ComputerSystemId>/Memory/proc1dimm1`

-   `/redfish/v1/Systems/<ComputerSystemId>/EthernetInterfaces/1`

-   `/redfish/v1/Systems/<ComputerSystemId>/Processors/1`

-   `/redfish/v1/Chassis`

-   `/redfish/v1/Chassis/{ComputerSystemId}`

-   `/redfish/v1/Chassis/<ComputerSystemId>/NetworkAdapters`

-   `/redfish/v1/Chassis/<ComputerSystemId>/Thermal`

-   `/redfish/v1/Chassis/<ComputerSystemId>/Power`

-   `/redfish/v1/Systems?filter=<searchKeys>%20<conditionKeys>%20<value>`

-   `/redfish/v1/Systems/{ComputerSystemId}/Actions/ComputerSystem.Reset`

-   `/redfish/v1/Systems/{ComputerSystemId}/Actions/ComputerSystem.SetDefaultBootOrder`
