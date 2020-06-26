---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - cURL
  - json: JSON Request and Response
  


toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  

search: true
---
# About this guide

This guide provides usage information for HPE Resource Aggregator for Open Distributed Infrastructure Management™ (ODIM™) including helpful reference information for the northbound REST APIs that management and orchestration systems use to communicate with southbound infrastructure elements through their respective plugins. 


# Introduction 

 Welcome to HPE Resource Aggregator for Open Distributed Infrastructure Management™!
 
 
HPE Resource Aggregator for Open Distributed Infrastructure Management (ODIM™) is a modular, open framework for
simplified management and orchestration of distributed workloads. It provides a unified management platform for
converging multivendor hardware equipment. By exposing a standards-based programming interface, it enables easy and
secure management of wide range of multivendor IT infrastructure distributed across multiple data centers.
HPE Resource Aggregator for ODIM framework comprises the following two components.

- The resource aggregation function (the resource aggregator)
- One or more plugins

This guide provides reference information for the northbound APIs exposed by the resource aggregator. These APIs
are designed as per DMTF's [Redfish® Scalable Platforms API (Redfish) specification 1.8.0](http://redfish.dmtf.org/schemas/DSP0266_1.8.0.pdf). 
Redfish® is an open industry standard specification, API, and schema, which specifies a RESTful interface and uses JSON and OData. The Redfish
standards are designed to deliver simple and secure management for converged, hybrid IT in a multivendor environment.
These APIs are fully Redfish-compliant.

##  HPE Resource Aggregator for ODIM logical architecture

HPE Resource Aggregator for ODIM framework adopts a layered architecture and has many functional layers.


The following figure shows these functional layers of HPE Resource Aggregator for ODIM deployed in a data center.

![ODIM_architecture](telco_bruce_arch.png)

- **API layer**


This layer hosts a REST server which is open-source and secure. It learns about the southbound resources from
the plugin layer and exposes the corresponding Redfish data model payloads to the northbound clients. The
northbound clients communicate with this layer through a REST-based protocol that is fully compliant with
DMTF's Redfish® specifications (Schema 2019.3 and Specification 1.8.0).
The API layer sends user requests to the plugins through the aggregation, event, and fabric services.

- **Services layer**


The services layer is where all the services are hosted. This layer implements service logic for all use cases
through an extensible domain model (Redfish Data Model). Requests coming from the API layer and the
responses coming from the plugin layer are mapped to the actual end resources in this layer. It maintains the state
for event subscriptions, credentials, and tasks. It also hosts a message bus called the Plug-in Message Bus (PMB).

![Redfish_data_model](telco_redfish_data_model.png)

- **Event message bus layer**


This layer hosts a message broker which acts as a communication channel between the upper layers and the
plugin layer. It supports common messaging architecture and real-time streaming. HPE Resource Aggregator for
ODIM uses Kafka as the event message bus.
The services and the event message bus layers host Redis data store.

- **Plugin layer**


This layer connects the actual managed resources to the aggregator layers and is de-coupled from the upper
layers. A plugin abstracts vendor-specific access protocols to a common interface which the aggregator layers use
to communicate with the resources. It uses REST-based communication which is based on OpenAPI Specification
v3.0 to interact with the other layers. It collects events to be exposed to fault management systems and uses the
event message bus to publish events. The messaging mechanism is based on OpenMessaging Specification.
The plugin layer allows developers to create plugins on any tool set of their choice without enforcing any strict
language binding. To know how to develop plugins, refer to *HPE Resource Aggregator for Open Distributed
Infrastructure Management Plugin Developer's Guide*.


# Accessing the APIs

To access the RESTful APIs exposed by the resource aggregator, you need an HTTPS-capable client, such as a web
browser with a REST Client plugin extension, or a Desktop REST Client application, or curl (a popular, free command-line
utility). You can also easily write simple REST clients for RESTful APIs using modern scripting languages.

<aside class="notice">
Tip: It is good to use a tool, such as curl or any Desktop REST Client application initially to perform requests. Later,
you will want to write your own scripting code to perform requests.
</aside>


