---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - curl


toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
   - Aggregating_resources

search: true
---
# <a name="GUID-C1C0E36F-4C5E-44F5-BB6C-1281B7C9ACDC"/> About This Guide

This document provides reference
information for the REST API requests
that you can send to FODIM RESTful API.
This document is intended for the
technical personnel who customize and
deploy this optimized solution according
to the requirements of the end customer.


# <a name="GUID-097D329B-7B27-4682-B24B-0C869A8D6109"/> Introduction to Framework for Open Distributed Manageability

The FODIM \(Framework for Open Distributed Manageability\) RESTful API for HPE FODIM is a programming interface enabling management of HPE Telco NFV infrastructure.
FODIM combines with Redfish aggregation function to allow northbound clients to interact with the system and manage the southbound infrastructure using Redfish compliant APIs.

With modern scripting languages, you can easily write simple REST clients for RESTful APIs.

# <a name="GUID-34228414-88DA-4989-96EE-413E0B8827BE"/> FODIM logical architecture

The FODIM based on Redfish standards uses RESTful API to create an environment that is designed to be implemented on many different models of servers and other IT infrastructure devices for years to come. These devices may be quite different from one another. For this reason, the Redfish API does not specify the URIs to various resources.

![](GUID-80B6D875-3C75-47BB-8CC1-A7426CCBC145-high.png)




# <a name="GUID-3F3347E9-3570-4FFB-B515-BF17D2745CB4"/> List of available APIs

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

<blockquote>
NOTE:

<ComputerSystemId\> is ORCA specified unique id of the server. It is represented as "UUID:<n\>" in FODIM \( UUID:<n\> Eg : ba0a6871-7bc4-5f7a-903d-67f3c205b08c:1 \)

</blockquote>
# <a name="GUID-68797EFE-178C-48A6-9ED1-47D35A1233C5"/> HTTP Request Methods, Responses, and Error Codes for FODIM REST API

Following are the Redfish defined HTTP methods that you can use to implement various actions:

|HTTP Request Methods|Description|
|--------------------|-----------|
|`GET` \[Read Requests\]|Use this method to request a representation of a specified resource \(single resource or collection\).|
|`PATCH` \[Update\]|Use this method to apply partial modifications to a resource.|
|`PUT` \[Replace\]|Use this method to complete replace a resource. Any properties omitted from the body of the request are reset to their default value.|
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
|207 Multi-Status|Information about multiple resources|
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



# <a name="GUID-63D85EBC-2982-494F-8B65-2DEDED0536A8"/> Authenticating requests when using the FODIM REST APIs

Any one of the following authentication methods must be implemented to authenticate requests to the FODIM Redfish services:

-   **HTTP BASIC authentication**

    You can implement HTTP BASIC authentication by providing base64 encoded user name and password in an HTTP `Authorization: Basic` header \( Basic Auth\).

