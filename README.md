# Beaconstac REST API documentation

## Introduction
The Beaconstac API is organized around [REST](https://en.wikipedia.org/wiki/Representational_state_transfer). Our API has predictable resource-oriented URLs, accepts [form-encoded](https://en.wikipedia.org/wiki/POST_(HTTP)#Use_for_submitting_web_forms) request bodies, returns [JSON-encoded](http://www.json.org/) responses, and uses standard HTTP response codes, authentication, and verbs.

## Authentication
The Beaconstac API uses API key to authenticate requests.

Your API key carry many privileges, so be sure to keep them secure! Do not share your secret API keys in publicly accessible areas such as GitHub, client-side code, and so forth.

All API requests must be made over HTTPS. Calls made over plain HTTP will fail. API requests without authentication will also fail.

You can find your developer token by using the following steps.
1. Login to the [Beaconstac dashboard](https://dashboard.beaconstac.com/).
2. Go to your 'Account' section using the drop-down on the top-right.
3. In the 'Account Details' section, copy the 'Developer Token' value.
4. Add a header field with key `Authorization` and value as `Token <YOUR_TOKEN>`

![Account Page](https://github.com/Beaconstac/api/blob/master/images/account_page.png)

## Reading the Documentation
#### Filter fields
The API supports filters on fields specified in each object collections' `List` request. To use the filter add the filter name along with the operator and then the value in the request params. 

Filter fields supported:
1. `exact`: default. Based on exact match
2. `icontains`: Case insensitive contains
3. `lt`: less than match
4. `gt`: greater than match
5. `lte`: less than equal to match
6. `gte`: greater than equal to match

Example:
If a object states that the filter arguments: 
1. `name`: `exact`, `icontains`
2. `place__name`: `exact`, `icontains`

Some of the ways you can use filtering
1. `?name='test_beacon'` by default if no filter is specified for a param it defaults to `exact`
2. `?name__icontains='test'` case insensitive filter
3. `?name='test_beacon'&place__name__icontains='office'` Does a filter for all objects with name 'test_beacon' and who have been attached to places whose name inturn contains (case insensitive) 'office' in it

*`organization` `exact` filter is supported across all objects. To get objects from all your organizations(including child organizations) use 'all' as the filter value*

#### Searching
The API supports search filters on fields specified in each object collections' `List` request. To use the search filter specify the value in the request params and the response will be objects that match value with any of the search fields mentioned in the objects' `List` request documentation. The Searches will use case-insensitive partial matches.

Example:
If a object states that the search fields supported are: 
1. `name`
2. `place__name`

Some of the ways you can use filtering
1. `?search='office'` Will search both the name of the object and attached places' name objects for the text 'office' and return the results

#### Ordering
The API supports ordering on fields specified in each object collections' `List` request. To use the ordering feature specify the parameter to order by in the request params. Ordering is done in an ascending order by default.

Example:
If a object states that the ordering fields supported are: 
1. `name`

Some of the ways you can use filtering
1. `?ordering='name'` Will order by name by ascending order
2. `?ordering='-name'` Will order by name in decending order

## Core Resources
### Place
Place objects allow you to view all places in your account and view beacons attached to them.

#### Place object
***Attributes***

| Field | Type | Required | Read only | Description |
|---|---|---|---|---|
| `id` | `integer` |  `false` | `true`  | Unique identifier for the object |
| `name` | `string` |  `true` | `false`  | The name of the place |
| `organization` | `integer` |  `true` | `true`  | Organization id |
| `latitude` | `float` |  `false` | `false`  | Latitude |
| `longitude` | `float` |  `false` | `false`  | Longitude |
| `place_id` | `string` |  `false` | `false`  | Google Place ID |
| `address` | `string` |  `false` | `false`  | Address of the place |
| `beacons` | `list[object]` |  `false` | `true`  | Beacon data associated with place id. Only available in `List` |
| `beacon_count` | `integer` |  `false` | `true`  | Beacon count |
| `default_place` | `boolean` |  `false` | `true`  | Indicated whether the place is a default place |
| `business_icon_url` | `string` |  `false` | `false`  | Business Icon URL (Used in NearBee) |
| `business_cover_url` | `string` |  `false` | `false`  | Business Cover URL(Used in NearBee) |
| `business_color` | `string` |  `false` | `false`  | Business Color(Used in NearBee) |
| `created` | `timestamp` |  `false` | `true`  | Created timestamp |
| `updated` | `timestamp` |  `false` | `true`  | Last updated timestamp |

#### List Places
Returns a list of your beacons. The beacons are returned sorted by beacon heartbeat, with the most recent detected beacons appearing first.

`GET https://beaconstac.mobstac.com/api/2.0/places/`

Filter arguments:
1. `name`: `exact`, `icontains`

Search Fields:
1. `name`

Ordering fields:
1. `name`
2. `created`
3. `updated` - default
4. `address`

## Partner API endpoints

[Creating user accounts in the Beaconstac store](https://github.com/Beaconstac/api/blob/master/storeUserCreate.md)

