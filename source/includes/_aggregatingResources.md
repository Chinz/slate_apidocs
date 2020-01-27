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


