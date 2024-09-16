# Welcome to the Wiser by Feller Cloud API!

This documentation provides you with all the information you need to integrate your applications with the Wiser by Feller Cloud.

This guide is divided into two main sections:
1. Authentication
   Before you can start using the Wiser by Feller Cloud API, you'll need to authenticate your requests. Once you register as a partner, you will receive a unique client_id and client_secret. These credentials are used to obtain an access token, which is required for all API calls.
2. API Documentation
   This section provides detailed information about the available API endpoints, including request methods, parameters, response codes, and example payloads. You'll find comprehensive documentation for each endpoint, along with code samples to help you get started quickly.
   We encourage you to explore the API documentation and discover the possibilities of integrating with the Wiser by Feller Cloud. If you have any questions or require further assistance, please don't hesitate to contact our support team.


## Authentication
A Machine to Machine Application represents a program that interacts with an API where there is no user involved.

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
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Ik9UTTBOa1JHUVRjM01FWXlPVU5GT1RZd1F6RXlSVGd6T0RaQ09EUkVRak5DT1RJME9EQTVOZyJ9.eyJpc3MiOiJodHRwczovL2ZlbGxlci5ldS5hdXRoMC5jb20vIiwic3ViIjoiWk9POEx5SkROdVZiakRveHhDZzJyQ0l2OU5VYTNCeXNAY2xpZW50cyIsImF1ZCI6Imh0dHBzOi8vaWQuZmVsbGVyLmNoL3dpc2VyLXBhcnRuZXJzIiwiaWF0IjoxNzIxMjIwMjkxLCJleHAiOjE3MjEzMDY2OTEsImd0eSI6ImNsaWVudC1jcmVkZW50aWFscyIsImF6cCI6IlpPTzhMeUpETnVWYmpEb3h4Q2cyckNJdjlOVWEzQnlzIn0.lxko2VgQcPSNnaHBzBVSmtnbsgejrfT1RNHoojvdSurL0s3StDlbAUkKYUUar4jFtosaC5_f3t_rJXJHItCRczhVopPvxRwx_s9dTVI30htMqBgft9X41MkOWyqph49J8YO3DXxO68jadoV2XH7dgUdk1ri4-JGdGo7XOpkmPsWwCJNs8uM-9xB95LDU47Gmq7gAGX5cwmTEvrrCyCHV8_0qVRR0FBprWfkPfKfUBuERR-31HQWe_xsB9jO_S4jSFdIccT64MM29uG9zzsKvA0-M15IeEg5cCbmL3bqwlh5YsxfWAkEKxkQic7Y3_oTHDt0x0ePppMzDDIVXSa7lYA",
  "token_type": "Bearer"
}
```

That's it!
Now that the application has an access_token, it is now able to make authorized calls to the API.

Example:
```console
curl --request GET \
  --url http://user.nubes.feller.ch/api/partner/sites \
  --header 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Ik9UTTBOa1JHUVRjM01FWXlPVU5GT1RZd1F6RXlSVGd6T0RaQ09EUkVRak5DT1RJME9EQTVOZyJ9.eyJpc3MiOiJodHRwczovL2ZlbGxlci5ldS5hdXRoMC5jb20vIiwic3ViIjoiWk9POEx5SkROdVZiakRveHhDZzJyQ0l2OU5VYTNCeXNAY2xpZW50cyIsImF1ZCI6Imh0dHBzOi8vaWQuZmVsbGVyLmNoL3dpc2VyLXBhcnRuZXJzIiwiaWF0IjoxNzIxMjIwMjkxLCJleHAiOjE3MjEzMDY2OTEsImd0eSI6ImNsaWVudC1jcmVkZW50aWFscyIsImF6cCI6IlpPTzhMeUpETnVWYmpEb3h4Q2cyckNJdjlOVWEzQnlzIn0.lxko2VgQcPSNnaHBzBVSmtnbsgejrfT1RNHoojvdSurL0s3StDlbAUkKYUUar4jFtosaC5_f3t_rJXJHItCRczhVopPvxRwx_s9dTVI30htMqBgft9X41MkOWyqph49J8YO3DXxO68jadoV2XH7dgUdk1ri4-JGdGo7XOpkmPsWwCJNs8uM-9xB95LDU47Gmq7gAGX5cwmTEvrrCyCHV8_0qVRR0FBprWfkPfKfUBuERR-31HQWe_xsB9jO_S4jSFdIccT64MM29uG9zzsKvA0-M15IeEg5cCbmL3bqwlh5YsxfWAkEKxkQic7Y3_oTHDt0x0ePppMzDDIVXSa7lYA'
