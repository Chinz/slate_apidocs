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

# <a name="GUID-FD398AD7-F078-4D07-A529-0AC21975452F"/> Adding a Server

|<strong>Method</strong> |<strong>POST</strong> |
|<strong>URI</strong> |`/redfish/v1/AggregatorService/Actions/AggregationService.Add` |
|<strong>Returns</strong> |Location URI of the task monitor associated with this operation in the response header.|
|<strong>Response Code</strong> |202|

## <a name="SECTION_DF9D801BAD2C4FCB8868F370BECEF031"/> Description

This action adds a single server in the inventory. On success, ORCA specified Computer System Id is assigned to the added server.

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


