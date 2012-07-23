abs-api
=======

API Documentation for the Always Be Social Platform

Always Be Social API Documentation


The Always Be Social Platform is a complete suite of tools designed to help you reach your community wherever they are. Through our API, third-party applications can take advantage of our implementation of several social features, while leveraging our robust data collection and reporting infrastructure.

This guide will walk you through the features currently available through our API and is divided into two sections. The first provides a quick overview of our Javascript API, while the second details how you can customize and fully embed an ABS Module into your application. We appreciate and welcome any feedback or requests to expose additional ABS Platform functionality through the API.


Javascript SDK

The Javascript SDK provides a set of client-side calls for integrating a custom external application into the ABS Platform.

Initializing the SDK
To initialize the API, include the Javascript SDK into your web application

http://platform.alwaysbesocial.com/js/api.js

The SDK inserts a div element with the ID abs-root, containing an iframe with the ID abs-frame, into your web application’s source. The ABS object included in the SDK provides access to the various API requests describe in the section below.

Authentication
Please contact your account manager to obtain an authorization key required to initialize the API.

Example

ABS.init({
  apiKey: '1a1e31b4d4a3b5191ca22a6919ade490'
}, function(response) {
	if(response.success) {
		alert(‘Successfully initialized the API’);
	}
	else {
		alert(response.message);
}
});

Facebook Connect
Provides access to the ABS Platform’s implementation of various Facebook API requests.

Example 

	ABS.api(‘initFacebook’,  { 
appId: ‘YOUR_FACEBOOK_APP_ID’ 
}, function(response) { 
//handle response 
});

Register FB Open Graph Action
Register an Open Graph action to be used with your custom application

Example 

	ABS.api(‘registerAction’,  { 
appId: ‘FACEBOOK_APP_ID’,
fb_namespace:  ‘FACEBOOK_APP_NAMESPACE’,
fb_action: ‘FACEBOOK_ACTION_NAME,
fb_object: ‘FACEBOOK_ACTION_OBJECT’,
label:  ‘ALIAS_FOR_ACTION’ /* i.e. hiked a trail */  

}, function(response) { 
//JSON response containing ABS action type ID
//i.e. { success:1, id:45 }
});

Publish FB Open Graph Action
Publish an open graph action triggered from you custom application

Example

	ABS.api(‘registerAction’,  { 
fb_action_type_id: ‘FACEBOOK_APP_ID’,
object_id:  ‘UNIQUE_OBJECT_ID_IN_CUSTOM_APP’,
object_name:  ‘UNIQUE_OBJECT_NAME_IN_CUSTOM_APP’

}, function(response) {
//JSON response containing ABS action ID
//i.e. { success:1, id:1298 }
});

Set Custom App Analytics
Allows custom applications to push object level analytics to the ABS Platform

Example

	ABS.api(‘setAnalytics’,  { 
action_type_id: ‘ANALYTICS_ACTION_TYPE’, 
/* 1 = viewed    2 = clicked    3 = time spent    4 =  share / like */
object_id:  ‘UNIQUE_OBJECT_ID_IN_CUSTOM_APP’
}, function(response) {
//JSON response, i.e. { success:1 }
});

Get Custom App Analytics
Retrieve your application’s ABS and Facebook analytics

Example

	ABS.api(‘getAnalytics’, function(response) {
//JSON response containing header level data, array of object level data, 
//and Facebook Insights data
});

Analytics Header Fields

Field	Description	Type
avg_time_spent	Average time spent in app	Decimal
num_actions	Total actions published from app	Integer
num_views	Total times app has been loaded	Integer

Analytics Object Level Fields

Field	Description	Type
avg_time_spent	Average time spent in object	Decimal
num_views	Total views of objects	Integer
num_clicks	Total clicks of links / buttons within object	Integer
num_shares	Total shares / likes of object	Integer
actions	Contains all custom action stats for object	JSON Object

Facebook Insights

Please visit the following link for a list of all stats available through the Facebook Insights API:
https://developers.facebook.com/docs/reference/fql/insights/


Map Module API

This API provides access to the ABS Platform’s Map Module. This API resides outside of the Javascript SDK and is accessed via direct URL calls, with all fields set using key/value query string parameters.

All requests require your map’s unique authorization token (field ‘token’) in addition to the specific fields listed below for each request.

Display Map
http://platform.alwaysbesocial.com/api/YOUR_MAP_IDENTIFIER

Field	Description	Type
w	Map width	Integer
h	Map height	Integer
lt	Map center latitude position	Decimal
ln	Map center longitude position	Decimal
z	Zoom level of map	Integer
m	Comma delimited list of the IDs of markers to show on the map. If excluded, all map markers will be shown. Use a value of -1 to show no markers	Integers


Create Marker
http:// platform.alwaysbesocial.com/api.php?action=externalMap.createMarker

Field	Description	Type
lat	The marker’s latitude position	Decimal
lng	The marker’s longitude position	Decimal
address	The address to show in the map’s info window	String
title	The title to show in the marker’s info window	String
copy	The description to show in the marker’s info window	String
link	URL of a link to include in the marker’s info window	String
icon_id	The marker icon’s image ID	Integer

Returns a JSON object containing the marker’s ID – { success:1, marker_id:243 }

Update Marker
http://platform.alwaysbesocial.com/api.php?action=externalMap.updateMarker

Field	Description	Type
lat	The marker’s latitude position	Decimal
lng	The marker’s longitude position	Decimal
address	The address to show in the map’s info window	String
title	The title to show in the marker’s info window	String
copy	The description to show in the marker’s info window	String
link	URL of a link to include in the marker’s info window	String
icon_id	The marker icon’s image ID	Integer
marker_id	The marker’s ID	Integer

Returns a JSON object containing the marker’s ID – { success:1, marker_id:243 }

Remove Marker
http://platform.alwaysbesocial.com/api.php?action= externalMap.destroyMarker

Field	Description	Type
marker_id	The marker’s ID	Integer

Returns a JSON object – { success:1 }

Remove All Markers
http://platform.alwaysbesocial.com/api.php?action=externalMap.destroyAllMarkers

Returns a JSON object – { success:1 }

Create Review
http://platform.alwaysbesocial.com/api.php?action=externalMap.createReview

Field	Description	Type
review_id	The review’s ID	Integer
marker_id	The ID of the marker being reviewed	Integer
review	The review message	String
rating	Rating for review 1 – 5	Integer

Returns a JSON object containing the review’s ID – { success:1, review_id:243 }

Update Review
http://platform.alwaysbesocial.com/api.php?action=externalMap.updateReview

Field	Description	Type
marker_id	The ID of the marker being reviewed	Integer
review	The review message	String
rating	Rating for review 1 – 5	Integer

Returns a JSON object containing the review’s ID – { success:1, review_id:243 }

Remove Review
http://platform.alwaysbesocial.com/api.php?action=externalMap.destroyReview

Field	Description	Type
review_id	The review’s ID	Integer

Returns a JSON object – { success:1 }

