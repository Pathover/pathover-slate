---
title: Pathover API Reference

language_tabs:
  - shell: curl
  - javascript: Node.js

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

## About Pathover API

Welcome to the Pathover API. You can use our API to access Pathover API endpoints, which can be used to submit orders and also create, read, update and delete the data related to your store.

We have language bindings in cURL and Node.js. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

## Requirement

We follow RESTful principles and make use of standard HTTP methods like GET and POST. To get access to Pathover API, please make sure you satisfy the following requirements:

* You must access the API over **HTTPS** protocol

* POST data should be encoded as standard **application/x-www-form-urlencoded**

* Make sure to specify API version in the endpoints, the current version is `api/v1`

# Authentication

Pathover uses API keys to allow access to the API. You can get your API key at your [Company Details](https://app.pathover.com/store/company) page.

Pathover expects for the API key to be included in all API requests to the server in request body specified as **apikey**, like the following:

`apikey: YOUR_API_KEY`

<aside class="notice">
You must replace <code>YOUR_API_KEY</code> with your API key.
</aside>

# Locations

Location is an object that represents one of your physical store locations, where you process orders for your customers.

### Query Parameters

Attribute | Type | Description
--------- | ------- | -----------
actual_rush_waiting_time | integer |(**Required**) Limit result set to whether locations is active.
limit | integer | (**Optional**) Maximum number of items to be returned in result set. Default is 10.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get All Locations

```shell
curl https://app.pathover.com/api/v1/locations/query \
  -d "apikey=YOUR_API_KEY"
```

```javascript
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

> The above command returns JSON response like this:

```json
{
    "total": 1,
    "limit": 10,
    "locations": [{
        "actual_rush_waiting_time": 60,
        "actual_pickup_waiting_time": 60,
        "notification_list": [],
        "default_courier_id": 3,
        "tax": 0,
        "updated_at": "2016/11/22 00:23:46",
        "total_products": 2,
        "location_users": [{
            "active": true,
            "email": "store1-manager@pathover.com",
            "name": "Store1 Manager",
            "id": 3
        }, {
            "active": true,
            "email": "store1-staff@pathover.com",
            "name": "Store1 Staff",
            "id": 4
        }],
        "timezone": {
            "utc_offset": -480,
            "name": "America/Los_Angeles",
            "created_at": "2016/11/18 00:18:12",
            "updated_at": "2016/11/18 21:30:01",
            "identifier": "PST",
            "id": 147
        },
        "id": 1,
        "store_location_constraints": {
            "maintains_inventory": false,
            "store_id": 2,
            "created_at": "2016/11/18 00:20:47",
            "batch_times": [{
                "days": [
                    "monday",
                    "tuesday",
                    "wednesday",
                    "thursday",
                    "friday",
                    "saturday",
                    "sunday"
                ],
                "time": "19:00:00"
            }, {
                "days": [
                    "tuesday",
                    "wednesday",
                    "thursday",
                    "friday",
                    "saturday",
                    "sunday",
                    "monday"
                ],
                "time": "01:30:00"
            }, {
                "days": [
                    "tuesday",
                    "wednesday",
                    "thursday",
                    "friday",
                    "saturday",
                    "sunday",
                    "monday"
                ],
                "time": "02:00:00"
            }],
            "updated_at": "2016/11/22 01:36:08",
            "store_hours": null,
            "stock_warning_threshold": null,
            "capacity": null,
            "location_id": 1,
            "id": 1
        },
        "default_pickup_waiting_time": 60,
        "company_id": 2,
        "geocode": {
            "lat": 37.3246063,
            "lng": -122.0338235
        },
        "default_rush_waiting_time": 60,
        "default_courier": {
            "card_last_four_digits": null,
            "name": "Door Express",
            "carrier_info": null,
            "company_type": "courier",
            "created_at": "2016/11/18 00:18:12",
            "lead_time_mins": 60,
            "updated_at": "2016/11/18 23:41:20",
            "dev_api_key": null,
            "rate": 0,
            "billing_cycle": "monthly",
            "id": 3,
            "prod_api_key": null,
            "active": true,
            "billing_minimum": 0,
            "stripe_customer_id": null,
            "default_location_id": null,
            "last_billing_date": "2016/11/18 23:41:20"
        },
        "pickup_option_name": null,
        "phone": "+12133991633",
        "address": {
            "city": "CUPERTINO",
            "address1": "10122 BANDLEY DR",
            "address2": "",
            "state": "CA",
            "postal_code": "95014-2181",
            "country_code": "US"
        },
        "active": true,
        "name": "Marina Food - Cupertino",
        "default_driver_id": null,
        "timezone_id": 147,
        "created_at": "2016/11/18 00:18:13",
        "do_not_disturb": false,
        "lead_time_mins": 30,
        "email": "chunhao@pathover.com",
        "default_location": true
    }],
    "success": true,
    "page": 0
}
```

This endpoint retrieves all store locations.

### HTTP Request

`GET https://app.pathover.com/api/v1/`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
active | boolean |(**Optional**) Limit result set to whether locations is active.
limit | integer | (**Optional**) Maximum number of items to be returned in result set. Default is 10.

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