This guide contains sample request and response payloads. For information on response payload parameters, refer to 
[Redfish® Scalable Platforms API (Redfish) schema 2019.3](https://www.dmtf.org/sites/default/files/standards/documents/DSP2046_2019.3.pdf).

**HTTP headers**

Include the following HTTP headers:

- `Content-Type:application/json` for all RESTful API operations that include a request body in JSON
    format.
- Authentication header (BasicAuth or XAuthToken) for all RESTful API operations except for HTTP GET on the
    Redfish service root and HTTP POST on sessions.

**URL**

Use the following URL in all HTTP requests that you send to the resource aggregator:

`https://{odim_host}:{port}/`

- {odim_host} is the fully qualified domain name (FQDN) used for generating certificates while deploying the
    resource aggregator.
	
	**NOTE:**
     Ensure that FQDN is provided in the `/etc/hosts` file or in the DNS server.


- {port} is the port where the services of the resource aggregator are running. The default port is 45000. If you
    have changed the default port in the `odim_config.json` file, use that as the port.

**curl usage**

The examples shown in this guide use curl to make HTTP requests.

[curl](https://curl.haxx.se) is a command-line tool which helps you get or send information through URLs using supported protocols. HPE
Resource Aggregator for ODIM supports HTTPS.

**curl command options (flags):**

- `--cacert` <file_path> includes a specified X.509 root certificate.
- `-H` passes on custom headers.
- `-X` specifies a custom request method. Use -X for HTTP PATCH, PUT, DELETE operations.
- `-d` posts data to a URI. Use -d for all HTTP operations that include a request body.
- `-i` returns HTTP response headers.
- `-v` fetches verbose.

For a complete list of curl flags, see information provided at [https://curl.haxx.se](https://curl.haxx.se).

<aside class="notice">
IMPORTANT: If you have set proxy configuration, set no_proxy using the following command, before running a
curl command.<br>
$ export no_proxy="127.0.0.1,localhost,{odim_host}".
</aside>
   



**Including HTTP certificate**

Without CA certificate, curl fails to verify that HTTP connections are secure and curl commands may fail with the SSL
certificate problem. Provide the root CA certificate to curl for secure SSL communication.

	
```curl
curl -v --cacert {path}/rootCA.crt 'https://{odim_host}:{port}/redfish/v1'
 ```
 
**1.** If you are running curl commands on the server where the resource aggregator is deployed, provide the
    `rootCA.crt` file as shown in the curl command:

	
    {path} is where you have generated certificates during the deployment of the resource aggregator.

**2.** If you are running curl commands on a different server, perform the following steps to provide the rootCA.crt file.
      
      a. Navigate to `~/ODIM_v1.0/configuration/Odim/certificates` on the server where the resource
         aggregator is deployed.


      b. Copy the `rootCA.crt` file.


      c. Log in to your server and paste the `rootCA.crt` file in a folder.


      d. Open the `/etc/hosts` file to edit.


      e. Scroll to the end of the file, add the following line, and save:
         `{odim_server_ipv4_address} {FQDN}`

```curl
curl -v --cacert {path}/rootCA.crt 'https://{odim_host}:{port}/redfish/v1'
 ```
 
      f. Check if curl is working using the curl command:
	     

		 
<aside class="notice">
To avoid using the `--cacert` flag in every curl command, add `rootCA.crt` in the `ca-certificates.crt` file located in this path:<br> `/etc/ssl/certs/ca-certificates.crt`.
</aside>



#  List of supported APIs

HPE Resource Aggregator for ODIM supports the following Redfish APIs:



|Redfish Service Root||
|-------|--------------------|
|/redfish|`GET`|
|/redfish/v1|`GET`|
|/redfish/v1/odata|`GET`|
|/redfish/v1/$metadata|`GET`|

|SessionService||
|-------|--------------------|
|/redfish/v1/SessionService|`GET`|
|/redfish/v1/SessionService/Sessions|`POST`, `GET`|
|redfish/v1/SessionService/Sessions/\{sessionId\}|`GET`, `DELETE`|

|AccountService||
|-------|--------------------|
|/redfish/v1/AccountService|`GET`|
|/redfish/v1/AccountService/Accounts|`POST`, `GET`|
|/redfish/v1/AccountService/Accounts/\{accountId\}|`GET`, `DELETE`, `PATCH`|
|/redfish/v1/AccountService/Roles|`POST`, `GET`|
|/redfish/v1/AccountService/Roles/\{roleId\}|`GET`, `DELETE`, `PATCH`|

|AggregationService||
|-------|--------------------|
|/redfish/v1/AggregationService|`GET`|
|/redfish/v1/AggregationService/Actions|`GET`|
|/redfish/v1/AggregationService/Actions/AggregationService.Add|`POST`|
|/redfish/v1/AggregationService/Actions/AggregationService.Remove|`POST`|
|/redfish/v1/AggregationService/Actions/AggregationService.Reset|`POST`|
|/redfish/v1/AggregationService/Actions/AggregationService.SetDefaultBootOrder|`POST`|

|Systems||
|-------|--------------------|
|/redfish/v1/Systems|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}|`GET`, `PATCH`|
|/redfish/v1/Systems/\{ComputerSystemId\}/Memory|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/Memory/\{memoryId\}|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/MemoryDomains|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/NetworkInterfaces|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/EthernetInterfaces|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/EthernetInterfaces/\{Id\}|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/Bios|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/SecureBoot|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/Storage|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/Storage/\{storageSubsystemId\}|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/Storage/\{storageSubsystemId\}/Drives/\{driveId\}|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/Processors|`GET`|
|/redfish/v1/Systems/\{ComputerSystemId\}/Processors/\{Id\}|`GET`|
|/redfish/v1/Systems?filter=\{searchKeys\}%20\{conditionKeys\}%20\{value/regEx\}|`GET`|
| /redfish/v1/Systems/\{ComputerSystemId\}/Bios/Settings<br> |`GET`, `PATCH`|
|/redfish/v1/Systems/\{ComputerSystemId\}/Actions/ComputerSystem.Reset|`POST`|
|/redfish/v1/Systems/\{ComputerSystemId\}/Actions/ComputerSystem.SetDefaultBootOrder|`POST`|

|Chassis||
|-------|--------------------|
|/redfish/v1/Chassis|`GET`|
|/redfish/v1/Chassis/\{chassisId\}|`GET`|
|/redfish/v1/Chassis/\{chassisId\}/Thermal|`GET`|
|/redfish/v1/Chassis/\{ChassisId\}/Power|`GET`|
|/redfish/v1/Chassis/\{chassisId\}/NetworkAdapters|`GET`|

|Managers||
|-------|--------------------|
|/redfish/v1/Managers|`GET`|
|/redfish/v1/Managers/\{managerId\}|`GET`|
|/redfish/v1/Managers/\{managerId\}/EthernetInterfaces|`GET`|
|/redfish/v1/Managers/\{managerId\}/HostInterfaces|`GET`|
|/redfish/v1/Managers/\{managerId\}/LogServices|`GET`|
|/redfish/v1/Managers/\{managerId\}/NetworkProtocol|`GET`|

|EventService||
|-------|--------------------|
|/redfish/v1/EventService|`GET`|
|/redfish/v1/EventService/Subscriptions|`POST`, `GET`|
|/redfish/v1/EventService/Actions/EventService.SubmitTestEvent|`POST`|
|/redfish/v1/EventService/Subscriptions/\{subscriptionId\}|`GET`, `DELETE`|

|Fabrics||
|-------|--------------------|
|/redfish/v1/Fabrics|`GET`|
|/redfish/v1/Fabrics/\{fabricId\}|`GET`|
|/redfish/v1/Fabrics/\{fabricId\}/Switches|`GET`|
|/redfish/v1/Fabrics/\{fabricId\}/Switches/\{switchId\}|`GET`|
| /redfish/v1/Fabrics/\{fabricId\}/Switches/\{switchId\}/Ports<br> |`GET`|
| /redfish/v1/Fabrics/\{fabricId\} /Switches/\{switchId\}/Ports/\{portid\}<br> |`GET`|
|/redfish/v1/Fabrics/\{fabricId\}/Zones|`GET`, `POST`|
|/redfish/v1/Fabrics/\{fabricId\}/Zones/\{zoneId\}|`GET`, `PATCH`, `DELETE`|
|/redfish/v1/Fabrics/\{fabricId\}/AddressPools|`GET`, `POST`|
|/redfish/v1/Fabrics/\{fabricId\}/AddressPools/\{addresspoolid\}|`GET`, `DELETE`|
|/redfish/v1/Fabrics/\{fabricId\}/Endpoints|`GET`, `POST`|
|/redfish/v1/Fabrics/\{fabricId\}/Endpoints/\{endpointid\}|`GET`, `DELETE`|

|TaskService||
|-------|--------------------|
|/redfish/v1/TaskService|`GET`|
|/redfish/v1/TaskService/Tasks|`GET`|
|/redfish/v1/TaskService/Tasks/\{taskId\}|`GET`, `DELETE`|
| /redfish/v1/ TaskService/Tasks/\{taskId\}/SubTasks |`GET`|
| /redfish/v1/ TaskService/Tasks/\{taskId\}/SubTasks/ \{subTaskId\} |`GET`|


<aside class="notice">
Subtask URIs are not available in the current Redfish standard (1.8.0). They will be available in the next Redfish standard.
</aside>


|Task monitor||
|-------|--------------------|
|/taskmon/\{taskId\}|`GET`|

|Registries||
|-------|--------------------|
|/redfish/v1/Registries|`GET`|
|/redfish/v1/Registries/\{registryId\}|`GET`|
|/redfish/v1/registries/\{registryFileId\}|`GET`|


**NOTE:**

`ComputerSystemId` is the unique identifier of a system. It is specified by HPE Resource Aggregator for ODIM.

`ComputerSystemId` is represented as `<UUID:n>` in HPE Resource Aggregator for ODIM. `<UUID:n>` is the
universally unique identifier of a system (Example: ba0a6871-7bc4-5f7a-903d-67f3c205b08c:1).


## Viewing the list of supported Redfish services

```curl
curl -i GET 'https://{odim_host}:{port}/redfish/v1'
```

|||
|---------|-------|
|**Method** |`GET` |
|**URI** |`/redfish/v1` |
|**Description** |The URI for the Redfish service root.|
|**Returns** |All the available services in the service root.|
|**Response Code** |`200 OK` |
|**Authentication** |No|

 

 



> Sample response header 

```
Allow:GET
Cache-Control:no-cache
Connection:keep-alive
Content-Type:application/json; charset=UTF-8
Link:</redfish/v1/SchemaStore/en/ServiceRoot.json/>; rel=describedby
Odata-Version:4.0
X-Frame-Options:sameorigin
Date":Fri,15 May 2020 13:55:53 GMT+5m 11s
Transfer-Encoding:chunked

```

> Sample response body 

```
{
   "@odata.context":"/redfish/v1/$metadata#ServiceRoot.ServiceRoot",
   "@odata.id":"/redfish/v1/",
   "@odata.type":"#ServiceRoot.v1_5_0.ServiceRoot",
   "Id":"RootService",
   "Registries":{
      "@odata.id":"/redfish/v1/Registries"
   },
   "SessionService":{
      "@odata.id":"/redfish/v1/SessionService"
   },
   "AccountService":{
      "@odata.id":"/redfish/v1/AccountService"
   },
   "EventService":{
      "@odata.id":"/redfish/v1/EventService"
   },
   "Tasks":{
      "@odata.id":"/redfish/v1/TaskService"
   },
   "AggregationService":{
      "@odata.id":"/redfish/v1/AggregationService"
   },
   "Systems":{
      "@odata.id":"/redfish/v1/Systems"
   },
   "Chassis":{
      "@odata.id":"/redfish/v1/Chassis"
   },
   "Fabrics":{
      "@odata.id":"/redfish/v1/Fabrics"
   },
   "Managers":{
      "@odata.id":"/redfish/v1/Managers"
   },
   "Links":{
      "Sessions":{
         "@odata.id":"/redfish/v1/SessionService/Sessions"
      }
   },
   "Name":"Root Service",
   "Oem":{

   },
   "RedfishVersion":"1.8.0",
   "UUID":"a64fc187-e0e9-4f68-82a8-67a616b84b1d"
}
```



#  HTTP Request Methods, Responses, and Error Codes for FODIM REST API

Following are the Redfish-defined HTTP methods that you can use to implement various actions:

|HTTP Request Methods|Description|
|--------------------|-----------|
|`GET` \[Read Requests\]|Use this method to request a representation of a specified resource \(single resource or collection\).|
|`PATCH` \[Update\]|Use this method to apply partial modifications to a resource.|
|`POST` \[Create\] \[Actions\]|Use this method to create a resource. Submit this request to the resource collection to which you want to add the new resource. You can also use this method to initiate operations on a resource or a collection of resources.|
|`PUT` \[Replace\]|Use this method to replace the property values of a resource completely. It is used to both create and update the state of a resource.|
|`DELETE` \[Delete\]|Use this method to delete a resource.|

HPE Resource Aggregator for ODIM supports the following responses:

|Responses|Description|
|---------|-----------|
|Metadata responses|Describes the resources and types exposed by the service to generic clients.|
|Resource responses|Response in JSON format for an individual resource.|
|Resource collection responses|Response in JSON format for a collection of resources.|
|Error responses|If there is an HTTP error, a high-level JSON response is provided with additional information.|

Following are the HTTP status codes with their descriptions:

| Success code<br> |Description|
|-----------------|-----------|
|200 OK|Successful completion of the request with representation in the body.|
|201 Created|A new resource is successfully created with the `Location` header set to well-defined URI for the newly created resource. The response body may include the representation of the newly created resource.|
|202 Accepted|The request has been accepted for processing but not processed. The `Location` header is set to URI of a task monitor that can be queried later for the status of the operation.|
|204 No Content|The request succeeded, but no content is being returned in the body of the response.|

| Error code<br> |Description|
|-----------------|-----------|
|301 Moved Permanently|The requested resource resides in a different URI given by the `Location` headers.|
|400 Bad Request|The request could not be performed due to missing or invalid information. An extended error message is returned in the response body.|
|401 Unauthorized|Missing or invalid authentication credentials included with a request.|
|403 Forbidden|The server recognizes the credentials to be not having the necessary authorization to perform the operation.|
|404 Not Found|The request specified the URI of a nonexisting resource.|
|405 Method Not Allowed|When the HTTP verb specified in the request \(GET, PATCH, DELETE, and so on\) is not supported for a particular request URI. The response includes `Allow` header that lists the supported methods.|
|409 Conflict|A creation or an update cannot be completed because it would conflict with the current state of the resources supported by the platform.|
|500 Internal Server Error|When the server encounters an unexpected condition that prevents it from fulfilling the request.|
|501 Not Implemented|When the server has not implemented the method for the resource.|
|503 Service Unavailable|When the server is unable to service the request due to temporary overloading or maintenance.|


<aside class="notice">
This guide provides success codes (200, 201, 202, 204) for all the referenced API operations. For failed operations, refer to the error codes listed in this section.
</aside>




#  Authentication methods for Redfish APIs

 To keep HTTP connections secure, HPE Resource Aggregator for ODIM verifies credentials of HTTP requests. If you perform an unauthenticated HTTP operation on resources except for the following, you will receive an HTTP `401 unauthorized` error.

|Resource|URI|Description|
|--------|---|-----------|
|The Redfish service root|`GET` `/redfish` |The URI for the Redfish service root. It returns the version of Redfish services.|
|List of Redfish services|`GET``/redfish/v1` |It returns a list of available services.|
|$metadata|`GET``/redfish/v1/$metadata` |The Redfish metadata document.|
|OData|`GET``/redfish/v1/odata` |The Redfish OData service document.|
| The `Sessions` resource<br> |`POST``/redfish/v1/SessionService/Sessions` |Creates a Redfish login session.|

To authenticate requests with the Redfish services, implement any one of the following authentication methods:

-   **HTTP BASIC authentication \(BasicAuth\)** 
 
     To implement HTTP BASIC authentication:

     1. Generate a base64 encoded string of `{valid_username_of_odim_userAccount}:{valid_password_of_odim_userAccount}` using the following command:

       ```
       $ echo -n '{username}:{password}' | base64 -w0
       ```

       Initially, use the username and the password of the default administrator account. Later, you can create additional [user accounts](#user-accounts) and use their details to implement authentication.

```
curl -i --cacert {path}/rootCA.crt GET\
-H 'Authorization:Basic {base64_encoded_string_of_[username:password]}' \
'https://{odim_host}:{port}/redfish/v1/AccountService'
 ```
     
	 2. Provide the base64 encoded string in an HTTP `Authorization:Basic` header as shown in the curl command:

        

<br>

-   **Redfish session login authentication \(XAuthToken\)** 

```
curl -i --cacert {path}/rootCA.crt GET \
-H "X-Auth-Token:{X-Auth-Token}" \
'https://{odim_host}:{port}/redfish/v1/AccountService'
  ```

      To implement Redfish session login authentication, create a Redfish login [session](#Sessions) and obtain an authentication token through session management interface.

      Every session that is created has an authentication token called `X-AUTH-TOKEN` associated with it. An `X-AUTH-TOKEN` is returned in the response header from session creation.

      To authenticate subsequent requests, provide this token in the `X-AUTH-TOKEN` request header.

      

      An `X-AUTH-TOKEN` is valid and a session is open only for 30 minutes, unless you continue to send requests to a Redfish service using this token. An idle session is automatically terminated after the time-out interval.



# Sessions

A session represents a window of user's login with a Redfish service and contains details about the user and the user activity. You can run multiple sessions simultaneously.

HPE Resource Aggregator for ODIM offers Redfish `SessionService` interface for creating and managing sessions. It exposes APIs to achieve the following:

-   Fetching the `SessionService` root

-   Creating a session

-   Listing active sessions

-   Deleting a session



**Supported APIs**

|||
|-------|--------------------|
|/redfish/v1/SessionService|`GET`|
|/redfish/v1/SessionService/Sessions|`POST`, `GET`|
|redfish/v1/SessionService/Sessions/\{sessionId\}|`GET`, `DELETE`|



## SessionService root

```
curl -i GET \
              'https://{odim_host}:{port}/redfish/v1/SessionService'

 ```
 
 
> Sample response body

```
{
   "@odata.type":"#SessionService.v1_1_6.SessionService",
   "@odata.id":"/redfish/v1/SessionService",
   "Id":"Sessions",
   "Name":"Session Service",
   "Status":{
      "State":"Enabled",
      "Health":"OK"
   },
   "ServiceEnabled":true,
   "SessionTimeout":30,
   "Sessions":{
      "@odata.id":"/redfish/v1/SessionService/Sessions"
   }
}
```


|||
|---------|---------------|
|**Method** | `GET` |
|**URI** |`/redfish/v1/SessionService` |
|**Description** |Schema representing the Redfish `SessionService` root.|
|**Returns** |The properties for the Redfish `SessionService` itself and links to the actual list of sessions.|
|**Response Code** |`200 OK` |
|**Authentication** |No|






##  Creating a session

```
curl -i POST \
   -H "Content-Type:application/json" \
   -d \
'{
"UserName": "{username}",
"Password": "{password}"
}' \
 'https://{odim_host}:{port}/redfish/v1/SessionService/Sessions'

 ```

|||
|---------|---------------|
|**Method** | `POST` |
|**URI** |`/redfish/v1/SessionService/Sessions` |
|**Description** |This operation creates a session to implement authentication. Creating a session allows you to create an `X-AUTH-TOKEN` which is then used to authenticate with other services.<br>**NOTE:**<br>It is a good practice to note down the following:<br><ul><li>The session authentication token returned in the `X-AUTH-TOKEN` header.</li><li>The session Id returned in the `Location` header and the JSON response body.</li></ul><br> You will need the session authentication token to authenticate subsequent requests to the Redfish services and the session Id to log out later.|
|**Returns** |<ul><li> An `X-AUTH-TOKEN` header containing session authentication token.</li><li>`Location` header that contains a link to the newly created session instance.</li><li>The session Id and a message in the JSON response body saying that the session is created.</li></ul> |
|**Response Code** |`201 Created` |
|**Authentication** |No|



> Sample request body

```
{
"Username": "abc",
"Password": "abc123"
}
```





###  Request URI parameters

|Parameter|Type|Description|
|---------|----|-----------|
|Username|String \(required\)|Username of the user account for this session. For the first time, use the username of the default administrator account \(admin\). Later, when you create other user accounts, you can use the credentials of those accounts to create a session.<br>**NOTE:**<br> This user must have `Login` privilege.|
|Password|String \(required\)<br> |Password of the user account for this session. For the first time, use the password of the default administrator account. Later, when you create other user accounts, you can use the credentials of those accounts to create a session.<br> |



> Sample response header


```
Cache-Control:no-cache
Connection:keep-alive
Content-Type:application/json; charset=utf-8
Link:</redfish/v1/SessionService/Sessions/2d2e8ebc-4e7c-433a-bfd6-74dc420886d0/>; rel=self
**Location:\{odim\_host\}:\{port\}/redfish/v1/SessionService/Sessions/2d2e8ebc-4e7c-433a-bfd6-74dc420886d0**
Odata-Version:4.0
**X-Auth-Token:15d0f639-f394-4be7-a8ef-ef9d1df07288**
X-Frame-Options:sameorigin
Date:Fri,15 May 2020 14:08:55 GMT+5m 11s
Transfer-Encoding:chunked
```

> Sample response body


```
{
	"@odata.type": "#SessionService.v1_1_6.SessionService",
	"@odata.id": "/redfish/v1/SessionService/Sessions/1a547199-0dd3-42de-9b24-1b801d4a1e63",
	**"Id": "1a547199-0dd3-42de-9b24-1b801d4a1e63"**,
	"Name": "Session Service",
	"Message": "The resource has been created successfully",
	"MessageId": "Base.1.6.1.Created",
	"Severity": "OK",
	"UserName": "abc"
}
 ```



 
## Listing sessions

```
curl -i GET \
               -H 'Authorization:Basic {base64_encoded_string_of_[username:password]}' \
              'https://\{odim\_host\}:\{port}/redfish/v1/SessionService/Sessions'


```

|||
|---------|---------------|
|**Method** | `GET` |
|**URI** |`/redfish/v1/SessionService/Sessions` |
|**Description** |This operation lists user sessions.<br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can see a list of all user sessions.<br> Users with `ConfigureSelf` privilege can see only the sessions created by them.|
|**Returns** |Links to user sessions.|
|**Response Code** |`200 OK` |
|**Authentication** |Yes|




## Viewing information about a single session


```
curl -i GET \
                -H "X-Auth-Token:{X-Auth-Token}" \
              'https://\{odim\_host\}:\{port}/redfish/v1/SessionService/Sessions/{sessionId}'

```


> Sample response body

```
{
   "@odata.type":"#Session.v1_2_1.Session",
   "@odata.id":"/redfish/v1/SessionService/Sessions/4ee42139-22db-4e2a-97e4-020013248768",
   "Id":"4ee42139-22db-4e2a-97e4-020013248768",
   "Name":"User Session",
   "UserName":"admin"
}
```


|||
|---------|---------------|
|**Method** | `GET` |
|**URI** |`/redfish/v1/SessionService/Sessions/{sessionId}` |
|**Description** |This operation retrieves information about a specific user session.<br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can view information about any user session.<br><br> Users with `ConfigureSelf` privilege can view information about only the sessions created by them.<br>|
|**Returns** |JSON schema representing this session.|
|**Response Code** |`200 OK` |
|**Authentication** |Yes|

 



## Deleting a session


```
curl -i -X DELETE \
               -H 'Authorization:Basic {base64_encoded_string_of_[username:password]}' \
              'https://{odim_host}:{port}/redfish/v1/SessionService/Sessions/{sessionId}'

 ```

|||
|---------|---------------|
|**Method** | `DELETE` |
|**URI** |`/redfish/v1/SessionService/Sessions/{sessionId}` |
|**Description** |This operation terminates a specific Redfish session when the user logs out.<br>**NOTE:**<br> Users having the `ConfigureSelf` and `ConfigureComponents` privileges are allowed to delete only those sessions that they created.<br><br> Only a user with all the Redfish-defined privileges \(Redfish-defined `Administrator` role\) is authorized to delete any user session.<br> |
|**Response Code** |`204 No Content` |
|**Authentication** |Yes|

 








#  User roles and privileges

HPE Resource Aggregator for ODIM supports role-based authorization of requests—the roles and privileges control which users have what access to resources. If you perform an HTTP operation on a resource without necessary privileges, you will receive an HTTP `403 Forbidden` error.

**Roles**

A role represents a set of operations that a user is allowed to perform. It is determined by a defined set of privileges. Every user is assigned one role at the time of user account creation.

With HPE Resource Aggregator for ODIM, there are two kinds of defined roles:

-   **Redfish predefined roles** 

      Redfish predefined roles come with a predefined set of privileges. These predefined privileges cannot be removed or modified. You may assign additional OEM \(custom\) privileges. Following are the Redfish predefined roles that are available in HPE Resource Aggregator for ODIM by default:

       -   `Administrator` 

       -   `Operator` 

       -   `ReadOnly` <br>
	


-   **User-defined roles** 

      User-defined roles are the custom roles that you can create and assign to a user. The privileges of a user-defined role are configurable - you can choose a privilege or a set of privileges to assign to this role at the time of role creation.
      
	  **Note:** Before assigning a user-defined role to a user at the time of user account creation, ensure that it is created first.
	  
	  
<aside class="notice">
Redfish predefined roles cannot be modified. 
</aside>




**Privileges**

A privilege is a permission to perform an operation or a set of operations within a defined management domain.

The following Redfish-specified privileges can be assigned to any user in HPE Resource Aggregator for ODIM:

-   `ConfigureComponents`:
 
        Users with this privilege can configure components managed by the Redfish services in HPE Resource Aggregator for ODIM.

        This privilege is required to create, update, and delete a resource or a collection of resources exposed by Redfish APIs using HTTP `POST, PATCH, DELETE` operations.

-  `ConfigureManager`:
 
        Users with this privilege can configure manager resources.

-  `ConfigureComponents`:
 
        Users with this privilege can configure components managed by the services.

-  `ConfigureSelf`:
 
        Users with this privilege can change the password for their account.

-  `ConfigureUsers`:
 
        Users with this privilege can configure users and their accounts. This privilege is assigned to an `Administrator`.

        This privilege is required to create, update, and delete user accounts using HTTP `POST, PATCH, DELETE` operations.

-  `Login`:

        Users with this privilege can log in to the service and read the resources.

        This privilege is required to view any resource or a collection of resources exposed by Redfish APIs using HTTP `GET` operation.

 
 
 
**Mapping of privileges to roles in HPE Resource Aggregator for ODIM**

|Roles|Assigned privileges|
|-----|-------------------|
|Administrator \(Redfish predefined\)| `Login` <br>   `ConfigureManager` <br>   `ConfigureUsers` <br>    `ConfigureComponents` <br>   `ConfigureSelf` <br>|
|Operator \(Redfish predefined\)| `Login` <br>   `ConfigureComponents` <br>   `ConfigureSelf` <br>|
|ReadOnly \(Redfish predefined\)| `Login` <br>   `ConfigureSelf` <br>|


## Creating a role


```
curl -i POST \
   -H 'Authorization:Basic {base64_encoded_string_of_[username:password]}' \
   -H "Content-Type:application/json" \
   -d \
'{ 
   "RoleId":"CLIENT11",
   "AssignedPrivileges":[ 
      "Login",
      "ConfigureUsers",
      "ConfigureSelf"
   ],
   "OemPrivileges":null 
}' \
 'https://\{odim\_host\}:\{port}/redfish/v1/AccountService/Roles'


```

|||
|---------|---------------|
|**Method** | `POST` |
|**URI** |`/redfish/v1/AccountService/Roles` |
|**Description** |This operation creates a role other than Redfish predefined roles. <br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can perform this operation.|
|**Returns** |JSON schema representing the newly created role.|
|**Response code** |`201 Created` |
|**Authentication** |Yes|

 



> Sample request body

```
{ 
   "RoleId":"CLIENT11",
   "AssignedPrivileges":[ 
      "Login",
      "ConfigureUsers",
      "ConfigureSelf"
   ],
   "OemPrivileges":null
}
```

###  Request URI parameters

|Parameter|Type|Description|
|---------|----|-----------|
|Id|String \(required, read-only\)<br> |Name for this role. <br>**NOTE:**<br> Id cannot be modified later.|
|AssignedPrivileges|Array \(string \(enum\)\) \(required\)<br> |The Redfish privileges that this role includes. Possible values are:<br>  `ConfigureManager` <br>   `ConfigureSelf` <br>   `ConfigureUsers` <br>   `Login` <br>   `ConfigureComponents` <br>|
|OemPrivileges|Array \(string\) \(required\)<br> |The OEM privileges that this role includes. If you do not want to specify any OEM privileges, use `null` or `[]` as value.|


> Sample response body

```
{
   "@odata.type":"#Role.v1_2_4.Role",
   "@odata.id":"/redfish/v1/AccountService/Roles/CLIENT11",
   "Id":"CLIENT11",
   "Name":"User Role",
   "Message":"The resource has been created successfully.",
   "MessageId":"ResourceEvent.1.0.2.ResourceCreated",
   "Severity":"OK",
   "IsPredefined":false,
   "AssignedPrivileges":[
      "Login",
      "ConfigureUsers",
      "ConfigureSelf"
   ],
   "OemPrivileges":null
}
```

## Listing roles

```
curl -i GET \
   -H 'Authorization:Basic {base64_encoded_string_of_[username:password]}' \
 'https://\{odim\_host\}:\{port}/redfish/v1/AccountService/Roles'

```
> Sample response body

```
{ 
   "@odata.type":"#RoleCollection.RoleCollection",
   "@odata.id":"/redfish/v1/AccountService/Roles",
   "Name":"Roles Collection",
   "Members@odata.count":5,
   "Members":[ 
      { 
         "@odata.id":"/redfish/v1/AccountService/Roles/Administrator"
      },
      { 
         "@odata.id":"/redfish/v1/AccountService/Roles/Operator"
      },
      { 
         "@odata.id":"/redfish/v1/AccountService/Roles/ReadOnly"
      },
      { 
         "@odata.id":"/redfish/v1/AccountService/Roles/CLIENT13"
      },
      { 
         "@odata.id":"/redfish/v1/AccountService/Roles/CLIENT11"
      }
      
   ]
}
```


|||
|---------|---------------|
|**Method** | `GET` |
|**URI** |`/redfish/v1/AccountService/Roles` |
|**Description** |This operation lists available user roles. <br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can perform this operation.|
|**Returns** |Links to user role resources.|
|**Response Code** | `200 OK` |
|**Authentication** |Yes|

 


## Viewing information about a role

```
curl -i GET \
   -H 'Authorization:Basic {base64_encoded_string_of_[username:password]}' \
 'https://\{odim\_host\}:\{port}/redfish/v1/AccountService/Roles/{RoleId}'


 ```
 
 > Sample response body

```
{
   "@odata.type":"#Role.v1_2_4.Role",
   "@odata.id":"/redfish/v1/AccountService/Roles/CLIENT11",
   "Id":"CLIENT11",
   "Name":"User Role",
   "IsPredefined":false,
   "AssignedPrivileges":[
      "Login",
      "ConfigureUsers",
      "ConfigureSelf"
   ],
   "OemPrivileges":null
}
```


|||
|---------|---------------|
|**Method** | `GET` |
|**URI** |`/redfish/v1/AccountService/Roles/{RoleId}` |
|**Description** |This operation fetches information about a specific user role. <br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can perform this operation.|
|**Returns** |JSON schema representing this role. The schema has the details such as - Id, name, description, assigned privileges, OEM privileges.|
|**Response Code** | `200 OK` |
|**Authentication** |Yes|


## Updating a role

```
 curl -i -X PATCH \
   -H 'Authorization:Basic {base64_encoded_string_of_[username:password]}' \
   -H "Content-Type:application/json" \
   -d \
'{
  "AssignedPrivileges": [{Set_Of_Privileges_to_update}],
  "OemPrivileges": []
}' \
 'https://\{odim\_host\}:\{port}/redfish/v1/AccountService/Roles/{RoleId}'


```

> Sample request body

```
{ 
   "AssignedPrivileges":[ 
      "Login",
      "ConfigureManager",
      "ConfigureUsers"
   ],
   "OemPrivileges": []
}
```

> Sample response body

```
{
   "RoleId":"CLIENT11",
   "IsPredefined":false,
   "AssignedPrivileges":[
      "Login",
      "ConfigureManager",
      "ConfigureUsers"
   ],
   "OemPrivileges":null
}
```

|||
|---------|---------------|
|**Method** | `PATCH` |
|**URI** |`/redfish/v1/AccountService/Roles/{RoleId}` |
|**Description** |This operation updates privileges of a specific user role - assigned privileges \(Redfish predefined\) and OEM privileges. Id of a role cannot be modified.<br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can perform this operation.|
|**Returns** |JSON schema representing the updated role.|
|**Response code** | `200 OK` |
|**Authentication** |Yes|


## Deleting a role

```
curl -i -X DELETE \
   -H 'Authorization:Basic {base64_encoded_string_of_[username:password]}' \
 'https://\{odim\_host\}:\{port}/redfish/v1/AccountService/Roles/{RoleId}'

```


|||
|---------|---------------|
|**Method** | `DELETE` |
|**URI** |`/redfish/v1/AccountService/Roles/{RoleId}` |
|**Description** |This operation deletes a specific user role. If you attempt to delete a role that is already assigned to a user account, you will receive an HTTP `403 Forbidden` error.<br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can perform this operation.|
|**Response Code** |`204 No Content` |
|**Authentication** |Yes|


#  User accounts

HPE Resource Aggregator for ODIM allows users to have accounts to configure their actions and restrictions.

HPE Resource Aggregator for ODIM has an administrator user account by default.

Create other user accounts by defining a username, a password, and a role for each account. The username and the password are used to authenticate with the Redfish services \(using `BasicAuth` or `XAuthToken`\).

HPE Resource Aggregator for ODIM exposes Redfish `AccountsService` APIs to create and manage user accounts. Use these endpoints to perform the following operations:

-   Creating, modifying, and deleting account details.

-   Fetching account details.


**Supported APIs**:

|||
|-------|--------------------|
|/redfish/v1/AccountService|`GET`|
|/redfish/v1/AccountService/Accounts|`POST`, `GET`|
|/redfish/v1/AccountService/Accounts/\{accountId\}|`GET`, `DELETE`, `PATCH`|
|/redfish/v1/AccountService/Roles|`POST`, `GET`|
|/redfish/v1/AccountService/Roles/\{roleId\}|`GET`, `DELETE`, `PATCH`|



## AccountService root

```
curl -i GET \
   -H "X-Auth-Token:{X-Auth-Token}" \
 'https://{odim_host}:{port}/redfish/v1/AccountService'


```

> Sample response header

```
Allow:GET
Cache-Control:no-cache
Connection:Keep-alive
Content-Type:application/json; charset=utf-8
Link:</redfish/v1/SchemaStore/en/AccountService.json>; rel=describedby
Odata-Version:4.0
X-Frame-Options:sameorigin
Date:Fri,15 May 2020 14:32:09 GMT+5m 12s
Transfer-Encoding:chunked
```


> Sample response body

```
{
   "@odata.type":"#AccountService.v1_6_0.AccountService",
   "@odata.id":"/redfish/v1/AccountService",
   "@odata.context":"/redfish/v1/$metadata#AccountService.AccountService",
   "Id":"AccountService",
   "Name":"Account Service",
   "Status":{
      "State":"Enabled",
      "Health":"OK"
   },
   "ServiceEnabled":true,
   "AuthFailureLoggingThreshold":0,
   "MinPasswordLength":12,
   "AccountLockoutThreshold":0,
   "AccountLockoutDuration":0,
   "AccountLockoutCounterResetAfter":0,
   "Accounts":{
      "@odata.id":"/redfish/v1/AccountService/Accounts"
   },
   "Roles":{
      "@odata.id":"/redfish/v1/AccountService/Roles"
   }
}
```


|||
|---------|---------------|
|**Method** | `GET` |
|**URI** |`/redfish/v1/AccountService` |
|**Description** |The schema representing the Redfish `AccountService` root.|
|**Returns** |The properties common to all user accounts and links to the collections of manager accounts and roles.|
|**Response Code** | `200 OK` |
|**Authentication** |Yes|

 










## Creating a user account

```
curl -i POST \
   -H 'Authorization:Basic {base64_encoded_string_of_[username:password]}' \
   -H "Content-Type:application/json" \
   -d \
'{"Username":"{username}","Password":"{password}","RoleId":"{roleId}"}
' \
 'https://{odim_host}:{port}/redfish/v1/AccountService/Accounts'


```


|||
|-------|--------------------|
|**Method** | `POST` |
|**URI** |`/redfish/v1/AccountService/Accounts` |
|**Description** |This operation creates a user account. <br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can create a user account.|
|**Returns** |<ul><li>`Location` header that contains a link to the newly created account.</li><li>JSON schema representing the newly created account.</li></ul> |
|**Response Code** |`201 Created` |
|**Authentication** |Yes|

 





> Sample request body

```
{ 
   "Username":"monitor32",
   "Password":"HP1nvent2020!",
   "RoleId":"CLIENT11"
}
```

###  Request URI parameters

|Parameter|Type|Description|
|---------|----|-----------|
|Username|String \(required\)<br> |User name for the user account.|
|Password|String \(required\)<br> |Password for the user account. Before creating a password, see "Password Requirements" .|
|RoleId|String \(required\)<br> |The role for this account. To know more about roles, see [user roles and privileges](#user-roles-and-privileges). Ensure that the `roleId` you want to assign to this user account exists. To check the existing roles, see [Listing Roles](#listing-roles). If you attempt to assign an unavailable role, you will receive an HTTP `400 Bad Request` error.|

### Password requirements

-   Your password must not be same as your username.

-   Your password must be at least 12 characters long and at most 16 characters long.

-   Your password must contain at least one uppercase letter \(A-Z\), one lowercase letter \(a-z\), one digit \(0-9\), and one special character \(~!@\#$%^&\*-+\_|\(\)\{\}:;<\>,.?/\).


> Sample response header

```
Cache-Control:no-cache
Connection:keep-alive
Content-Type:application/json; charset=utf-8
Link:</redfish/v1/AccountService/Accounts/monitor32/>; rel=describedby
**Location:/redfish/v1/AccountService/Accounts/monitor32/**
Odata-Version:4.0
X-Frame-Options:"sameorigin
Date":Fri,15 May 2020 14:36:14 GMT+5m 11s
Transfer-Encoding:chunked
```

> Sample response body

```
{
   "@odata.type":"#ManagerAccount.v1_4_0.ManagerAccount",
   "@odata.id":"/redfish/v1/AccountService/Accounts/monitor32",
   "@odata.context":"/redfish/v1/$metadata#ManagerAccount.ManagerAccount",
   "Id":"monitor32",
   "Name":"Account Service",
   "Message":"The resource has been created successfully",
   "MessageId":"Base.1.6.1.Created",
   "Severity":"OK",
   "UserName":"monitor32",
   "RoleId":"CLIENT11",
   "AccountTypes":[
      "Redfish"
   ],
   "Password":null,
   "Links":{
      "Role":{
         "@odata.id":"/redfish/v1/AccountService/Roles/CLIENT11/"
      }
   }
}
```

##  Listing user accounts


```
curl -i GET \
   -H "X-Auth-Token:{X-Auth-Token}" \
 'https://{odim_host}:{port}/redfish/v1/AccountService/Accounts'


```


|||
|---------|---------------|
|**Method** | `GET` |
|**URI** |`/redfish/v1/AccountService/Accounts` |
|**Description** |This operation retrieves a list of user accounts. <br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can view a list of user accounts.|
|**Returns** |Links to user accounts.|
|**Response Code** |`200 OK` |
|**Authentication** |Yes|


##  Viewing the account details

```
curl -i GET \
   -H 'Authorization:Basic {base64_encoded_string_of_[username:password]}' \
 'https://{odim_host}:{port}/redfish/v1/AccountService/Accounts/{accountId}'


```


|||
|---------|---------------|
|**Method** | `GET` |
|**URI** |`/redfish/v1/AccountService/Accounts/{accountId}` |
|**Description** |This operation fetches information about a specific user account. <br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can view information about a user account.|
|**Returns** |JSON schema representing this user account.|
|**Response Code** |`200 OK` |
|**Authentication** |Yes|

 





> Sample response body

```
{
   "@odata.type":"#ManagerAccount.v1_4_0.ManagerAccount",
   "@odata.id":"/redfish/v1/AccountService/Accounts/monitor32",
   "@odata.context":"/redfish/v1/$metadata#ManagerAccount.ManagerAccount",
   "Id":"monitor32",
   "Name":"Account Service",
   "UserName":"monitor32",
   "RoleId":"CLIENT11",
   "AccountTypes":[
      "Redfish"
   ],
   "Password":null,
   "Links":{
      "Role":{
         "@odata.id":"/redfish/v1/AccountService/Roles/CLIENT11/"
      }
   }
}
```

## Updating a user account

```
curl -i -X PATCH \
   -H "X-Auth-Token:{X-Auth-Token}" \
   -H "Content-Type:application/json" \
   -d \
'{ 
   "Password":{new_password}",
   "RoleId":"CLIENT11"
}
' \
 'https://{odim_host}:{port}/redfish/v1/AccountService/Accounts/{accountId}'

```


|||
|---------|---------------|
|**Method** | `PATCH` |
|**URI** |`/redfish/v1/AccountService/Accounts/{accountId}` |
|**Description** |This operation updates user account details \(`username`, `password`, and `RoleId`\). To modify account details, add them in the request payload \(as shown in the sample request body\) and perform `PATCH` on the mentioned URI. <br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can modify user accounts. Users with `ConfigureSelf` privilege can modify only their own accounts.|
|**Returns** |<ul><li>`Location` header that contains a link to the updated account.</li><li>JSON schema representing the modified account.</li></ul>|
|**Response Code** |`200 OK` |
|**Authentication** |Yes|

 




> Sample request body

```
{ 
   "Password":"Testing)9-_?{}",
   "RoleId":"CLIENT11"
}
```

> Sample response header

```
Cache-Control:no-cache
Connection:keep-alive
Content-Type:application/json; charset=utf-8
Link:</redfish/v1/AccountService/Accounts/monitor32/>; rel=describedby
**Location:/redfish/v1/AccountService/Accounts/monitor32/**
Odata-Version:4.0
X-Frame-Options:"sameorigin
Date":Fri,15 May 2020 14:36:14 GMT+5m 11s
Transfer-Encoding:chunked

```

> Sample response body

```
{
   "@odata.type":"#ManagerAccount.v1_4_0.ManagerAccount",
   "@odata.id":"/redfish/v1/AccountService/Accounts/monitor32",
   "@odata.context":"/redfish/v1/$metadata#ManagerAccount.ManagerAccount",
   "Id":"monitor32",
   "Name":"Account Service",
   "Message":"The account was successfully modified.",
   "MessageId":"Base.1.6.1.AccountModified",
   "Severity":"OK",
   "UserName":"monitor32",
   "RoleId":"CLIENT11",
   "AccountTypes":[
      "Redfish"
   ],
   "Password":null,
   "Links":{
      "Role":{
         "@odata.id":"/redfish/v1/AccountService/Roles/CLIENT11/"
      }
   }
}
```

## Deleting a user account

```
curl  -i -X DELETE \
   -H "X-Auth-Token:{X-Auth-Token}" \
 'https://{odim_host}:{port}/redfish/v1/AccountService/Accounts/{accountId}'

```


|||
|---------|---------------|
|**Method** | `DELETE` |
|**URI** |`/redfish/v1/AccountService/Accounts/{accountId}` |
|**Description** |This operation deletes a user account. <br>**NOTE:**<br> Only a user with `ConfigureUsers` privilege can delete a user account.|
|**Response Code** |`204 No Content` |
|**Authentication** |Yes|


#  Resource aggregation

HPE Resource Aggregator for ODIM allows users to add and group southbound infrastructure into collections for easy management. It exposes Redfish `AggregationService` endpoints to achieve the following:

-   Adding a resource and building its inventory.

-   Resetting one or more resources.

-   Changing the boot path of one or more resources to default settings.

-   Removing a resource from the inventory which is no longer managed.


Using these endpoints, you can add or remove only one resource at a time. You can group the resources into one collection and perform actions \(`reset` and `setdefaultbootorder`\) in combination on that group.

All aggregation actions are performed as [tasks](#tasks) in HPE Resource Aggregator for ODIM. The actions performed on a group of resources \(resetting or changing the boot order to default settings\) are carried out as a set of subtasks.

<aside class="notice">
To access Redfish `AggregationService` endpoints, you require `ConfigureComponents` privilege. If you access these endpoints without necessary privileges, you will receive an HTTP `403` error.
</aside>



 **Supported endpoints**

|||
|-------|--------------------|
|/redfish/v1/AggregationService|`GET`|
|/redfish/v1/AggregationService/Actions|`GET`|
|/redfish/v1/AggregationService/Actions/AggregationService.Add|`POST`|
|/redfish/v1/AggregationService/Actions/AggregationService.Remove|`POST`|
|/redfish/v1/AggregationService/Actions/AggregationService.Reset|`POST`|
|/redfish/v1/AggregationService/Actions/AggregationService.SetDefaultBootOrder|`POST`|


## The aggregation service root


```
curl -i --insecure -X GET \
   -H "X-Auth-Token:{X-Auth-Token}" \
 'https://{FODIM_IP_address}/redfish/v1/AggregationService'


```

> Sample response header 

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

> Sample response body 

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



|**Method**|`GET` |
|-----|-------------------|
|**URI** |`redfish/v1/AggregationService` |
|**Description** |The URI for the Aggregation service root.|
|**Returns** |Properties for the service and a list of actions you can perform using this service.|
|**Response Code** |`200` on success|

 

 








## Adding a server

```
curl -i POST \
   -H "X-Auth-Token:{X-Auth-Token}" \
   -H "Content-Type:application/json" \
   -d \
'{"ManagerAddress":"{BMC_address}","UserName":"{BMC_UserName}","Password":"{BMC_Password}","Oem":{"PluginID":"{Redfish_PluginId}"}}' \
 'https://{odim_host}:{port}/redfish/v1/AggregationService/Actions/AggregationService.Add'


```



|||
|---------|---------------|
|**Method** | `POST` |
|**URI** |`/redfish/v1/AggregationService/Actions/AggregationService.Add` |
|**Returns** | <ul><li>`Location` URI of the task monitor associated with this operation in the response header.</li><li>Link to the task and the task Id in the sample response body. To get more information on the task, perform HTTP `GET` on the task URI. See "Sample response body \(HTTP 202 status\)".</li><li> On success:<ul><li>A success message in the JSON response body.</li><li>A link \(having `ComputerSystemId` \(`Id`\) specified by HPE Resource Aggregator for ODIM\) to the added server in the `Location` header.</li></ul></li></ul>|
|**Response code** |<ul><li>`202 Accepted`</li><li>`200 Ok`</li></ul>|
|**Authentication** |Yes|

**Description**

This action discovers information about a single server and performs a detailed inventory of it. 


It is performed as a task. To know the progress of this action, perform `GET` on the [task monitor](#task-monitor) returned in the response header \(until the task is complete\). When the task is successfully complete, you will receive `ComputerSystemId` of the added server.


**IMPORTANT:**

 `ComputerSystemId`(`Id`) is unique information about the server that is added. Save it as it is required to perform subsequent actions such as `delete`, `reset`, and `setdefaultbootorder` on this server. 
 
 You can get the `ComputerSystemId`(`Id`) of a specific server later by performing HTTP `GET` on `/redfish/v1/Systems`. See [collection of Computer Systems](#collection-of-computer-systems).


 

> Sample request body

```
{
	"ManagerAddress": "10.24.0.6",
	"Username": "iLO_username",
	"Password": "iLO_password",
	"Oem": {
		"PluginID": "ILO"
	}
}
```

### Request parameters

|Parameter|Type|Description|
|---------|----|-----------|
|ManagerAddress|String \(required\)<br> |A valid IP address or hostname of a baseboard management controller \(BMC\).|
|Username|String \(required\)<br> |The username of the HPE iLO administrator account.|
|Password|String \(required\)<br> |The password of the HPE iLO administrator account.|
|PluginID|String \(required\)<br> |The plugin id of the plugin. Example: "ILO" for the HPE iLO plugin.<br>**NOTE:**<br> Before specifying the plugin Id, ensure that the installed plugin is added in the HPE Resource Aggregator for ODIM inventory. To know how to add a plugin, see [Adding a Plugin](#adding-a-plugin).|

> Sample response header \(HTTP 202 status\)

```
Connection:keep-alive
Content-Type:application/json; charset=utf-8
**Location:/taskmon/task4aac9e1e-df58-4fff-b781-52373fcb5699**
Odata-Version:4.0
X-Frame-Options:sameorigin
Date:Sun,17 May 2020 14:35:32 GMT+5m 13s
Content-Length:491 bytes

```

> Sample response header \(HTTP 200 status\)

```
Connection:keep-alive
Content-Type:application/json; charset=utf-8
**Location:/redfish/v1/Systems/d8f740ba-5f01-4784-89b3-122fe76af739:1**
Odata-Version:4.0
X-Frame-Options:sameorigin
Date:Mon,18 May 2020 09:16:09 GMT+5m 15s
Content-Length:73 bytes
```

> Sample response body \(HTTP 202 status\)

```
{
   "@odata.type":"#Task.v1_4_2.Task",
   **"@odata.id":"/redfish/v1/TaskService/Tasks/task4aac9e1e-df58-4fff-b781-52373fcb5699",**
   "@odata.context":"/redfish/v1/$metadata#Task.Task",
   **"Id":"task4aac9e1e-df58-4fff-b781-52373fcb5699",**
   "Name":"Task task4aac9e1e-df58-4fff-b781-52373fcb5699",
   "Message":"The task with id task4aac9e1e-df58-4fff-b781-52373fcb5699 has started.",
   "MessageId":"TaskEvent.1.0.1.TaskStarted",
   "MessageArgs":[
      "task4aac9e1e-df58-4fff-b781-52373fcb5699"
   ],
   "NumberOfArgs":1,
   "Severity":"OK"
}
```

> Sample response body \(HTTP 200 status\)

```
{
   "code":"Base.1.6.1.Success",
   "message":"Request completed successfully."
}
```



## Resetting servers

```
curl -i POST \
   -H "X-Auth-Token:{X-Auth-Token}" \
   -H "Content-Type:application/json" \
   -d \
'{
"parameters": {
"ResetCollection": {
"description": "Collection of ResetTargets",
"ResetTarget": [{
"ResetType": "ForceRestart",
"TargetUri": "/redfish/v1/Systems/{ComputerSystemId}",
"Priority": 10,
"Delay": 5
},
{
"ResetType": "ForceOff",
"TargetUri": "/redfish/v1/Systems/{ComputerSystemId}",
"Priority": 9,
"Delay": 0
}
]
}
}
}' \
 'https://\{odim\_host\}:\{port\}/redfish/v1/AggregationService/Actions/AggregationService.Reset'


```


|||
|---------|---------------|
|**Method** | `POST` |
|**URI** |`/redfish/v1/AggregationService/Actions/AggregationService.Reset` |
|**Returns** |<ul><li> `Location` URI of the task monitor associated with this operation \(task\) in the response header.</li><li>Link to the task and the task Id in the sample response body. To get more information on the task, perform HTTP `GET` on the task URI.<br>**IMPORTANT:**<br>Note down the task Id. If the task completes with an error, it is required to know which subtask has failed. To get the list of subtasks, perform HTTP `GET` on `/redfish/v1/TaskService/Tasks/{taskId}`.</li><li>On successful completion of the reset operation, a message in the response body, saying that the reset operation is completed successfully. See "Sample response body \(HTTP 200 status \)".</li></ul>|
|**Response code** |<ul><li>`202 Accepted`</li><li>`200 Ok`</li></ul> |
|**Authentication** |Yes|

**Description**

This action shuts down, powers up, and restarts one or more servers. 
It is performed as a task and is further divided into subtasks to reset each server individually. To know the progress of this action, perform HTTP `GET` on the [task monitor](#task-monitor) returned in the response header \(until the task is complete\).

To get the list of subtask URIs, perform HTTP `GET` on the task URI returned in the JSON response body. See "Sample response body \(HTTP 202 status \)". The JSON response body of each subtask contains a link to the task monitor associated with it. To know the progress of the reset operation \(subtask\) on a specific server, perform HTTP `GET` on the task monitor associated with the respective subtask.



You can perform reset on a group of servers by specifying multiple target URIs in the request.




> Sample request body

```
{
   "parameters":{
      "ResetCollection":{
         "description":"Collection of ResetTargets",
         "ResetTarget":[
            {
               "ResetType":"ForceRestart",
               "TargetUri":"/redfish/v1/Systems/97d08f36-17f5-5918-8082-f5156618f58d:1",
               "Priority":10,
               "Delay":5
            },
            {
               "ResetType":"ForceOff",
               "TargetUri":"/redfish/v1/Systems/24b243cf-f1e3-5318-92d9-2d6737d6b0b9:1",
               "Priority":9,
               "Delay":0
            }
         ]
      }
   }
}

```

### Request parameters

|Parameter|Type|Description|
|---------|----|-----------|
|parameters\[\{|Array|The parameters associated with the reset Action.|
|ResetType|String \(required\)<br> |The type of reset to be performed. For possible values, see "Reset type". If the value is not supported by the target server machine, you will receive an HTTP `400 Bad Request` error.|
|TargetUri|String \(required\)<br> |The URI of the target for `Reset`. Example: `"/redfish/v1/Systems/{ComputerSystemId}"` |
|Priority|Integer\[0-10\] \(optional\)<br> |This parameter is used to indicate which reset action is performed on priority. You can set a priority number in the range of 0-10 \(zero being the least and 10 being the highest\). If this parameter is not specified, a priority of zero will be assigned by default.<br> |
|Delay\}\]|Integer \(seconds\)\[0-3600\] \(optional\)<br> |This parameter is used to defer the reset action by specified time. If this parameter is not specified, a delay of zero will be assigned by default.<br>**NOTE:**<ul><li> If two or more reset actions have equal priority values in a single request, they are performed one after the other in the order of their delay values \(a reset action with zero delay will be performed first\).</li><li>If two or more reset actions have equal priority and delay values, they are performed at the same time.</li></ul>|

### Reset type

|String|Description|
|------|-----------|
|ForceOff|Turn off the unit immediately \(non-graceful shutdown\).|
|ForceRestart|Perform an immediate \(non-graceful\) shutdown, followed by a restart.|
|GracefulRestart|Perform a graceful shutdown followed by a restart of the system.|
|GracefulShutdown|Perform a graceful shutdown. Graceful shutdown involves shutdown of the operating system followed by the power off of the physical server.|
|Nmi|Generate a Diagnostic Interrupt \(usually an NMI on x86 systems\) to cease normal operations, perform diagnostic actions and typically halt the system.|
|On|Turn on the unit.|
|PowerCycle|Perform a power cycle of the unit.|
|PushPowerButton|Simulate the pressing of the physical power button on this unit.|

> Sample response header \(HTTP 202 status\)

```
Connection:keep-alive
Content-Type:application/json; charset=utf-8
**Location:/taskmon/task85de4103-8757-4c7d-942f-55eaf7d6412a**
Odata-Version:4.0
X-Frame-Options:sameorigin
Date:Sun,17 May 2020 14:35:32 GMT+5m 13s
Content-Length:491 bytes

```

> Sample response body \(HTTP 202 status\)

```
{
   "@odata.type":"#Task.v1_4_2.Task",
   **"@odata.id":"/redfish/v1/TaskService/Tasks/task85de4103-8757-4c7d-942f-55eaf7d6412a",**
   "@odata.context":"/redfish/v1/$metadata#Task.Task",
   **"Id":"task85de4103-8757-4c7d-942f-55eaf7d6412a",**
   "Name":"Task task85de4103-8757-4c7d-942f-55eaf7d6412a",
   "Message":"The task with id task85de4103-8757-4c7d-942f-55eaf7d6412a has started.",
   "MessageId":"TaskEvent.1.0.1.TaskStarted",
   "MessageArgs":[
      "task85de4103-8757-4c7d-942f-55eaf7d6412a"
   ],
   "NumberOfArgs":1,
   "Severity":"OK"
}
```

> Sample response body \(subtask\)

```
{
   "@odata.type":"#SubTask.v1_4_2.SubTask",
   "@odata.id":"/redfish/v1/TaskService/Tasks/task85de4003-8757-4c7d-942f-55eaf7d6412a/SubTasks/task22a98864-5dd8-402b-bfe0-0d61e265391e",
   "@odata.context":"/redfish/v1/$metadata#SubTask.SubTask",
   "Id":"task22a98864-5dd8-402b-bfe0-0d61e265391e",
   "Name":"Task task22a98864-5dd8-402b-bfe0-0d61e265391e",
   "Message":"Successfully Completed Request",
   "MessageId":"Base.1.6.1.Success",
   "Severity":"OK",
   "Members@odata.count":0,
   "Members":null,
   "TaskState":"Completed",
   "StartTime":"2020-05-13T13:33:59.917329733Z",
   "EndTime":"2020-05-13T13:34:00.320539988Z",
   "TaskStatus":"OK",
   "SubTasks":"",
   **"TaskMonitor":"/taskmon/task22a98864-5dd8-402b-bfe0-0d61e265391e",**
   "PercentComplete":100,
   "Payload":{
      "HttpHeaders":null,
      "HttpOperation":"POST",
      "JsonBody":"",
      "TargetUri":"/redfish/v1/Systems/97d08f36-17f5-5918-8082-f5156618f58d:1"
   },
   "Messages":null
}
```

> Sample response body \(HTTP 200 status\)

```
{ 
   "error":{ 
      "code":"Base.1.6.1.Success",
      "message":"Request completed successfully"
   }
}
```

## Changing the boot order of servers to default settings

```
curl -i POST \
   -H "X-Auth-Token:{X-Auth-Token}" \
   -H "Content-Type:application/json" \
   -d \
'{ 
   "parameters":{ 
      "ServerCollection":[ 
         "/redfish/v1/Systems/97d08f36-17f5-5918-8082-f5156618f58d:1",
         "/redfish/v1/Systems/76632110-1c75-5a86-9cc2-471325983653:1"
      ]
   }
}' \
 'https://\{odim\_host\}:\{port}/redfish/v1/AggregationService/Actions/AggregationService.SetDefaultBootOrder'


```


|||
|---------|---------------|
|**Method** | `POST` |
|**URI** |`/redfish/v1/AggregationService/Actions/AggregationService.SetDefaultBootOrder` |
|**Returns** |<ul><li>`Location` URI of the task monitor associated with this operation in the response header. </li><li>Link to the task and the task Id in the sample response body. To get more information on the task, perform HTTP `GET` on the task URI.<br>**IMPORTANT:**<br> Note down the task Id. If the task completes with an error, it is required to know which subtask has failed. To get the list of subtasks, perform HTTP `GET` on `/redfish/v1/TaskService/Tasks/{taskId}`.<br></li><li> On successful completion of this operation, a message in the response body, saying that the operation is completed successfully. See "Sample response body \(HTTP 200 status\)".</li></ul> |
|**Response code** |<ul><li>`202 Accepted`</li><li>`200 Ok`</li></ul>|
|**Authentication** |Yes|


<br>
**Description**

This action changes the boot order of one or more servers to default settings. 


It is performed as a task and is further divided into subtasks to change the boot order of each server individually. To know the progress of this action, perform HTTP `GET` on the [task monitor](#task-monitor) returned in the response header \(until the task is complete\).

To get the list of subtask URIs, perform HTTP `GET` on the task URI returned in the JSON response body. See "Sample response body \(HTTP 202 status\)". The JSON response body of each subtask contains a link to the task monitor associated with it. To know the progress of `SetDefaultBootOrder` action \(subtask\) on a specific server, perform HTTP `GET` on the task monitor associated with the respective subtask.




You can perform `setDefaultBootOrder` action on a group of servers by specifying multiple server URIs in the request.




> Sample request body

```
{ 
   "parameters":{ 
      "ServerCollection":[ 
         "/redfish/v1/Systems/97d08f36-17f5-5918-8082-f5156618f58d:1",
         "/redfish/v1/Systems/76632110-1c75-5a86-9cc2-471325983653:1"
      ]
   }
}

```

### Request parameters

|Parameter|Type|Description|
|---------|----|-----------|
|parameters\[\{|Array|The parameters associated with the `SetDefaultBootOrder` action.|
|ServerCollection\}\]| \(required\)<br> |Target servers for `SetDefaultBootOrder`.|


> Sample response header \(HTTP 202 status\)

```
Connection:keep-alive
Content-Type:application/json; charset=utf-8
**Location:/taskmon/task85de4003-8057-4c7d-942f-55eaf7d6412a**
Odata-Version:4.0
X-Frame-Options:sameorigin
Date:Sun,17 May 2020 14:35:32 GMT+5m 13s
Content-Length:491 bytes

```

> Sample response body \(HTTP 202 status\)

```
{
   "@odata.type":"#Task.v1_4_2.Task",
   **"@odata.id":"/redfish/v1/TaskService/Tasks/task85de4003-8057-4c7d-942f-55eaf7d6412a",**
   "@odata.context":"/redfish/v1/$metadata#Task.Task",
   **"Id":"task85de4003-8057-4c7d-942f-55eaf7d6412a",**
   "Name":"Task task85de4003-8057-4c7d-942f-55eaf7d6412a",
   "Message":"The task with id task80de4003-8757-4c7d-942f-55eaf7d6412a has started.",
   "MessageId":"TaskEvent.1.0.1.TaskStarted",
   "MessageArgs":[
      "task80de4003-8757-4c7d-942f-55eaf7d6412a"
   ],
   "NumberOfArgs":1,
   "Severity":"OK"
}
```

> Sample response body \(subtask\)

```
{
   "@odata.type":"#SubTask.v1_4_2.SubTask",
   "@odata.id":"/redfish/v1/TaskService/Tasks/task85de4003-8757-4c7d-942f-55eaf7d6412a/SubTasks/task22a98864-5dd8-402b-bfe0-0d61e265391e",
   "@odata.context":"/redfish/v1/$metadata#SubTask.SubTask",
   "Id":"task22a98864-5dd8-402b-bfe0-0d61e265391e",
   "Name":"Task task22a98864-5dd8-402b-bfe0-0d61e265391e",
   "Message":"Successfully Completed Request",
   "MessageId":"Base.1.6.1.Success",
   "Severity":"OK",
   "Members@odata.count":0,
   "Members":null,
   "TaskState":"Completed",
   "StartTime":"2020-05-13T13:33:59.917329733Z",
   "EndTime":"2020-05-13T13:34:00.320539988Z",
   "TaskStatus":"OK",
   "SubTasks":"",
   **"TaskMonitor":"/taskmon/task22a98864-5dd8-402b-bfe0-0d61e265391e",**
   "PercentComplete":100,
   "Payload":{
      "HttpHeaders":null,
      "HttpOperation":"POST",
      "JsonBody":"",
      "TargetUri":"/redfish/v1/Systems/97d08f36-17f5-5918-8082-f5156618f58d:1"
   },
   "Messages":null
}
```

> Sample response body \(HTTP 200 status\)

```
{ 
   "error":{ 
      "code":"Base.1.6.1.Success",
      "message":"Request completed successfully"
   }
}
```


## Deleting the server inventory

```
curl -i POST \
   -H "X-Auth-Token:{X-Auth-Token}" \
   -H "Content-Type:application/json" \
   -d \
'{
"parameters": [
{
"Name": "/redfish/v1/Systems/{ComputerSystemId}"
}
]
}' \
 'https://\{odim\_host\}:\{port}/redfish/v1/AggregationService/Actions/AggregationService.Remove/'


```


|||
|---------|---------------|
|**Method** |`POST` |
|**URI** |`/redfish/v1/AggregationService/Actions/AggregationService.Remove` |
|**Returns** |<ul><li>`Location` URI of the task monitor associated with this operation in the response header.</li><li>Link to the task and the task Id in the sample response body. To get more information on the task, perform HTTP `GET` on the task URI. See "Sample response body \(HTTP 202 status\)".</li><li>On successful completion, a message in the response body, saying that the operation is completed successfully. See "Sample response body \(HTTP 200 status\)".</li></ul> |
|**Response code** |<ul><li>`202 Accepted`</li><li>`200 Ok`</li></ul> |
|**Authentication** |Yes|


<br>
**Description**

This action removes the inventory of a specific server and deletes all associated event subscriptions. 


It is performed as a task. To know the progress of this action, perform `GET` on the [task monitor](#task-monitor) returned in the response header \(until the task is complete\).




> Sample request body

```
{
   "parameters":[
      {
         "Name":"/redfish/v1/Systems/97d08f36-17f5-5918-8082-f5156618f58d:1"
      }
   ]
}
```

###  Request parameters

|Parameter|Type|Description|
|---------|----|-----------|
|parameters\[\{|Array|The parameters associated with the `Delete` Action.|
|Name\}\]|String \(required\)<br> |The URI of the target to be removed: `/redfish/v1/Systems/{ComputerSystemId}` |



> Sample response header \(HTTP 202 status\)

```
Connection:keep-alive
Content-Type:application/json; charset=utf-8
**Location:/taskmon/task85de4003-8757-2c7d-942f-55eaf7d6412a**
Odata-Version:4.0
X-Frame-Options:sameorigin
Date:Sun,17 May 2020 14:35:32 GMT+5m 13s
Content-Length:491 bytes

```

> Sample response body \(HTTP 202 status\)

```
{
   "@odata.type":"#Task.v1_4_2.Task",
   **"@odata.id":"/redfish/v1/TaskService/Tasks/task85de4003-8757-2c7d-942f-55eaf7d6412a",**
   "@odata.context":"/redfish/v1/$metadata#Task.Task",
   **"Id":"task85de4003-8757-2c7d-942f-55eaf7d6412a",**
   "Name":"Task task85de4003-8757-2c7d-942f-55eaf7d6412a",
   "Message":"The task with id task85de4003-8757-2c7d-942f-55eaf7d6412a has started.",
   "MessageId":"TaskEvent.1.0.1.TaskStarted",
   "MessageArgs":[
      "task85de4003-8757-2c7d-942f-55eaf7d6412a"
   ],
   "NumberOfArgs":1,
   "Severity":"OK"
}
```

> Sample response body \(HTTP 200 status\)

```
{ 
   "error":{ 
      "code":"ResourceEvent.1.0.2.ResourceRemoved",
      "message":"Request completed successfully"
   }
}
```



## Adding a plugin

```
curl -i POST \
   -H 'Authorization:Basic {base64_encoded_string_of_[username:password]}' \
   -H "Content-Type:application/json" \
   -d \
'{"ManagerAddress":"{BMC_address}:{port}",
  "UserName":"{plugin_userName}",
  "Password":"{plugin_password}",
  "Oem":{"PluginID":"{Redfish_PluginId}",
         "PreferredAuthType":"{Preferred_aunthentication_type}",
         "PluginType":"{plugin_type}"
        }
 }' \
 'https://{odim}:{port}/redfish/v1/AggregationService/Actions/AggregationService.Add'


```

|||
|---------|---------------|
|**Method** | `POST` |
|**URI** |`/redfish/v1/AggregationService/Actions/AggregationService.Add` |
|**Returns** |<ul><li>`Location` URI of the task monitor associated with this operation in the response header.</li><li>Link to the task and the task Id in the sample response body. To get more information on the task, perform HTTP `GET` on the task URI. See "Sample response body \(HTTP 202 status\)".</li><li>When the task is successfully complete, success message in the JSON response body.</li></ul>|
|**Response Code** |<ul><li>`202 Accepted`</li><li>`200 Ok`</li></ul>|
|**Authentication** |Yes|

<br>
**Description**

This action discovers information about a plugin and adds it in the inventory. 



It is performed as a task. To know the progress of this action, perform `GET` on the [task monitor](#task-monitor) returned in the response header \(until the task is complete\).


 

> Sample request body

```
{ 
   ​   "ManagerAddress":"{iLO_address}:45003",
   ​   "UserName":"abc",
   ​   "Password":"abc123",
   ​   "Oem":{ 
      ​      "PluginID":"ILO",
      ​      "PreferredAuthType":"BasicAuth(or)XAuthToken",
      ​      "PluginType":"RF-HPE"      ​
   }   ​
}
```

###  Request parameters

|Parameter|Type|Description|
|---------|----|-----------|
|ManagerAddress|String \(required\)<br> |A valid IP address or hostname and port of the baseboard management controller \(BMC\) where the plugin is installed. The default port for the plugin for HPE iLO is `45003`.<br>**NOTE:**<br> Ensure that the port is greater than `45000`.|
|Username|String \(required\)<br> |The plugin username. Example: Username for the HPE iLO plugin.|
|Password|String \(required\)<br> |The plugin password. Example: Password for the HPE iLO plugin.  <br> |
|PluginID|String \(required\)<br> |The id of the plugin you want to add. Example: GRF \(Generic Redfish Plugin\), ILO<br> |
|PreferredAuthType|String \(required\)<br> |Preferred authentication method to connect to the plugin - `BasicAuth` or `XAuthToken`.|
|PluginType|String \(required\)<br> |The string that represents the type of the plugin. Allowed values: `RF-GENERIC`, `RF-HPE`, and `RF-CFM` <br> |


> Sample response header \(HTTP 202 status\)

```
Connection:keep-alive
Content-Type:application/json; charset=utf-8
**Location:/taskmon/task85de4003-8757-4c7d-942f-55eaf7d6812a**
Odata-Version:4.0
X-Frame-Options:sameorigin
Date:Sun,17 May 2020 14:35:32 GMT+5m 13s
Content-Length:491 bytes

```

> Sample response body \(HTTP 202 status\)

```
{
   "@odata.type":"#Task.v1_4_2.Task",
   **"@odata.id":"/redfish/v1/TaskService/Tasks/task85de4003-8757-4c7d-942f-55eaf7d6812a",**
   "@odata.context":"/redfish/v1/$metadata#Task.Task",
   **"Id":"task85de4003-8757-4c7d-942f-55eaf7d6812a",**
   "Name":"Task task85de4003-8757-4c7d-942f-55eaf7d6812a",
   "Message":"The task with id task85de4003-8757-4c7d-942f-55eaf7d6812a has started.",
   "MessageId":"TaskEvent.1.0.1.TaskStarted",
   "MessageArgs":[
      "task85de4003-8757-4c7d-942f-55eaf7d6812a"
   ],
   "NumberOfArgs":1,
   "Severity":"OK"
}
```

> Sample response body \(HTTP 200 status\)

```
{
   "code":"Base.1.6.1.Success",
   "message":"Request completed successfully."
} 
```

## Removing a plugin

```
curl -i POST \
   -H "X-Auth-Token:{X-Auth-Token}" \
   -H "Content-Type:application/json" \
   -d \
'{
"parameters": [{
"Name": "/redfish/v1/Managers/{managerId}"
}
]
}' \
 'https://\{odim\_host\}:\{port\}/redfish/v1/AggregationService/Actions/AggregationService.Remove'


```


|||
|---------|---------------|
|**Method** |`POST` |
|**URI** |`/redfish/v1/AggregationService/Actions/AggregationService.Remove` |
|**Returns** | <ul><li>`Location` URI of the task monitor associated with this operation in the response header.</li><li>Link to the task and the task Id in the sample response body. To get more information on the task, perform HTTP `GET` on the task URI. See "Sample response body \(HTTP 202 status\)".</li><li>On successful completion, a message in the response body, saying that the operation is completed successfully. See "Sample response body \(HTTP 200 status\)".</li></ul>|
|**Response Code** |<ul><li>`202 Accepted`</li><li>`200 OK`</li></ul> |
|**Authentication** |Yes|


<br>
**Description**

This action removes the inventory of a specific plugin. 

It is performed as a task. 
To know the progress of this action, perform `GET` on the [task monitor](#task-monitor) returned in the response header \(until the task is complete\).

<aside class="notice">
Before removing the plugin, ensure that the plugin container is stopped. To know how to stop a container, refer to "Shutting down and restarting Docker containers" in <em>HPE Resource Aggregator for Open Distributed Infrastructure Management™ Getting Started Guide</em>.
</aside>





> Sample request body

```
{
	"parameters": [
		{
	       "Name": "/redfish/v1/Managers/a6ddc4c0-2568-4e16-975d-fa771b0be853"
        }
	]
}
```

###  Request parameters

|Parameter|Type|Description|
|---------|----|-----------|
|parameters\[\{|Array|The parameters associated with the `Delete` Action.|
|Name\}\]|String \(required\)<br> |The URI of the target to be removed: `/redfish/v1/Managers/{managerId}` |



> Sample response header \(HTTP 202 status\)

```
Connection:keep-alive
Content-Type:application/json; charset=utf-8
**Location:/taskmon/task85de4003-8757-4c7d-941f-55eaf7d6412a**
Odata-Version:4.0
X-Frame-Options:sameorigin
Date:Sun,17 May 2020 14:35:32 GMT+5m 13s
Content-Length:491 bytes

```

> Sample response body \(HTTP 202 status\)

```
{
   "@odata.type":"#Task.v1_4_2.Task",
   **"@odata.id":"/redfish/v1/TaskService/Tasks/task85de4003-8757-4c7d-941f-55eaf7d6412a",**
   "@odata.context":"/redfish/v1/$metadata#Task.Task",
   **"Id":"task85de4003-8757-4c7d-941f-55eaf7d6412a",**
   "Name":"Task task85de4003-8757-4c7d-941f-55eaf7d6412a",
   "Message":"The task with id task85de4003-8757-4c7d-941f-55eaf7d6412a has started.",
   "MessageId":"TaskEvent.1.0.1.TaskStarted",
   "MessageArgs":[
      "task85de4003-8757-4c7d-941f-55eaf7d6412a"
   ],
   "NumberOfArgs":1,
   "Severity":"OK"
}
```

> Sample response body \(HTTP 200 status\)

```
{ 
   "error":{ 
      "code":"ResourceEvent.1.0.2.ResourceRemoved",
      "message":"Request completed successfully"
   }
}
```




# Message registries

A `MessageRegistry` represents the properties for a message registry.

A message registry is an array of messages and their attributes organized by `MessageId`. Each entry has

-   Description

-   The message this Id translates to.

-   Severity

-   The number and type of arguments

-   Proposed resolution


The arguments are the substitution variables for the message. The `MessageId` is formed according to the Redfish specification. It consists of the `RegistryPrefix` concatenated with the version and the unique identifier for the message registry entry.

<aside class="notice">
To access Redfish message registries, ensure that you have a minimum privilege of `Login`. If you do not have the necessary privileges, you will receive an HTTP `403 Forbidden` error.
</aside>



**Supported endpoints**

|||
|-------|--------------------|
|/redfish/v1/Registries|`GET`|
|/redfish/v1/Registries/\{registryId\}|`GET`|
|/redfish/v1/registries/\{registryFileId\}|`GET`|



#  Viewing a collection of registries

```
curl -i GET \
   -H "X-Auth-Token:{X-Auth-Token}" \
 'https://{odim_host}:{port}/redfish/v1/Registries'

```

> Sample response body

```
{
   "@odata.context":"/redfish/v1/$metadata#MessageRegistryFileCollection.MessageRegistryFileCollection",
   "@odata.id":"/redfish/v1/Registries/",
   "@odata.type":"#MessageRegistryFileCollection.MessageRegistryFileCollection",
   "Name":"Registry File Repository",
   "Description":"Registry Repository",
   "Members@odata.count":27,
   "Members":[
      {
         "@odata.id":"/redfish/v1/Registries/Base.1.0.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Base.1.2.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Base.1.3.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Base.1.3.1"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Base.1.4.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Base.1.5.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Base.1.6.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Base.1.6.1"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Composition.1.0.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Redfish_1.0.1_PrivilegeRegistry"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Redfish_1.0.2_PrivilegeRegistry"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Redfish_1.0.3_PrivilegeRegistry"
      },
      {
         "@odata.id":"/redfish/v1/Registries/Redfish_1.0.4_PrivilegeRegistry"
      },
      {
         "@odata.id":"/redfish/v1/Registries/ResourceEvent.1.0.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/ResourceEvent.1.0.1"
      },
      {
         "@odata.id":"/redfish/v1/Registries/ResourceEvent.1.0.2"
      },
      {
         "@odata.id":"/redfish/v1/Registries/TaskEvent.1.0.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/TaskEvent.1.0.1"
      },
      {
         "@odata.id":"/redfish/v1/Registries/HpeCommon.2.0.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/BiosAttributeRegistryA40.v1_1_46"
      },
      {
         "@odata.id":"/redfish/v1/Registries/HpeDcpmmDiags.1.0.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/%23SmartStorageMessages.v2_0_1.SmartStorageMessages"
      },
      {
         "@odata.id":"/redfish/v1/Registries/iLO.2.13.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/HpeBiosMessageRegistry.v1_0_0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/iLOEvents.2.1.0"
      },
      {
         "@odata.id":"/redfish/v1/Registries/BiosAttributeRegistryU32.v1_2_00"
      },
      {
         "@odata.id":"/redfish/v1/Registries/BiosAttributeRegistryU30.v1_2_00"
      }
   ]
}
	
```

|||
|------|--------|
|**Method** |`GET` |
|**URI** |``/redfish/v1/Registries`` |
|**Description** |A collection of Redfish-provided registries and custom registries.|
|**Returns** |Links to the registry instances.|
|**Response code** |On success, `200 OK` |
|**Authentication** |Yes|



#  Viewing a single registry

```
curl -i GET \
   -H "X-Auth-Token:{X-Auth-Token}" \
 'https://{odim_host}:{port}/redfish/v1/Registries/{registryId}'

```

> Sample response body

```
{
   "Id":"Base.1.6.1",
   "@odata.context":"/redfish/v1/https://10.24.1.95:45000/redfish/v1/$metadata#MessageRegistryFile.MessageRegistryFile",
   "@odata.id":"/redfish/v1/Registries/Base.1.6.1",
   "@odata.type":"#MessageRegistryFile.v1_1_3.MessageRegistryFile",
   "Name":"Registry File Repository",
   "Description":"Base Message Registry File Locations",
   "Languages":[
      "en"
   ],
   "Location":[
      {
         "Language":"en",
         "Uri":"/redfish/v1/registries/Base.1.6.1.json"
      }
   ],
   "Registry":"Base.1.6.1"
}
```

|||
|------|--------|
|**Method** |`GET` |
|**URI** |``/redfish/v1/Registries/{registryId}`` |
|**Description** |A single registry.|
|**Returns** |Link to the file inside this registry.|
|**Response code** |On success, `200 OK` |
|**Authentication** |Yes|


# Viewing a file in a registry

```
curl -i GET \
   -H "X-Auth-Token:{X-Auth-Token}" \
 'https://{odim_host}:{port}/redfish/v1/registries/{jsonFileId}'

```


|||
|------|--------|
|**Method** |`GET` |
|**URI** |``/redfish/v1/registries/{registryFileId}`` |
|**Description** |A file in a registry.|
|**Returns** |Content of this file.|
|**Response code** |On success, `200 OK` |
|**Authentication** |Yes|



#  Managing tasks

A task represents an operation that takes more time than a user typically wants to wait and is carried out asynchronously.

An example of a task is resetting an aggregate of servers. Resetting all the servers in a group is a time-consuming operation; the user waiting for the result would be blocked from performing other operations. FODIM creates Redfish tasks for such long-duration operations and offers an interface for the users to manage them while performing other operations.

The FODIM exposes Redfish TaskService APIs and Task monitor API. Use these endpoints to manage and poll tasks for their completion.

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

The FODIM offers an eventing interface that allows northbound clients to interact and receive notifications such as alarms from southbound equipment. It exposes Redfish EventService APIs as an interface for managing events.

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

The FODIM allows northbound clients to access the inventory of southbound compute, network, and storage resources including chassis by exposing Redfish SystemService endpoints. It also offers the capability to search the inventory based on specific resource parameters.

Use these endpoints to discover information on all the telemetry data you want to monitor such as CPU utilization, memory utilization, processor power consumption, thermal health.

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