-   **Redfish session login authentication**

    You can implement Redfish Session Login Authentication by creating a Redfish login session through session management interface. To learn more about a session, See [Creating a session](#) section.

    Every session that is created has an authentication token called **X-AUTH-TOKEN** associated with it.

    To authenticate subsequent requests, provide this token in the `X-AUTH-TOKEN` request header.

<blockquote>
    NOTE:

    An **X-AUTH-TOKEN** is valid and a session is open only for 30 minutes, unless the user continues to send requests to a Redfish service using this token.

    An idle session is automatically terminated after the timeout interval.

</blockquote>



<blockquote>
NOTE:

-   When Basic Auth is used, creating a session is not required.

-   You must authenticate all write requests\(POST, PATCH, and DELETE \) on the resources except for The POST operation on the Sessions service/object needed for authentication.

-   You must authenticate all resources except for:

-   The service root

-   The $metadata

-   The OData Service Document

-   The Redfish version


</blockquote>
# <a name="GUID-A5EBA130-1939-4467-955A-5FA070E5A488"/> Creating and managing sessions

A session represents a window of user's login with a Redfish service and contains details about the user and the user activity. You can run multiple sessions simultaneously on FODIM.

FODIM offers Redfish SessionService interface for managing sessions and exposes APIs to achieve the following:

-   Fetching the SessionService root

-   Creating a session

-   Listing active sessions

-   Deleting a session


## <a name="SECTION_7D1A74F40AF24CC58CA9E4D4C7931EEC"/> Supported APIs

-   `/redfish/v1/SessionService`

-   `/redfish/v1/SessionService/Sessions`

-   `/redfish/v1/SessionService/Sessions/{session id}`




# <a name="GUID-2DAB4A03-6714-4738-822F-6492D3A384F4"/> Configuring roles and privileges for a user

FODIM supports role-based authorization of requests - the roles and privileges control which users have what access to resources.

## <a name="SECTION_BBA6081A64414384AF54FA4A8D24524D"/> Roles

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


<blockquote>
NOTE:

-   Every user is assigned one role at the time of user account creation; ensure that the role to be assigned to a user is created before creating a user account.

-   The Redfish pre-defined roles can include OEM privileges.

-   OEMs can create custom roles and assign the same to a user at the time of user creation.


</blockquote>
## <a name="SECTION_35D1CE274D6F497481A82BC84189C6F8"/> Privileges

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


<blockquote>
NOTE:

The privileges set for a Redfish pre-defined role cannot be modified.

</blockquote>
## <a name="SECTION_5FEE4116689947DDA3F54DAE14F8BCCA"/> Mapping of privileges to roles in FODIM

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

# <a name="GUID-5AC7D07F-6843-4089-9A6F-2B2F760FDE7A"/> Creating and managing user accounts

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


## <a name="SECTION_976BC1B372A544E3B3FBB47DD6A597F8"/> Authentication

X-AUTH-TOKEN authentication or Basic authentication.



# <a name="GUID-FCAF2009-0DB1-4AC6-9439-BD6776C59AC3"/> Aggregating and managing southbound infrastructure \(use case for server aggregation\)

One of the state-of-the-art features of FODIM is that it allows users to add and group southbound infrastructure\(servers, storage, or fabrics\) into one aggregate for easy manageability. It offers Redfish AggregationService as an interface and exposes endpoints to achieve the following:

-   Adding a resource in the inventory to manage.

-   Resetting one or more resources.

-   Changing the boot path of one or more resources to default settings.

-   Removing a resource from the inventory which is no longer managed.


Using these endpoints, you can add or remove only one resource at a time. You can group the resources into one collection and perform actions such as reset and setdefaultbootorder in combination on that group.

The next sections in this chapter contain the use case for server aggregation.

## <a name="SECTION_BE4F7B8CAC104DCEBF73E66F58CAF162"/> Prerequisites

-   To access Redfish AggregationService endpoints, ensure that the user has `ConfigureComponents` privilege.


## <a name="SECTION_976BC1B372A544E3B3FBB47DD6A597F8"/> Authentication

X-AUTH-TOKEN authentication or Basic authentication.

## <a name="SECTION_57EC3823E6E34F5C9EC3EC894C834122"/> Supported Endpoints

-   `/redfish/v1/AggregatorService/Actions/AggregationService.Add`

-   `/redfish/v1/AggregatorService/Actions/AggregationService.Remove`

-   `/redfish/v1/AggregationService/Actions/AggregationService.Reset`

-   `/redfish/v1/AggregationService/Actions/AggregationService.SetDefaultBootOrder`



# <a name="GUID-FD398AD7-F078-4D07-A529-0AC21975452F"/> Adding a Server

|<strong>Method</strong> |<strong>POST</strong> |
|<strong>URI</strong> |`/redfish/v1/AggregatorService/Actions/AggregationService.Add` |
|<strong>Returns</strong> |Location URI of the task monitor associated with this operation in the response header.|
|<strong>Response Code</strong> |202|

## <a name="SECTION_DF9D801BAD2C4FCB8868F370BECEF031"/> Description

This action adds a single server in the inventory.
On success, ORCA specified Computer System Id is assigned to the added server.

To poll this action for its completion, perform GET on the URI of the task monitor returned in the Location header. See [How to monitor a task](#). If the server is successfully added in the inventory, its endpoint having the System Id is returned as response.

<blockquote>
NOTE:

Make note of the Computer System Id as it is required to perform subsequent actions such as delete, reset, and setdefaultbootorder on this server.

\*Computer System Id is represented as "\{UUID\}:\{<n\>\}" in FODIM.

</blockquote>




## <a name="SECTION_6DE81E7892254E0687C134D360613807"/> Usage

```
curl --insecure -X POST \
   -H "X-Auth-Token:{X-Auth-Token}" \
   -H "Content-Type:application/json" \
   -d \
'{"ManagerAddress":"{Server_IP_Address}","UserName":"{Account_UserName}","Password":"{Account_Password}","Oem":{"PluginID":"{Redfish_PluginId}"}}' \
 'https://{FODIM_IP_address}/redfish/v1/AggregationService/Actions/AggregationService.Add'


```

## <a name="SECTION_20FBE069A2CF4C4FB8747D62D16A5F2A"/> Sample Request body

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

## <a name="SECTION_C40D7F028C9C48C4A7F51F2D08CCAEED"/> Request Parameters

|Parameter|Type|Description|
|---------|----|-----------|
|ManagerAddress| |A valid IP address or hostname of a baseboard management controller \(BMC\)|
|Username|string|The username for this user account|
|Password|string|The password for this user account|
|PluginID|string|The plugin id Example : GRF \(Generic Redfish Plugin\)<br> |

## <a name="SECTION_C97256BB9792461497DAFB69600894A0"/> Sample Response header

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

## <a name="SECTION_9451CFC577B74AC2B3A435EC191F5898"/> Sample Response body

None

# <a name="GUID-BF1C7CEB-B101-4BFC-930A-8A582AC478F6"/> Managing tasks

A task represents an operation that takes more time than a user typically wants to wait and is carried out asynchronously.

An example of a task is resetting an aggregate of servers. Resetting all the servers in a group is a time-consuming operation; the user waiting for the result would be blocked from performing other operations. FODIM creates Redfish tasks for such long-duration operations and offers an interface for the users to manage them while performing other operations.

FODIM exposes Redfish TaskService APIs and Task monitor API. Use these endpoints to manage and poll tasks for their completion.

## <a name="SECTION_BE4F7B8CAC104DCEBF73E66F58CAF162"/> Prerequisites

-   Ensure that the user has `Login` privilege at the minimum.


## <a name="SECTION_CDE2B668C2A248338F49149628D7B304"/> Authentication

X-AUTH-TOKEN authentication or Basic authentication.

## <a name="SECTION_57EC3823E6E34F5C9EC3EC894C834122"/> Supported Endpoints

-   `/redfish/v1/TaskService`

-   `/redfish/v1/TaskService/Tasks`

-   `/redfish/v1/TaskService/Tasks/{TaskID}`

-   `/taskmon/{TaskID}`
# <a name="GUID-3E0F2F80-1CEC-4182-85B8-3B9533874E44"/> Subscribing northbound clients to southbound events

FODIM offers an eventing interface that allows northbound clients to interact and receive notifications such as alarms from southbound equipment. It exposes Redfish EventService APIs as an interface for managing events.

Events are messages provided by the service to asynchronously notify the client of some significant state change or error condition, usually of a time critical nature.

Use these APIs to subscribe northbound clients to southbound events by creating a subscription entry in the service.

## <a name="SECTION_BE4F7B8CAC104DCEBF73E66F58CAF162"/> Prerequisites

-   Ensure that Kafka is installed.

-   Ensure that the user has an `administrator` role or a `client` role.


## <a name="SECTION_CDE2B668C2A248338F49149628D7B304"/> Authentication

X-AUTH-TOKEN authentication or Basic authentication.

## <a name="SECTION_57EC3823E6E34F5C9EC3EC894C834122"/> Supported Endpoints

-   `/redfish/v1/EventService/Subscriptions`

-   `redfish/v1/EventService`




# <a name="GUID-D6EAF208-051A-4A34-87BC-9F40423CFA67"/> Discovering and manipulating southbound infrastructure

FODIM allows northbound clients to monitor telemetry data from southbound compute, network, and storage resources including chassis by exposing Redfish SystemService endpoints. It also offers the capability to search southbound resources by applying filters.

Use these endpoints to discover information on all the telemetry you want to monitor such as CPU utilization, memory utilization, processor power consumption, thermal health.

## <a name="SECTION_BE4F7B8CAC104DCEBF73E66F58CAF162"/> Prerequisites

-   Ensure that the user has a minimum privilege of "Login".


## <a name="SECTION_CDE2B668C2A248338F49149628D7B304"/> Authentication

X-AUTH-TOKEN authentication or Basic authentication.

## <a name="SECTION_57EC3823E6E34F5C9EC3EC894C834122"/> Supported Endpoints

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
