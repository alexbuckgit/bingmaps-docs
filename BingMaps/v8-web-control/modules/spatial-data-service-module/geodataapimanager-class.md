---
title: "GeoDataAPIManager Class | Microsoft Docs"
description: Describes the GeoDataAPIManager class, a static class that provides the ability to request polygons that describe the boundaries of geographic entities, and details its static method, network status parameter, and troubleshooting tips.
ms.custom: ""
ms.date: "02/28/2018"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: c02f74bb-9214-47fe-a535-749d5354bb0b
caps.latest.revision: 9
author: "rbrundritt"
ms.author: "richbrun"
manager: "stevelom"
ms.service: "bing-maps"
---

# GeoDataAPIManager Class

[!INCLUDE [bing-maps-web-control-sdk-retirement](../../../includes/bing-maps-web-control-sdk-retirement.md)]

This is a static class that provides the ability to request polygons that describe the boundaries of a geographic entities, such as an AdminDivision1 (such as a state or province) or a Postcode1 (such as a zip code) that contain a given point (latitude and longitude) or address. This uses the [GeoData API](../../../spatial-data-services/geodata-api.md) in the Bing Spatial Data Services.

## Static Methods

Name                    | Definition | Description
----------------------- | ---------- | --------------------------------
`getBoundary`           | getBoundary(<br/>locations: string _or_ [Location](../../map-control-api/location-class.md) _or_ (string _or_ [Location](../../map-control-api/location-class.md))[], request: [GetBoundaryRequestOptions](getboundaryrequestoptions-object.md), credentials: string _or_ [Map](../../map-control-api/map-class.md), callback: function(results: [GeoDataResultSet](geodataresultset-object.md)), styles?: [PolygonOptions](../../map-control-api/polygonoptions-object.md), errorCallback?: function(locationValue: string _or_ [Location](../../map-control-api/location-class.md) , networkStatus: string)) | Gets a boundary for the specified request. Takes in location which could be a Location coordinate or a string address, or an array of either of these. A Bing Maps key or a reference to a map control is used for authentication. A callback function is used to return the results to you.<br/><br/>If the location value is a string, it will be geocoded and the coordinates of the result will be used to find a boundary of the specified **entityType** that intersects with this coordinate.<br/><br/>Optionally polygon style options can be specified which will be used to style the boundary polygons returned by this API.<br/><br/>An error callback can be specified that will be triggered when an error occurs when searching for a boundary. The error callback will receive the location value that the error occurred for and a network status value. 

## Network Status Parameter

The networkStatus parameter in the error callback can have the following values.

| Parameter Value | Description                                         |
|-----------------|-----------------------------------------------------|
| notStarted      | The request has not been started.                   |
| pending         | The request is executed, but has not been received. |
| complete        | The request is complete.                            |
| error           | The request resulted in an error.                   |
| abort           | The request was aborted by a client action.         |
| timeout         | The request timed out.                              |

> [!NOTE]
> When passing in an array of locations into the `getBoundary` method, the callback function will be called multiple times, passing back the boundary information for each request. This is done so that you process the boundary data as it is returned rather than having to wait for all of the boundary requests to process.

> [!TIP]
> If your application is often loading the same boundaries, instead of using string queries, geocoding the queries ahead of time and store the location coordinates and use them with the service. This will speed up the response time of the GeoData API and will also reduce the number of geocoding transactions generated by your application.

## Troubleshooting

* If an error occurs when searching for a boundary, the callback function will be called with a [GeoDataResultSet object](geodataresultset-object.md) whose `results` property will be an empty array.
* When a string location is used to retrieve a boundary it is passed through the geocoding service. The location of the first result found by the geocoding service is then used to find a boundary of the specified entity type that intersects this location. This can sometimes return unexpected results. For instance, if you the following query is passed in "this is not a place, Florida". You may expect that this would not return any results, however, the geocoder will likely return a result for Florida, which is a reasonable geocoding result for this query. The location coordinates returned for the Florida result would then be used to retrieve a boundary of the specified entity type.
*  When using string locations, it is important to remember that not all queries are unique. This can sometimes lead to unexpected results. Here are a few common examples and how to address them:
    * Acronyms often have multiple meanings. For example, "CA" is the ISO2 value used for "Canada" and is also the FIPS code used for the "State of California". In this case using the full name would resolve this issue.
    * Some location names have multiple popular meanings. For example, "Washington" is both a state and a city in the US. There are a number of ways to address this. If searching for the state, you could use the FIPS code "WA" however this goes against the advice of the previous tip. Adding the word "state" to the query often helps (i.e. "Washington State"). Using "county" when working with counties also works well. If searching for the city, you can change the query to "Washington DC". 
	* Sometimes it may not be possible to change the query to make it more definitive. If you If the user types in their query, you may not have any additional context. For example, "London" could mean "London England" or one of the many cities around the world called "London". In this case a good option is to geocode the location outside of the GeoData API. This can be done using the [AutoSuggest](../autosuggest-module/index.md) or [Search](../search-module/index.md) modules in V8, or you can use the [REST geocoding service](../../../rest-services/index.md) if geocoding the data ahead of time. These solutions will often return multiple results from which you or the user can pick the location they want.
