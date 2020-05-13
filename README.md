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

![Account Page](https://github.com/Beaconstac/api/blob/master/older/images/account_page.png)

## Table of Contents
1. [Reading the documentation](#reading-the-documentation)
2. [Beacon](#beacon)
3. [NFC Tag](#nfc-tag)
4. [QR Code](#qr-code)
5. [Bulk QR Code](#bulk-qr-codes)
6. [Geofence](#geofence)
7. [Place](#place)
8. [User](#user)

----
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

----
## Core Resources

### Beacon
`Beacon` objects allow you to perform actions on your beacons. You can retrieve individual beacons as well as a list of all your beacons or update a beacon.

#### Beacon object
***Attributes***

| Field | Type | Required | Read only | Description |
|---|---|---|---|---|
| `id` | `integer` |  `false` | `true`  | Unique identifier for the object |
| `name` | `string` |  `true` | `false`  | The name of the beacon |
| `UUID` | `string` |  `true` | `true`  | iBeacon UUID |
| `major` | `integer` |  `true` | `true`  | iBeacon major value |
| `minor` | `integer` |  `true` | `true`  | iBeacon minor value |
| `organization` | `integer` |  `true` | `true`  | Organization id |
| `place` | `integer` |  `true` | `false`  | Place id |
| `serial_number` | `string` |  `false` | `true`  | serial number |
| `eddystone_nid` | `string` |  `false` | `true`  | Eddystone namespace instance |
| `eddystone_bid` | `string` |  `false` | `true`  | Eddystone byte instance |
| `eddystone_url` | `string` |  `false` | `true`  | Eddystone URL(Deprecated) |
| `url` | `string` |  `false` | `true`  | Eddystone URL |
| `state` | `string` |  `false` | `true`  | State of the beacon (`A` Active, `S` Sleeping) |
| `place_data` | `list[object]` |  `false` | `true`  | Place data associated with place id. Only available in `List` |
| `tags` | `list[integer]` |  `false` | `false`  | List of tag ids associated |
| `tag_data` | `list[object]` |  `false` | `true`  | Tag data |
| `rules` | `list[integer]` |  `false` | `false`  | List of rule ids associated |
| `rule_data` | `list[object]` |  `false` | `true`  | Rule data |
| `mode` | `string` |  `false` | `true`  | Mode of the beacon |
| `tx_power` | `integer` |  `false` | `false`  | Transmission power |
| `advertising_interval` | `integer` |  `false` | `false`  | Advertising interval |
| `battery` | `integer` |  `false` | `false`  | Battery |
| `temperature` | `integer` |  `false` | `false`  | Temperature |
| `latitude` | `float` |  `false` | `false`  | Latitude |
| `longitude` | `float` |  `false` | `false`  | Longitude |
| `closeby_id` | `integer` |  `false` | `true`  | Closeby ID of the beacon(Deprecated) |
| `nearby_id` | `string` |  `false` | `true`  | Proximity ID of the beacon |
| `temperature` | `integer` |  `false` | `false`  | Temperature |
| `proximity_status` | `string` |  `false` | `true`  | Proximity status (Deprecated) |
| `meta` | `object` |  `false` | `false`  | Metadata associated with the beacon |
| `deployment_meta` | `object` |  `false` | `false`  | Deployment meta |
| `kit_type` | `object` |  `false` | `true`  | Beacon kit type (Deprecated) |
| `created` | `timestamp` |  `false` | `true`  | Created timestamp |
| `updated` | `timestamp` |  `false` | `true`  | Last updated timestamp |
| `heartbeat` | `timestamp` |  `false` | `true`  | Timestamp when the beacon was last detected |
| `campaign` | `Campaign` |  `false` | `false`  | `Campaign` object |
| `notifications` | `list[CampaignNotification]` |  `false` | `false`  | `CampaignNotification` object |

#### Campaign object
***Attributes***

| Field | Type | Required | Read only | Description |
|---|---|---|---|---|
| `id` | `integer` |  `false` | `true`  | Unique identifier for the object |
| `content_type` | `integer` |  `true` | `false`  | Campaign Type (0 for 'No campaign') |
| `custom_url` | `URL` |  `true` if `content_type` is 1 | `false`  | Custom URL |
| `markdown_card` | `integer` |  `true` if `content_type` is 2 | `false`  | Markdown card |
| `form` | `integer` |  `true` if `content_type` is 3 | `false`  | Form |
| `schedule` | `integer` |  `true` if `content_type` is 4 | `false`  | Schedule |
| `campaign_active` | `boolean` |  `false` | `true`  | Current campaign status |
| `organization` | `integer` |  `false` | `true`  | Organization |
| `created` | `timestamp` |  `false` | `true`  | Created timestamp |
| `updated` | `timestamp` |  `false` | `true`  | Last updated timestamp |

#### Campaign Notification object
***Attributes***

| Field | Type | Required | Read only | Description |
|---|---|---|---|---|
| `id` | `integer` |  `false` | `true`  | Unique identifier for the object |
| `language_code` | `string` |  `true` | `false`  | [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) language code. Each beacon can have only 1 language notification. |
| `is_default` | `boolean` |  `true` | `false`  | Is default notification. Each beacon can have only one notification set as default. |
| `title` | `string` |  `false` | `false`  | Title of the notification (max 49 characters) |
| `description` | `string` |  `false` | `false`  | Description on the notification (max 2048 characters) |
| `icon_url` | `URL` |  `false` | `false`  | Icon URL |
| `app_intent` | `string` |  `false` | `false`  | App intent |
| `banner_type` | `integer` |  `false` | `false`  | Banner type for NearBee |
| `banner_image_url` | `string` |  `false` | `false`  | Banner Image URL for NearBee |
| `meta` | `object` |  `false` | `false`  | Metadata |
| `slug` | `string` |  `false` | `true`  | Slug for the notification |
| `created` | `timestamp` |  `false` | `true`  | Created timestamp |
| `updated` | `timestamp` |  `false` | `true`  | Last updated timestamp |

#### List beacons
Returns a list of your beacons. The beacons are returned sorted by beacon `heartbeat`, with the most recent detected beacons appearing first.

`GET https://api.beaconstac.com/api/2.0/beacons/`

Filter arguments:
1. `name`: `exact`, `icontains`
2. `serial_number`:  `exact`, `icontains`
3. `place__name`: `exact`, `icontains`
4. `tags__name`: `exact`, `icontains`
5. `url`: `exact`
6. `heartbeat`: `lt`, `lte`, `gt`, `gte`
7. `campaign__content_type`: `exact` 
8. `state`: `exact`

Search Fields:
1. `name`
2. `serial_number`
3. `place__name`
4. `tags__name`
5. `url`
6. `campaign__content_type`

Ordering fields:
1. `name`
2. `serial_number`
3. `battery`
4. `heartbeat`: default
5. `place__name`
6. `created`
7. `updated`
8. `campaign__content_type`
9. `state`

#### Retrieve a beacon
Retrieves the details of an existing beacon. You need only supply the unique beacon identifier that was returned upon beacon listing.

`GET https://api.beaconstac.com/api/2.0/beacons/{beacon_id}`


#### Update beacon
Updates the specified beacon by setting the values of the parameters passed. Any parameters not provided will be left unchanged. However, the request should contain the required fields. Please refer to the Beacon object.

`PUT https://api.beaconstac.com/api/2.0/beacons/{beacon_id}`

Example:
Changing campaign to a Markdown Card
```json
{
    "id": 1189,
    "campaign": {
        "id": 223,
        "custom_url": "https://beaconstac.com",
        "content_type": 2,
        "campaign_active": true,
        "organization": 1284,
        "markdown_card": 434,
        "form": 7099,
        "schedule": null
    },
    "name": "Corner_",
    "url": "https://eddy.pro/f78WrG",
    "UUID": "F94DBB23-2266-7822-3782-57BEAC0952AC",
    "major": 16,
    "minor": 10,
    "serial_number": "0117C53F26D6",
    "eddystone_nid": "5DC33487F02E477D4058",
    "eddystone_bid": "0117C53F26D6",
    "place": 1929
}
```
----

### NFC Tag
`NFCTag` objects allow you to perform actions on your NFC Tags. You can retrieve individual nfc tags as well as a list of all your tags or update a tag.

#### NFC Tag object
***Attributes***

| Field | Type | Required | Read only | Description |
|---|---|---|---|---|
| `id` | `integer` |  `false` | `true`  | Unique identifier for the object |
| `name` | `string` |  `true` | `false`  | The name of the NFC Tag |
| `uid` | `string` |  `true` | `true`  | NFC Tag UID |
| `counter` | `integer` |  `false` | `true`  | NFC Tag counter. Counts the number of times the tag has been read |
| `organization` | `integer` |  `true` | `true`  | Organization id |
| `place` | `integer` |  `true` | `false`  | Place id |
| `url` | `string` |  `false` | `true`  | NFC Tag URL |
| `state` | `string` |  `false` | `true`  | State of the NFC Tag (`A` Active, `S` Sleeping) |
| `place_data` | `list[object]` |  `false` | `true`  | Place data associated with place id. Only available in `List` |
| `tags` | `list[integer]` |  `false` | `false`  | List of tag ids associated |
| `tag_data` | `list[object]` |  `false` | `true`  | Tag data |
| `meta` | `object` |  `false` | `false`  | Metadata associated with the NFC Tag |
| `created` | `timestamp` |  `false` | `true`  | Created timestamp |
| `updated` | `timestamp` |  `false` | `true`  | Last updated timestamp |
| `heartbeat` | `timestamp` |  `false` | `true`  | Timestamp when the NFC Tag was last detected |
| `campaign` | `Campaign` |  `false` | `false`  | `Campaign` object |


#### List NFC Tags
Returns a list of your NFC Tags. The tags are returned sorted by `updated`, with the most recently updated tags appearing first.

`GET https://api.beaconstac.com/api/2.0/nfctags/`

Filter arguments:
1. `name`: `exact`, `icontains`
2. `place__name`: `exact`, `icontains`
3. `tags__name`: `exact`, `icontains`
4. `url`: `exact`
5. `campaign__content_type`: `exact` 
6. `state`: `exact`

Search Fields:
1. `name`
2. `place__name`
3. `tags__name`
4. `url`
5. `campaign__content_type`

Ordering fields:
1. `name`
2. `place__name`
3. `created`
4. `updated` - default
5. `campaign__content_type`
6. `state`

#### Retrieve an NFC Tag
Retrieves the details of an existing NFC Tag. You need only supply the unique NFC Tag identifier that was returned upon tags listing.

`GET https://api.beaconstac.com/api/2.0/nfctags/{nfctag_id}`


#### Update an NFC Tag
Updates the specified NFC Tag by setting the values of the parameters passed. Any parameters not provided will be left unchanged. However, the request should contain the required fields. Please refer to the NFCTag object.

`PUT https://api.beaconstac.com/api/2.0/nfctags/{nfctag_id}`

Example:
Changing campaign to a Markdown Card
```json
{
    "id": 1189,
    "campaign": {
        "id": 223,
        "custom_url": "https://beaconstac.com",
        "content_type": 2,
        "campaign_active": true,
        "organization": 1284,
        "markdown_card": 434,
        "form": 7099,
        "schedule": null
    },
    "name": "Corner",
    "url": "https://nfc.tapnscan.me/f78WrG",
    "uid": "abc",
    "place": 1929
}
```
----
### QR Code
`QRCode` objects allow you to perform actions on your QR Codes. You can retrieve individual QR Codes as well as a list of all your qr codes or update a QR Code.

#### QR Code object
***Attributes***

| Field | Type | Required | Read only | Description |
|---|---|---|---|---|
| `id` | `integer` |  `false` | `true`  | Unique identifier for the object |
| `name` | `string` |  `true` | `false`  | The name of the qr code |
| `fields_data` | `object` |  `true` | `false`  | Fields data for the QR Code |
| `attributes` | `object` |  `true` | `false`  | Attributes data for the QR Code |
| `qr_type` | `integer` |  `false` | `false`  | QR type (`1` Static, `2` Dynamic) |
| `organization` | `integer` |  `true` | `true`  | Organization id |
| `place` | `integer` |  `true` | `false`  | Place id |
| `url` | `string` |  `false` | `true`  | QR code URL |
| `state` | `string` |  `false` | `true`  | State of the QR Code (`A` Active, `S` Sleeping) |
| `place_data` | `list[object]` |  `false` | `true`  | Place data associated with place id. Only available in `List` |
| `tags` | `list[integer]` |  `false` | `false`  | List of tag ids associated |
| `tag_data` | `list[object]` |  `false` | `true`  | Tag data |
| `meta` | `object` |  `false` | `false`  | Metadata associated with the QR Code |
| `created` | `timestamp` |  `false` | `true`  | Created timestamp |
| `updated` | `timestamp` |  `false` | `true`  | Last updated timestamp |
| `heartbeat` | `timestamp` |  `false` | `true`  | Timestamp when the qr code was last detected |
| `campaign` | `Campaign` |  `false` | `false`  | `Campaign` object |

#### List QR Codes
Returns a list of your QR Codes. The tags are returned sorted by `updated`, with the most recently updated qr codes appearing first.

`GET https://api.beaconstac.com/api/2.0/qrcodes/`

Filter arguments:
1. `name`: `exact`, `icontains`
2. `place__name`: `exact`, `icontains`
3. `tags__name`: `exact`, `icontains`
4. `url`: `exact`
5. `campaign__content_type`: `exact` 
6. `state`: `exact`

Search Fields:
1. `name`
2. `place__name`
3. `tags__name`
4. `url`
5. `campaign__content_type`

Ordering fields:
1. `name`
2. `place__name`
3. `created`
4. `updated` - default
5. `campaign__content_type`
6. `state`

#### Retrieve a QR Code
Retrieves the details of an existing qr code. You need only supply the unique qr code identifier that was returned upon qr codes listing.

`GET https://api.beaconstac.com/api/2.0/qrcodes/{qrcode_id}`

#### Update a QR Code
Updates the specified qr code by setting the values of the parameters passed. Any parameters not provided will be left unchanged. However, the request should contain the required fields. Please refer to the QRCode object.

`PUT https://api.beaconstac.com/api/2.0/qrcodes/{qrcode_id}`

Example:
Changing campaign to a Markdown Card
```json
{
    "id": 1189,
    "campaign": {
        "id": 223,
        "custom_url": "https://beaconstac.com",
        "content_type": 2,
        "campaign_active": true,
        "organization": 1284,
        "markdown_card": 434,
        "form": 7099,
        "schedule": null
    },
    "name": "Corner",
    "url": "https://qr.tapnscan.me/f78WrG",
    "place": 1929
}
```

#### Create a QR Code
Creates a new qr code. However, the request should contain the required fields. Please refer to the NFCTag object.

`POST https://api.beaconstac.com/api/2.0/qrcodes/`

Example:
Create a QRCode with campaign set to a Markdown Card
```json
{
    "attributes": {
        "color": "#000000",
        "margin": "",
        "backgroundImage": "",
        "logoImage": "https://static.beaconstac.com/assets/img/qr-code-logos/calender.svg"
    },
    "name": "test 3",
    "qr_type": 2,
    "organization": 1284,
    "place": 23
}
```

#### Download a QR code
Download a QR code in a particular size and image format. Image types supported are PNG, JPEG, SVG and PDF.
If no canvas type is sent in the parameters, the QRCode would be generated in all image types supported.

`GET https://api.beaconstac.com/api/2.0/qrcodes/{qrcode_id}/download/?size={size}&canvas_type={canvas_type}`

Example:
Download the QRCode of size 1024x1024 pixels in SVG format.
`GET https://api.beaconstac.com/api/2.0/qrcodes/100/download/?size=1024&canvas_type=svg`
```json
{
    "svg": "url.svg"
}
```

( For detailed QR Code API documentation please refer: https://www.beaconstac.com/qr-code-generator-api )


----
### Bulk QR Codes
`BulkQRCode` objects allow you to create a collection of multiple QR Codes from a CSV file in a single request. You can list, retreive, create or download a Bulk QR Code collection.

Obtain your AuthorizationToken from the [Beaconstac dashboard](https://dashboard.beaconstac.com/account)

#### Bulk QR Code object
***Attributes***

| Field | Type | Required | Read only | Description |
|---|---|---|---|---|
| `id` | `integer` |  `false` | `true`  | Unique identifier for the object |
| `name` | `string` |  `true` | `false`  | The name of the Bulk QR code collection |
| `media` | `integet` |  `false` | `true`  | Media ID |
| `media_data` | `object` |  `false` | `false`  | Media data associated with media ID |
| `attributes` | `object` |  `true` | `false`  | Attributes/Design data for the QR Codes being created |
| `qr_type` | `integer` |  `false` | `true`  | QR type (`1`) |
| `qr_data_type` | `integer` |  `true` | `false`  | QR data type (`1` Website, `2` SMS, `3` Phone, `4` email, `5` vCard, `6` text) |
| `organization` | `integer` |  `true` | `true`  | Organization ID |
| `storage_url` | `string` |  `false` | `true`  | QR code URL |
| `created` | `timestamp` |  `false` | `true`  | Created timestamp |
| `updated` | `timestamp` |  `false` | `true`  | Last updated timestamp |

#### List Bulk QR Code collections
Returns a list of your QR Codes. The tags are returned sorted by `updated`, with the most recently updated qr codes appearing first.

Example:

Use the below cURL command to list all Bulk QR Code collections in your account:

```json
curl --location --request GET 'https://api.beaconstac.com/api/2.0/bulkqrcodes/' \
--header 'Authorization: Token <AuthorizationToken>'
```

Filter arguments:
1. `name`: `exact`, `icontains`
2. `qr_data_type`: `exact`

Search Fields:
1. `name`

Ordering fields:
1. `name`
2. `created`
3. `updated` - default

#### Retrieve a Bulk QR Code collection
Retrieves the details of an existing Bulk QR Code collection. You need only supply the unique Bulk QR Code identifier that was returned upon listing.

Example:

Use the below cURL command to retreive a single Bulk QR Code collection by id.
```json
curl --location --request GET 'https://api.beaconstac.com/api/2.0/bulkqrcodes/101/' \
--header 'Authorization: Token <AuthorizationToken>'
```

#### CSV file data format
The examples for the CSV files can be viewed from the [Beaconstac dashboard](https://dashboard.beaconstac.com/bulk-qr-codes/add) or from the CSV Files in the links below:
| QR Data type | Example CSV File |
|---|---|
| `1` (Website) | https://admin.beaconstac.com/assets/files/static-website-csv.csv |
| `2` (Phone) | https://dashboard.beaconstac.com/assets/files/static-phone-csv.csv |
| `3` (SMS) | https://dashboard.beaconstac.com/assets/files/static-sms-csv.csv |
| `4` (Email) | https://dashboard.beaconstac.com/assets/files/static-email-csv.csv |
| `5` (vCard) | https://dashboard.beaconstac.com/assets/files/static-vcard-csv.csv |
| `6` (Text) | https://dashboard.beaconstac.com/assets/files/static-text-csv.csv |

#### Create a Bulk QR Code collection
Creates a new Bulk QR Code collection using data from CSV file. However, the request should contain the required fields. 

Example:

Create a Bulk QR Code collection by using the cURL request:

```json
curl --location --request POST 'https://api.beaconstac.com/api/2.0/bulkqrcodes/' \
--header 'Authorization: Token e62435a78e67ec98bba3b879ba00448650032557' \
--header 'Content-Type: multipart/form-data' \
--form 'file=@/Users/hariprasad/Desktop/bulk-qr-sample-csv/static-vcard-csv.csv' \
--form 'name=myvCard-QRCodes' \
--form 'qr_type=1' \
--form 'size=512' \
--form 'error_correction_level=0' \
--form 'qr_data_type=5' \
--form 'attributes={"color":"#ab578f",
"gradientType":"radial",
 "margin": 50,
"dataPattern":"left-diamond",
"eyeBallShape":"left-diamond",
"eyeFrameShape":"left-leaf",
"eyeBallColor":"#ea8745",
"eyeFrameColor":"#ea4587",
"logoImage": "https://static.beaconstac.com/assets/img/qr-code-logos/instagram.svg",
"frameStyle":"banner-top"
}' \
--form 'organization=720'
```

Response
```json
{
    "id": 220,
    "attributes": {
        "color": "#ab578f",
        "gradientType": "radial",
        "margin": 50,
        "dataPattern": "left-diamond",
        "eyeBallShape": "left-diamond",
        "eyeFrameShape": "left-leaf",
        "eyeBallColor": "#ea8745",
        "eyeFrameColor": "#ea4587",
        "logoImage": "https://static.beaconstac.com/assets/img/qr-code-logos/instagram.svg",
        "frameStyle": "banner-top"
    },
    "media_data": {
        "id": 68976,
        "name": "myvCard-QRCodes",
        "url": "https://d1bqobzsowu5wu.cloudfront.net/720/6d69b10e8e794beb920c7a899cf6d044"
    },
    "name": "myvCard-QRCodes",
    "storage_url": "https://d1bqobzsowu5wu.cloudfront.net/720/bulkqr/MjIwLTIwMjAtMDUtMTNUMTQ6MTY6NDAuNDY1ODI2KzAwOjAw.zip",
    "qr_type": 1,
    "qr_data_type": 5,
    "created": "2020-05-13T19:46:40.465826+05:30",
    "updated": "2020-05-13T19:46:50.948362+05:30",
    "organization": 720,
    "media": 68976
}
```

The QR Code designs can be customized by varying the attributes of the Bulk QR Code object. More details can be accessed [here](https://www.beaconstac.com/qr-code-generator-api#docs)

#### Download Bulk QR code collection
The Bulk QR Code collection can be downloaded using the link from the `storage_url` ( Ex. `https://d1bqobzsowu5wu.cloudfront.net/720/bulkqr/MjE5LTIwMjAtMDUtMTNUMTQ6MDA6MDkuODgwMTYzKzAwOjAw.zip` ) field. 

The URL downloads a zipped collection of the images in 4 formats: `png`, `jpeg`, `pdf`, `svg`.


#### Update an existing Bulk QR Code collection
You can update an existing Bulk QR Code collection with different design data using the unique ID field of the Bulk QR Code object. (Note: The csv file attached with a collection cannot be updated)

Example:

Update a Bulk QR Code collection by using the cURL request:

Request:
```json
curl --location --request PUT 'https://api.beaconstac.com/api/2.0/bulkqrcodes/220/' \
--header 'Authorization: Token e62435a78e67ec98bba3b879ba00448650032557' \
--header 'Content-Type: application/json' \
--header 'Content-Type: text/plain' \
--data-raw '{
    "id": 220,
    "attributes": {
        "color": "#2595ff",
        "gradientType": "radial",
        "margin": 50,
        "dataPattern": "left-diamond",
        "eyeBallShape": "left-diamond",
        "eyeFrameShape": "left-leaf",
        "eyeBallColor": "#2595ff",
        "eyeFrameColor": "#95ff25",
        "logoImage": "https://static.beaconstac.com/assets/img/qr-code-logos/instagram.svg",
        "frameStyle": "banner-bottom"
    },
    "media_data": {
        "id": 68976,
        "name": "myvCard-QRCodes",
        "url": "https://d1bqobzsowu5wu.cloudfront.net/720/6d69b10e8e794beb920c7a899cf6d044"
    },
    "name": "myvCard-QRCodes",
    "storage_url": "https://d1bqobzsowu5wu.cloudfront.net/720/bulkqr/MjIwLTIwMjAtMDUtMTNUMTQ6MTY6NDAuNDY1ODI2KzAwOjAw.zip",
    "qr_type": 1,
    "qr_data_type": 5,
    "created": "2020-05-13T19:46:40.465826+05:30",
    "updated": "2020-05-13T19:46:50.948362+05:30",
    "organization": 720,
    "media": 68976
}'
```

Response
```json
{
    "id": 220,
    "attributes": {
        "color": "#2595ff",
        "gradientType": "radial",
        "margin": 50,
        "dataPattern": "left-diamond",
        "eyeBallShape": "left-diamond",
        "eyeFrameShape": "left-leaf",
        "eyeBallColor": "#2595ff",
        "eyeFrameColor": "#95ff25",
        "logoImage": "https://static.beaconstac.com/assets/img/qr-code-logos/instagram.svg",
        "frameStyle": "banner-bottom"
    },
    "media_data": {
        "id": 68977,
        "name": "myvCard-QRCodes",
        "url": "https://d1bqobzsowu5wu.cloudfront.net/720/45b9ef3414d541e88c6cae514aea454e"
    },
    "name": "myvCard-QRCodes",
    "storage_url": "https://d1bqobzsowu5wu.cloudfront.net/720/bulkqr/MjIwLTIwMjAtMDUtMTNUMTQ6MTY6NDAuNDY1ODI2KzAwOjAw.zip",
    "qr_type": 1,
    "qr_data_type": 5,
    "created": "2020-05-13T19:46:40.465826+05:30",
    "updated": "2020-05-13T19:49:44.578320+05:30",
    "organization": 720,
    "media": 68977
}
```

----
### Geofence
`Geofence` objects allow you to perform actions on your geofences. You can retrieve individual geofences as well as a list of all your geofences or update a geofence.

#### Geofence object
***Attributes***

| Field | Type | Required | Read only | Description |
|---|---|---|---|---|
| `id` | `integer` |  `false` | `true`  | Unique identifier for the object |
| `name` | `string` |  `true` | `false`  | The name of the geofence |
| `latitude` | `float` |  `true` | `false`  | Latitude |
| `longitude` | `float` |  `true` | `false`  | Longigtude |
| `radius` | `integer` |  `true` | `false`  | Radius of the geofence |
| `organization` | `integer` |  `true` | `true`  | Organization id |
| `place` | `integer` |  `true` | `false`  | Place id |
| `url` | `string` |  `false` | `true`  | Geofence URL |
| `state` | `string` |  `false` | `true`  | State of the geofence (`A` Active, `S` Sleeping) |
| `place_data` | `list[object]` |  `false` | `true`  | Place data associated with place id. Only available in `List` |
| `tags` | `list[integer]` |  `false` | `false`  | List of tag ids associated |
| `tag_data` | `list[object]` |  `false` | `true`  | Tag data |
| `meta` | `object` |  `false` | `false`  | Metadata associated with the geofence |
| `created` | `timestamp` |  `false` | `true`  | Created timestamp |
| `updated` | `timestamp` |  `false` | `true`  | Last updated timestamp |
| `heartbeat` | `timestamp` |  `false` | `true`  | Timestamp when the geofence was last detected |
| `campaign` | `Campaign` |  `false` | `false`  | `Campaign` object |
| `notifications` | `list[CampaignNotification]` |  `false` | `false`  | `CampaignNotification` object |

#### List Geofences
Returns a list of your Geofences. The Geofences are returned sorted by `updated`, with the most recently updated Geofence appearing first.

`GET https://api.beaconstac.com/api/2.0/geofences/`

Filter arguments:
1. `name`: `exact`, `icontains`
2. `place__name`: `exact`, `icontains`
3. `url`: `exact`
4. `campaign__content_type`: `exact` 
5. `state`: `exact`

Search Fields:
1. `name`
2. `place__name`
3. `url`
4. `campaign__content_type`

Ordering fields:
1. `name`
2. `place__name`
3. `created`
4. `updated` - default
5. `campaign__content_type`
6. `state`

#### Retrieve a Geofence
Retrieves the details of an existing Geofence. You need only supply the unique Geofence identifier that was returned upon Geofence listing.

`GET https://api.beaconstac.com/api/2.0/geofences/{geofence_id}`


#### Update Geofence
Updates the specified Geofence by setting the values of the parameters passed. Any parameters not provided will be left unchanged. However, the request should contain the required fields. Please refer to the Geofence object.

`PUT https://api.beaconstac.com/api/2.0/geofences/{geofence_id}`

Example:
Changing campaign to a Markdown Card
```json
{
    "id": 1189,
    "campaign": {
        "id": 223,
        "custom_url": "https://beaconstac.com",
        "content_type": 2,
        "campaign_active": true,
        "organization": 1284,
        "markdown_card": 434,
        "form": 7099,
        "schedule": null
    },
    "name": "Corner",
    "url": "https://geo.tapnscan.me/f78WrG",
    "place": 1929,
    "latitude": 12.21343,
    "longitude": 12.213132,
    "radius": 100
}
```

#### Create Geofence
Creates a new geofence by setting the values of the parameters passed. The request should contain the required fields. Please refer to the Geofence object.

`POST https://api.beaconstac.com/api/2.0/geofences/`

Example:
Create Geofence with campaign to a Markdown Card
```json
{
    "campaign": {
        "id": 223,
        "custom_url": "https://beaconstac.com",
        "content_type": 2,
        "campaign_active": true,
        "organization": 1284,
        "markdown_card": 434,
        "form": 7099,
        "schedule": null
    },
    "name": "Corner",
    "place": 1929,
    "latitude": 12.21343,
    "longitude": 12.213132,
    "radius": 100,
    "organization": 1234
}
```

----
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

`GET https://api.beaconstac.com/api/2.0/places/`

Filter arguments:
1. `name`: `exact`, `icontains`

Search Fields:
1. `name`

Ordering fields:
1. `name`
2. `created`
3. `updated` - default
4. `address`


#### Retrieve a Place
Retrieves the details of an existing Place. You need only supply the unique Place identifier that was returned upon Place listing.

`GET https://api.beaconstac.com/api/2.0/places/{place_id}`


#### Update Place
Updates the specified Place by setting the values of the parameters passed. Any parameters not provided will be left unchanged. However, the request should contain the required fields. Please refer to the Place object.

`PUT https://api.beaconstac.com/api/2.0/places/{place_id}`

Example:
Update an existing place based on ID of the place
```json
{
	"organization": 1234,
	"name": "Mobstac, New York",
	"address": "450 Lexington Avenue, 4th Floor New York, NY 10017",
	"latitude": "40.73",
	"longitude": "-73.94"
}
```

#### Create Place
Creates a new place in your account by setting the values of the parameters passed. The request should contain the required fields. Please refer to the Place object.

`POST https://api.beaconstac.com/api/2.0/places/`

Example:
Create a new Place
```json
{
    "organization": 1234,
    "name": "Mobstac, Bangalore",
    "address": "MobStac,2nd floor, Mayfair Building, Karnataka, 316, 100 Feet Rd, Binnamangala, Stage 1, Indiranagar, Bengaluru, Karnataka 560038, India",
    "latitude": 12.9818683,
    "longitude": 77.6404921999999
}
```


----
### User
User objects allow you to perform actions on the users in your account. You can list of all your users, retrieve individual users, create a user or update a user.

(Note: Creation of Users is supported only on Reseller plan and above)

#### User object
***Attributes***

| Field | Type | Required | Read only | Description |
|---|---|---|---|---|
| `id` | `integer` |  `false` | `true`  | Unique identifier for the object |
| `username` | `string` |  `true` | `false`  | The username used to login |
| `organization` | `integer` |  `true` | `true`  | Organization id |
| `first_name` | `string` |  `false` | `false`  | First name of the user |
| `last_name` | `string` |  `false` | `false`  | Last name of the user |
| `email` | `string` |  `false` | `false`  | email of the user |
| `customer_plan` | `string` |  `false` | `false`  | Customer Plan the User is subscribed to (`ST` Starter, `LT` Lite, `BA` Basic, `PR` Premium, `RE` Reseller, `WL` Whitelabel, `EN` Enterprise)|
| `subscription_state` | `string` |  `false` | `false`  | Subscription state of the user (`A` Active, `E` Expired, `R` Removed)|
| `user_group` | `string` |  `false` | `false`  | User group the user belongs to (`OW` Owner, `AD` Admin, `RO` Read-Only)|
| `profile_picture` | `string` |  `false` | `false`  | URL to the User's profile picture |
| `billing_email` | `string` |  `false` | `false`  | User email used for billing |
| `firebase_token` | `string` |  `false` | `false`  | User firebase token used to send Notifications |
| `is_active` | `boolean` |  `false` | `true`  | User active status |
| `is_superuser` | `boolean` |  `false` | `true`  | Superuser status |
| `stripe_id` | `string` |  `false` | `false`  | Stripe ID of the user |
| `shopify_id` | `integer` |  `false` | `false`  | Shopify ID for the user |
| `date_joined` | `timestamp` |  `false` | `true`  | Date joined timestamp |
| `created` | `timestamp` |  `false` | `true`  | Created timestamp |
| `updated` | `timestamp` |  `false` | `true`  | Last updated timestamp |

(Note: Stripe ID and Shopify ID will be null for all users with user groups AD and RO)

#### List Users
Returns a list of all Users. The users are returned sorted by date joined, with the most recently joined users appearing first.

`GET https://api.beaconstac.com/api/2.0/users/`

Filter arguments:
1. `first_name`: `exact`, `icontains`
2. `last_name`:  `exact`, `icontains`
3. `username`: `exact`, `icontains`
4. `email`: `exact`, `icontains`
5. `organization id`: `exact`
6. `date_joined`: `lt`, `lte`, `gt`, `gte`
7. `customer_plan`: `exact` 
8. `subscription_state`: `exact`


Search Fields:
1. `username`
1. `email`
1. `user_group`
1. `first_name`
1. `last_name`

Ordering fields:
1. `username`
2. `email`
3. `date_joined` - default
4. `user_group`
5. `first_name`
6. `last_name`

#### Retrieve a User
Retrieves the details of an existing user. You need to supply the unique user identifier that was returned upon listing.

`GET https://api.beaconstac.com/api/2.0/users/{user_id}`



#### Create a new User
Creates a new user. However, the request should contain the required fields. New users can be created only by users having customer

`POST https://api.beaconstac.com/api/2.0/users/`

Example:
Create a new user User in an organization.
```json
{
      "username": "sample_user",
      "customer_plan": "PR",
      "first_name": "Sample",
      "last_name": "User",
      "email": "sample_user@beaconstac.com",
      "billing_email": "billing@beaconstac.com",
      "organization": 1284,
      "profile_picture": "https://d1bqobzsowu5wu.cloudfront.net/720/93f7199471e64ddb9ad4e8bb888bb1ce",
      "password": "testPassword123"
}
```

(Note: User password is set to default password if no password is passed)


#### Update an existing User
Creates a new user. However, the request should contain the required fields. New users can be created only by users having customer

`POST https://api.beaconstac.com/api/2.0/users/{user_id}`

Example:
Create a new user User in an organization.
```json
{
    "first_name": "John",
    "last_name": "Doe",
    "profile_picture": "https://d1bqobzsowu5wu.cloudfront.net/2934/cf50e4abe1724089a7dd8bc60dbe8977",
    "organization": 1284
}
```

----

## Partner API endpoints

[Creating user accounts in the Beaconstac store](https://github.com/Beaconstac/api/blob/master/older/storeUserCreate.md)

----
## Beaconstac Analytics

### Product
Product types:
1. beacon
2. nfc
3. qr
4. geofence

from, to parameters should be in EPOCH milliseconds.

#### Product overview
Get analytics overview for the product type over the time interval.

`POST https://api.beaconstac.com/reporting/2.0/?organization={organization_id}&method=Products.getOverview`

Example:
Get performance for all beacons in organization ID 23
`POST https://api.beaconstac.com/reporting/2.0/?organization=23&method=Products.getOverview`

Request body:
```json
{
    "product_type": "beacon",
    "from": 1559273172,
    "to": 1559283172
}
```

#### Product performance
Get analytics performace for the product.

`POST https://api.beaconstac.com/reporting/2.0/?organization={organization_id}&method=Products.getPerformance`

Example:
Get performance for nfc tag
`POST https://api.beaconstac.com/reporting/2.0/?organization=23&method=Products.getPerformance`

Request body:
```json
{
    "product_type": "nfc",
    "product_id": 12,
    "from": 1559273172,
    "to": 1559283172,
    "interval": "1d",
    "timezone": "UTC"
}
```


#### Product impressions
Get impressions generated by the product. If no `place` is provided data will be returned for all products of the product type specified in the organization.

`POST https://api.beaconstac.com/reporting/2.0/?organization={organization_id}&method=Products.getImpressions`

Example:
Get impression for all qr codes in place id 12
`POST https://api.beaconstac.com/reporting/2.0/?organization=23&method=Products.getImpressions`

Request body:
```json
{
    "product_type": "qr",
    "place": 12,
    "from": 1559273172,
    "to": 1559283172,
    "interval": "1d",
    "timezone": "UTC"
}
```

#### Product impression detail
Get impressions details generated by the product. If no `place` is provided data will be returned for all products of the product type specified in the organization.

`POST https://api.beaconstac.com/reporting/2.0/?organization={organization_id}&method=Products.getImpressionDetail`

Example:
Get impression details for all qr codes in place id 12
`POST https://api.beaconstac.com/reporting/2.0/?organization=23&method=Products.getImpressionDetail`

Request body:
```json
{
    "product_type": "qr",
    "place": 12,
    "from": 1559273172,
    "to": 1559283172
}
```

#### Product impression distribution
Get impression distribution generated by the product. If no `place` is provided data will be returned for all products of the product type specified in the organization.

`POST https://api.beaconstac.com/reporting/2.0/?organization={organization_id}&method=Products.getImpressionDistribution`

Example:
Get impression distribution for all qr codes in place id 12
`POST https://api.beaconstac.com/reporting/2.0/?organization=23&method=Products.getImpressionDistribution`

Request body:
```json
{
    "product_type": "qr",
    "place": 12,
    "from": 1559273172,
    "to": 1559283172
}
```

#### Generate product CSV data
Generate a detailed CSV report for products in the specified organizations. The report will be emailed to the user email mentioned in the POST body. If not email is specified it would be emailed to the user making the request.

`POST https://api.beaconstac.com/reporting/2.0/?organization={organization_id}&method=Products.getImpressionDistribution`

Example:
Generate CS impression distribution for qr codes 23 and 43 in organizations 1 and 2.
`POST https://api.beaconstac.com/reporting/2.0/?method=Csv.getProductData`

Request body:
```json
{
    "product_type": "qr",
    "organization_ids": [1, 2],
    "product_ids": [23, 43],
    "from": 1559273172,
    "to": 1559283172,
    "timezone": "UTC"
}
```


#### Get analytics overview for a given period
Get analytics overview (product count, notification count, impression count, conversion percentage) of your product based on product type being passed. The total number of unique visitors is also given in the response. 

`POST https://api.beaconstac.com/reporting/2.0/?organization={organization_id}&method=Overview.getPeriodOverview`

Example:
Generate analytics overview of all products over a period of time.

`POST https://api.beaconstac.com/reporting/2.0/?organization={organization_id}&method=Overview.getPeriodOverview`

Request body:
```json
{
	"from": "1567535400000",
	"to": "1582482600000",
	"product_type": "geofence",
	"organization": 1234
}
```