```


## API Documentation

* [Site](#site)
    * [Get all accessible site details](#get-all-accessible-site-details)
    * [Get site details](#get-site-details)
* [HVAC](#hvac)
    * [Boost using default boost temperature for site](#boost-using-default-boost-temperature-for-site)
    * [Set boost temperature by hvacgroup](#set-boost-temperature-by-hvac-group)
    * [Set target temperature by hvac group](#set-target-temperature-by-hvac-group)
    * [Set desired valve position by hvac group](#set-desired-valve-position-by-hvac-group)

## Site

### Get all accessible site details

* **Path:** `http://user.nubes.feller.ch/api/partner/sites`
* **Method:** `GET`
* **Summary:** Get all accessible site details
* **Description:** Get all accessible site details
* **Responses:**
    * **200:** 200
* **Security:**
    * bearerAuthJWT

```json
{
  "sites": [
    {
      "id": "bdeb2e4c-9531-4865- acb9- d912b30c277b",
      "online": true
    }
  ]
}
```

### Get site details

* **Path:** `http://user.nubes.feller.ch/api/partner/sites/{siteId}`
* **Method:** `GET`
* **Summary:** Get site details
* **Description:** Get site details* **Parameters:**
    * **siteId** `(path, string, required)` - The id of the site.
* **Responses:**
    * **200:** 200
    * **400:** 400
    * **403:** 403
* **Security:**
    * bearerAuthJWT

```json
{
  "id": "bdeb2e4c-9531-4865- acb9- d912b30c277b",
  "online": true
}
```

## HVAC

### Boost using default boost temperature for site

* **Path:** `http://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/boost`
* **Method:** `PUT`
* **Summary:** Boost using default boost temperature for site
* **Description:** Boost using default boost temperature for site
* **Parameters:**
    * **siteId** `(path, string, required)`
* **Request Body:**
  ```json
  {
    "enabled": true
  }
  ```
* * **Responses:**
    * **200:** 200
    * **504:** 504
* **Security:**
    * bearerAuthJWT

### Set boost temperature by hvac group

* **Path:** `http://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/{hvacGroupId}/boost-temperature`
* **Method:** `PUT`
* **Summary:** Set boost temperature by hvac group
* **Description:** Set boost temperature by hvac group
* **Parameters:**
    * **siteId** `(path, string, required)` - The idof the site.
    * **hvacGroupId** `(path, string, required)` - The id of the hvac group.
* **Request Body:**
  ```json
  {
    "value": 2.5
  }
  ```
* **Responses:**
    * **200:** 200
* **Security:**
    * bearerAuthJWT

### Set target temperature by hvac group

* **Path:** `http://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/{hvacGroupId}/target-temperature`
* **Method:** `PUT`
* **Summary:** Set target temperature by hvac group
* **Description:** Set target temperature by hvac group
* **Parameters:**
    * **siteId** `(path, string, required)` - The id of the site.
    * **hvacGroupId** `(path, string, required)` - The id of the hvac group.
* **Request Body:**
  ```json
  {
    "value": 22.9
  }
  ```
* **Responses:**
    * **200:** 200
* **Security:**
    * bearerAuthJWT

### Set desired valve position by hvac group

* **Path:** `http://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/{hvacGroupId}/valve-position`
* **Method:** `PUT`
* **Summary:** Set desired valve position by hvac group
* **Description:** Set desired valve position by hvac group
* **Parameters:**
    * **siteId** `(path, string, required)` - The id of the site.
    * **hvacGroupId** `(path, string, required)` - The id of the hvac group.
* **Request Body:**
  ```json
  {
    "value": 10001
  }
  ```
* **Responses:**
    * **200:** 200
    * **400:** 400
* **Security:**
    * bearerAuthJWT
