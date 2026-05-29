# Wiser by Feller Cloud API for Partners

The Wiser Cloud API is a dedicated service for Feller business partners aiming to develop innovative applications for the [Wiser by Feller](https://www.feller.ch/de/connected-buildings/wiser-by-feller) connected home platform. Currently, API access is available for heating controls, with plans to expand functionality in the future. As a partner, you have the opportunity to co-innovate with Wiser by Feller, shaping the future of smart living by creating new use cases and contribute to greener homes. End users retain full control over which third-party apps they authorize, with the ability to manage access settings at any time. To begin the process, please contact us with details about your project by emailing partner.api@feller.ch using the subject line: “Partner Access Request”. We will review your request and guide you through the next steps. You’re also welcome to share any feedback or proposals regarding the API by sending an email to this address. Please note that this service is exclusively available to Feller business partners and is not open to private individuals [Terms of Use](https://mam.feller.ch/public/M-1763643966).
<br><br>

## Overview
<p align="center">
<img src="./images/overview.png">
</p>

## Get started

This documentation contains everything you need to integrate your application with the Wiser by Feller Cloud.

This guide is divided into two main sections:
1. [Authentication](#authentication).
   Before you can use the Wiser by Feller Cloud API, you must authenticate your requests. Once your application is approved, you will receive a unique client_id and client_secret. These credentials are required to obtain an access token, which is mandatory for all API calls.
   <br><br>
3. [API Documentation](#-api-documentation).
   This section offers in-depth details on all available API endpoints, including request methods, parameters, response codes, and sample payloads. We encourage you to explore the API documentation and discover the full potential of integrating with the Wiser by Feller Cloud. If you have any questions or need further assistance, please contact our team: partner.api@feller.ch


# Authentication
A Machine to Machine Application represents a program that interacts with an API where there is no user involved.

⚠️ **Please note that the provided access_token is short-lived and will expire 2 hours after its creation. You will need to generate a new token after it expires.**

![image info](./images/api_flow.png)

### Getting an access token for your API
You can execute a client credentials exchange to get an access token for Wiser By Feller cloud. Example in curl.
```console
curl --request POST \
  --url https://id.feller.ch/oauth/token \
  --header 'content-type: application/json' \
  --data '{"client_id":"__________________________","client_secret":"__________________________","audience":"https://id.feller.ch/wiser-partners","grant_type":"client_credentials"}'
```

Response:
```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI...",
  "token_type": "Bearer"
}
```

That's it!
Now that the application has an access_token, it is now able to make authorized calls to the API.

Example:
```console
curl --request GET \
  --url https://user.nubes.feller.ch/api/partner/sites \
  --header 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI...'
```

***
---

# 📄 API Documentation

* [Site](#Site)
    * [Get all accessible sites](#-get-all-accessible-sites)
    * [Get site details](#-get-site-details)
* [Heating Controls](#%EF%B8%8F-Heating-Controls)
    * [Set the default boost temperature for a site](#-set-the-default-boost-temperature-for-the-entire-site)
    * [Set boost temperature for a hvac group](#-set-boost-temperature-for-a-hvac-group)
    * [Set target temperature for a hvac group](#-set-target-temperature-for-a-hvac-group)
    * [Set desired valve position for a hvac group](#-set-the-desired-valve-position-for-a-hvac-group)
* [Application examples](#application-examples)

## Site
A “site” refers to a single Wiser installation, such as a single family home or an apartment. When a user grants your application access through the Wiser Home App, that site becomes accessible. Each site can contain zero, one, or many “hvac groups,” which represent heating zones like a living room. These "hvac groups" are set up by the electrician during comissioning phase using the eSetup app.

## 📌 Get all accessible sites

Retrieve all accessible sites. Returns the site ID, its online status, and location.

* **Path:** `https://user.nubes.feller.ch/api/partner/sites`
* **Method:** `GET`

### Response:
```json
{
  "sites": [
    {
      "id": "bdeb2e4c-9531-4865- acb9- d912b30c277b",
      "online": true
      "gpl":"47.24497900 8.18521610"
    }
  ]
}
```
| Field               | Type     | Description |
|--------------------|----------|-------------|
| `id`               | `string`    | Unique identifier of the site. |
| `online`           | `boolean`| Indicates whether the site is online or not |
| `gpl`             | `string`  | Site location |
---

## 📌 Get site details

This endpoint provides all available information about the site.

* **Path:** `https://user.nubes.feller.ch/api/partner/sites/{siteId}`
* **Method:** `GET`
  
### Parameters

| Name          | Location | Type     | Required | Description                     |
|---------------|----------|----------|----------|---------------------------------|
| `siteId`      | Path     | `string` | Yes      | Unique identifier of the site. |


### Response

```json
{
        "id": "4c788521-9001-4bdd-9287-0e9b544c411e",
        "online": true,
        "gpl":"47.24497900 8.18521610"
        "hvac": [
            {
                "id": 1,
                "ambientTemperature": 21.5,
                "targetTemperature": 25.5,
                "boost_temperature": 2,     
                "unit": "C",
                "thirdPartyEnabled": true
            },
            {
                "id": 2,
                "ambientTemperature": 21.5,
                "targetTemperature": 25.5,
                "boost_temperature": 2,     
                "unit": "C",
                "thirdPartyEnabled": true
             }
        ]
}
```


| Field               | Type     | Description |
|--------------------|----------|-------------|
| `id`               | `int`    | Unique identifier of the hvac group. |
| `ambientTemperature` | `float`  | Current temperature measured in the heating zone. |
| `targetTemperature`  | `float`  | Desired temperature (set point) for the heating zone. |
| `boost_temperature`  | `int`    | Additional temperature increase applied above the set point. |
| `unit`               | `string` | Temperature unit used. Value: `"C"` for Celsius. |
| `thirdPartyEnabled`  | `boolean`| Indicates whether cloud control of this heating group has been disabled by the end user. |

---

# 🌡️ Heating Controls

This set of endpoints allows external systems to control heating behavior within a Wiser installation ("site"). Each hvac group can be managed independently, including setting temperatures, valve positions, and boost levels. To maintain control over an hvac group, your system must call the relevant endpoint **at least once every 15 minutes**. If no update is received within this interval, the Wiser system will automatically resume control of the heating group to ensure consistent operation. 

⚠️ The number of API requests is limited to 5 per minute per site.

---

## 📌 Set the default boost temperature for the entire site.

This endpoint allows boosting all hvac groups by +2 °C relative to the user defined set points. Example. If the set points are 21 °C (as configured in the Home App) across all hvac groups, this endpoint will update the temperatures to 23 °C.

* **Path:** `https://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/boost`
* **Method:** `PUT`

### Parameters

| Name          | Location | Type     | Required | Description                     |
|---------------|----------|----------|----------|---------------------------------|
| `siteId`      | Path     | `string` | Yes      | Unique identifier of the site. |

### Request Body

  ```json
  {
    "enabled": true
  }
  ```
| Field               | Type     | Description |
|--------------------|----------|-------------|
| `enabled`               | `boolean`    | Activate / De-activate boosting |

---

## 📌 Set boost temperature for a hvac group

This endpoint allows boosting individual hvac groups. The boost range is limited to –5 °C to +5 °C.

* **Path:** `https://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/{hvacGroupId}/boost-temperature`
* **Method:** `PUT`

### Parameters

| Name          | Location | Type     | Required | Description                     |
|---------------|----------|----------|----------|---------------------------------|
| `siteId`      | Path     | `string` | Yes      | Unique identifier of the site. |
| `hvacGroupId` | Path     | `string` | Yes      | Unique identifier of the hvac group. |

### Request Body

  ```json
  {
    "value": 1
  }
  ```
| Field               | Type     | Description |
|--------------------|----------|-------------|
| `value`               | `int`    | Desired boost temperature |

---

## 📌 Set target temperature for a hvac group

Set the desired temperature for a hvac group (room temperature setpoint) between +5 °C ... +30 °C. This endpoint also overrides any user-defined minimum or maximum temperature limits that may have been previously configured in the Home App.

* **Path:** `https://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/{hvacGroupId}/target-temperature`
* **Method:** `PUT`

### Parameters

| Name          | Location | Type     | Required | Description                     |
|---------------|----------|----------|----------|---------------------------------|
| `siteId`      | Path     | `string` | Yes      | Unique identifier of the site. |
| `hvacGroupId` | Path     | `string` | Yes      | Unique identifier of the hvac group. |
  

### Request Body

  ```json
  {
    "value": 22.9
  }
  ```
| Field               | Type     | Description |
|--------------------|----------|-------------|
| `value`               | `float`    | Desired room temperature |

---

## 📌 Set the desired valve position for a hvac group

This endpoint allows setting the desired valve position for a given HVAC group. The position is expressed as an integer between `0` and `10000`, representing approximation of the valve opening: 0 = closed, 5000 = 50 %, 10000 fully open).

* **Path:** `https://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/{hvacGroupId}/valve-position`
* **Method:** `PUT`

### Parameters

| Name          | Location | Type     | Required | Description                     |
|---------------|----------|----------|----------|---------------------------------|
| `siteId`      | Path     | `string` | Yes      | Unique identifier of the site. |
| `hvacGroupId` | Path     | `string` | Yes      | Unique identifier of the hvac group. |


### Request Body

```json
{
  "value": 10000
}
```
| Field               | Type     | Description |
|--------------------|----------|-------------|
| `value`               | `int`    | Desired valve position. Range: 0 (closed) to 10000 (fully open). |
---
## Application examples
### Energy Optimizer
Turn your building into an energy storage with [Solar Manager](https://wwww.solarmanager.ch) and Wiser by Feller. Use the thermal mass of your building to store energy. This transforms the building structure into a sustainable, cost-saving thermal battery that maximizes your self-consumption rate. More information in: [DE](https://mam.feller.ch/public/M-1770385409), [FR](https://mam.feller.ch/public/M-1770385505) or [IT](https://mam.feller.ch/public/M-1770385570).
