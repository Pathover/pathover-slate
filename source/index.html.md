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

* Base URL of Pathover API is `app.pathover.com/api`

* Make sure to specify API version in the endpoints, the current version is `/v1`

## About Timestamp

> An example of a timestamp format:

```
2016/11/18 00:18:12
```

Times are formatted only in **UTC time**, please remember to convert your local time to UTC before sending request.

# Authentication

Pathover uses API keys to allow access to the API. You can get your API key at your [Company Details](https://app.pathover.com/store/company) page.

Pathover expects for the API key to be included in all API requests to the server in request body specified as **apikey**, like the following:

`apikey: YOUR_API_KEY`

<aside class="notice">
You must replace <code>YOUR_API_KEY</code> with your API key.
</aside>

# Locations

Location is an object that represents one of your physical store locations, where you process orders for your customers.

## Location properties

Attribute | Type | Description
--------- | ------- | -----------
actual_rush_waiting_time | integer | The wait time in minute for an on-demand delivery to be ready for pickup, this value will be reset to `default_rush_waiting_time` every midnight at local time.
actual_pickup_waiting_time | integer | The wait time in minute for a pickup order to be ready for pickup, this value will be reset to `default_pickup_waiting_time` every midnight at local time.
default_rush_waiting_time | integer | The default wait time in minute for an on-demand delivery to be ready for pickup, used to reset `actual_rush_waiting_time`.
default_pickup_waiting_time | integer | The default wait time in minute for a pickup order to be ready for pickup, used to reset `actual_pickup_waiting_time`.
notification_list | array | An extra list of phone numbers and emails to get notification when new order comes in or batch time happens, this list will be cleared every midnight at local time. See [Notification List properties](#notification-list-properties).
default_courier_id | integer | The id of default courier, if specified.
default_courier | courier | **TODO:** are they going to see this?
tax | float | The tax rate of store location.
created_at | datetime | The timestamp when store location was created.
updated_at | datetime | The timestamp when store location was last modified.
total_products | integer | Total number of products this location possesses.
location_users | array | A list of users associated with this store location. See [Location User properties](#location-user-properties).
timezone_id | integer | The id of the [timezone object](#timezones)
timezone | [timezone object](#timezones) | The timezone information based on the address of the store location.
id | integer | The unique id number of the location.
store_location_constraints | [location constraints](#location-constraints) | The general settings of the store location.
company_id | integer | The unique id number of the store.
geocode | [geocode object](#geocode) | The geocode of the locations.
pickup_option_name | string | The pickup option name used for Shopify or WooCommerce integration.
phone | string | The phone number to reach the store location.
address | [address object](#address) | The address of the store location.
active | boolean | Whether store location is active.
name | string | The name of store location.
default_driver_id | integer | The id of default driver, if specified.
do_not_disturb | boolean | Whether the store location would like to stop receiving order temporarily. **TODO:** are we showing this?
lead_time_mins | integer | The lead time in minutes until couriers come to store.
email | string | The email to reach the store location.
default_location | boolean | Whether the store location is the flag ship store.

### Notification List properties

Attribute | Type | Description
--------- | ------- | -----------
phone | string | The recipient's phone to get notification.
name | string | The recipient's name to get notification.
email | string | The recipient's email to get notification.

### Location User properties

Attribute | Type | Description
--------- | ------- | -----------
id | integer | The unique id number of the user.
name | string | The name of user.
email | string | The email of user.
active | boolean | Whether the user is active.

## Create a Locations

```shell
curl -X POST https://localhost:8888/api/v1/locations/create \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d '{
      "apikey": "YOUR_API_KEY",
      "name": "Pathover, Inc.",
      "active": true,
      "default_location": false,
      "phone": "3012840610",
      "email": "support@pathover.com",
      "default_pickup_waiting_time": 60,
      "default_rush_waiting_time": 60,
      "address": {
          "city": "Sunnyvale",
          "address1": "440 North Wolfe Road",
          "address2": "",
          "state": "CA",
          "postal_code": "94085-3869",
          "country_code": "US"
      }
    }'
```

```javascript
var request = require('request');

// allow SSL to work
process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";

var data = {
  form: {
    apikey: 'YOUR_API_KEY',
    name: 'Pathover, Inc.',
    active: true,
    default_location: false,
    phone: '3012840610',
    email: 'support@pathover.com',
    default_pickup_waiting_time: 60,
    default_rush_waiting_time: 60,
    address: JSON.stringify({
        city: 'Sunnyvale',
        address1: '440 North Wolfe Road',
        address2: '',
        state: 'CA',
        postal_code: '94085-3869',
        country_code: 'US'
    })
  }
};

request.post('https://app.pathover.com/api/v1/locations/create', data, function(err, res, body) {
  console.log(body);
});

```

> The above command returns JSON response like this:

```json
{
    "message": "New location added.",
    "location": {
        "phone": "+13012840610",
        "address": {
            "city": "SUNNYVALE",
            "address1": "440 N WOLFE RD",
            "address2": "",
            "state": "CA",
            "postal_code": "94085-3869",
            "country_code": "US"
        },
        "active": true,
        "id": 6,
        "name": "Pathover, Inc.",
        "timezone_id": 147,
        "default_pickup_waiting_time": "60",
        "company_id": 7,
        "email": "support@pathover.com",
        "geocode": {
            "lat": 37.3839198,
            "lng": -122.0128444
        },
        "default_rush_waiting_time": "60"
    },
    "success": true
}
```

This endpoint helps you to create a store locations.

To create a new store location, you'll make a POST with the following arguments:

### HTTP Request

`POST https://app.pathover.com/api/v1/locations/create`

### Query Arguments

Arguments | Type | Description
--------- | ------- | -----------
apikey | string | (**Required**) Your api key.
name | string | (**Required**) The name of store location
address | [address object](#address) | (**Required**) The address of the store location.
active | boolean | (**Required**) Whether store location is active. Default to `true`.
default_location | boolean | (**Required**) Whether the store location is the flag ship store.
phone | string | (**Required**) The phone number to reach the store location.
email | string | (**Required**) The email to reach the store location.
default_pickup_waiting_time | integer | (**Required**) The default wait time in minute for a pickup order to be ready for pickup. Default to `60`.
default_rush_waiting_time | integer | (**Required**) The default wait time in minute for an on-demand delivery to be ready for pickup. Default to `60`.

## Retrieve a Location

```shell
curl https://app.pathover.com/api/v1/locations/read \
  -d '{
      "apikey": "YOUR_API_KEY",
      "id": 6
    }'
```

```javascript
var request = require('request');

// allow SSL to work
process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";

var data = {
  form: {
    apikey: 'YOUR_API_KEY',
    id: 6
  }
};

request.get('https://app.pathover.com/api/v1/locations/read', data, function(err, res, body) {
  console.log(body);
});

```

> The above command returns JSON response like this:

```json
{
    "location": {
        "actual_rush_waiting_time": 60,
        "actual_pickup_waiting_time": 60,
        "notification_list": null,
        "default_courier_id": null,
        "tax": 0,
        "updated_at": "2016/11/30 02:56:41",
        "total_products": 0,
        "location_users": [],
        "id": 6,
        "store_location_constraints": null,
        "default_pickup_waiting_time": 60,
        "company_id": 7,
        "geocode": {
            "lat": 37.3839198,
            "lng": -122.0128444
        },
        "default_rush_waiting_time": 60,
        "pickup_option_name": null,
        "phone": "+13012840610",
        "address": {
            "city": "SUNNYVALE",
            "address1": "440 N WOLFE RD",
            "address2": "",
            "state": "CA",
            "postal_code": "94085-3869",
            "country_code": "US"
        },
        "active": true,
        "name": "Pathover, Inc.",
        "default_driver_id": null,
        "timezone_id": 147,
        "created_at": "2016/11/30 02:56:41",
        "do_not_disturb": false,
        "email": "support@pathover.com",
        "default_location": false
    },
    "success": true
}
```

This endpoint lets you to retrieve a specific store location by `id`.

### HTTP Request

`GET https://app.pathover.com/api/v1/locations/read`

### Query Arguments

Arguments | Type | Description
--------- | ------- | -----------
apikey | string | (**Required**) Your api key.
id | integer | (**Required**) The unique id number of the store location.

## Get All Locations

```shell
curl https://app.pathover.com/api/v1/locations/query \
  -d "apikey=YOUR_API_KEY"
```

```javascript
var request = require('request');

// allow SSL to work
process.env.NODE_TLS_REJECT_UNAUTHORIZED = "0";

var data = {
  form: {
    apikey: 'YOUR_API_KEY'
  }
};

request.get('https://app.pathover.com/api/v1/locations/query', data, function(err, res, body) {
  console.log(body);
});

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
        "phone": "+13012840610",
        "address": {
            "city": "Sunnyvale",
            "address1": "440 North Wolfe Road",
            "address2": "",
            "state": "CA",
            "postal_code": "94085-3869",
            "country_code": "US"
        },
        "active": true,
        "name": "Pathover, Inc.",
        "default_driver_id": null,
        "timezone_id": 147,
        "created_at": "2016/11/18 00:18:13",
        "do_not_disturb": false,
        "lead_time_mins": 30,
        "email": "support@pathover.com",
        "default_location": true
    }],
    "success": true,
    "page": 0
}
```

This endpoint retrieves all store locations.

### HTTP Request

`GET https://app.pathover.com/api/v1/locations/query`

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
apikey | string | (**Required**) Your api key.
active | boolean |(**Optional**) Limit result set to whether locations is active.
limit | integer | (**Optional**) Maximum number of items to be returned in result set. Default is 10.
page | integer | (**Optional**) Current page of the result set.
sort | string | (**Optional**) Sort result set by location attribute. Default to `id`. Options: `id`, `name`, `address`, `active`, `created_at` and `updated_at`.

# Timezones

The timezone object contains the informations regarding a specific time zone.

## Timezone properties

> An example of a timezone object:

```json
{
    "utc_offset": -480,
    "name": "America/Los_Angeles",
    "created_at": "2016/11/18 00:18:12",
    "updated_at": "2016/11/18 21:30:01",
    "identifier": "PST",
    "id": 147
}
```

Attribute | Type | Description
--------- | ------- | -----------
utc_offset | integer | The time difference between UTC time, measured in minutes.
name | string | The name of timezone, as defined in [tz database](https://en.wikipedia.org/wiki/Tz_database).
created_at | datetime | The timestamp when timezone was created.
updated_at | datetime | The timestamp when timezone was last modified.
identifier | string | The abbreviation of timezone.
id | integer | The unique id number of the timezone.

# Geocode

The geocode object contains latitude and longitude.

## Geocode properties

> An example of a geocode object:

```json
{
    "lat": 37.3246063,
    "lng": -122.0338235
}
```

Attribute | Type | Description
--------- | ------- | -----------
lat | float | The latitude of the given location.
lng | float | The longitude of the given location.

# Address

The address object contains detailed address of a location.

## Address properties

> An example of a address object:

```json
{
    "city": "CUPERTINO",
    "address1": "10122 BANDLEY DR",
    "address2": "",
    "state": "CA",
    "postal_code": "95014-2181",
    "country_code": "US"
}
```

Attribute | Type | Description
--------- | ------- | -----------
city | string | The city of the location.
address1 | string | The address line one of the location.
address2 | string | The address line two of the location.
state | string | The state of the location in two letter abbreviation.
postal_code | string | The postal code of the location.
country_code | string | The country code of the location, in [ISO Alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) format.

# Location Constraints

The location constraints object contains the general informations regarding a store location.

## Location Constraints properties

> An example of a location constraints object:

```json
{
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
}
```

Attribute | Type | Description
--------- | ------- | -----------
maintains_inventory | boolean | Whether or not the store maintains inventory
store_id | integer | The unique id number of the store.
created_at | datetime | The timestamp when location constraints was created.
updated_at | datetime | The timestamp when location constraints was last modified.
batch_times | array | A list of batch time for a store location. See [Batch Time properties](#batch-time-properties).
store_hours | array | A list of store location hours. See [Store Hours properties](#store-hours-properties).
stock_warning_threshold | integer | The number that when remaining stock reaches this threshold, users associated with the location will be notified.
capacity | object | **TODO**
location_id | integer | The unique id number of the location.
id | integer | The unique id number of the location constraints.

### Batch Time properties

Attribute | Type | Description
--------- | ------- | -----------
days | string array | A list of days of the week which the store location would like to batch orders. Available values are **monday**, **tuesday**, **wednesday**, **thursday**, **friday**, **saturday** and **sunday**.
time | string | The **UTC time** string of the batch time, in 24-hour clock.

### Store Hours properties

**TODO**
