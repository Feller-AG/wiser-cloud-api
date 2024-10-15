# Wiser by Feller cloud API (in Work)

The Wiser Cloud API is a service designed for partners who want to develop new applications for Wiser by Feller. Currently, we are providing access only for heating controls. Wiser by Feller end users have full control over which third-party apps they grant access to, and they can modify these settings at any time.
As our onboarding process is currently in progress, we kindly ask you to contact us and provide details about your project by emailing customercare.feller@feller.ch with the subject line: "Partner Access Request." We will validate your request and provide the necessary information about the next steps. Please note that this service is exclusively available to Feller business partners and not to private individuals.

## Get started

This documentation provides you with all the information you need to integrate your applications with the Wiser by Feller Cloud.

This guide is divided into two main sections:
1. Authentication
   Before you can start using the Wiser by Feller Cloud API, you'll need to authenticate your requests. As soon as your appliction is accepted, you will receive a unique client_id and client_secret. These credentials are used to obtain an access token, which is required for all API calls.
2. API Documentation
   This section provides detailed information about the available API endpoints, including request methods, parameters, response codes, and example payloads. You'll find comprehensive documentation for each endpoint, along with code samples to help you get started quickly.
   We encourage you to explore the API documentation and discover the possibilities of integrating with the Wiser by Feller Cloud. If you have any questions or require further assistance, please don't hesitate to contact our support team.


### Authentication
A Machine to Machine Application represents a program that interacts with an API where there is no user involved.

**Please note that the provided access_token is short-lived and will expire 2 hours after its creation. You will need to generate a new token after it expires.**

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
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Ik9UTTBOa1JHUVRjM01FWXlPVU5GT1RZd1F6RXlSVGd6T0RaQ09EUkVRak5DT1RJME9EQTVOZyJ9.eyJpc3MiOiJodHRwczovL2ZlbGxlci5ldS5hdXRoMC5jb20vIiwic3ViIjoiWk9POEx5SkROdVZiakRveHhDZzJyQ0l2OU5VYTNCeXNAY2xpZW50cyIsImF1ZCI6Imh0dHBzOi8vaWQuZmVsbGVyLmNoL3dpc2VyLXBhcnRuZXJzIiwiaWF0IjoxNzIxMjIwMjkxLCJleHAiOjE3MjEzMDY2OTEsImd0eSI6ImNsaWVudC1jcmVkZW50aWFscyIsImF6cCI6IlpPTzhMeUpETnVWYmpEb3h4Q2cyckNJdjlOVWEzQnlzIn0.lxko2VgQcPSNnaHBzBVSmtnbsgejrfT1RNHoojvdSurL0s3StDlbAUkKYUUar4jFtosaC5_f3t_rJXJHItCRczhVopPvxRwx_s9dTVI30htMqBgft9X41MkOWyqph49J8YO3DXxO68jadoV2XH7dgUdk1ri4-JGdGo7XOpkmPsWwCJNs8uM-9xB95LDU47Gmq7gAGX5cwmTEvrrCyCHV8_0qVRR0FBprWfkPfKfUBuERR-31HQWe_xsB9jO_S4jSFdIccT64MM29uG9zzsKvA0-M15IeEg5cCbmL3bqwlh5YsxfWAkEKxkQic7Y3_oTHDt0x0ePppMzDDIVXSa7lYA",
  "token_type": "Bearer"
}
```

That's it!
Now that the application has an access_token, it is now able to make authorized calls to the API.

Example:
```console
curl --request GET \
  --url https://user.nubes.feller.ch/api/partner/sites \
  --header 'authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6Ik9UTTBOa1JHUVRjM01FWXlPVU5GT1RZd1F6RXlSVGd6T0RaQ09EUkVRak5DT1RJME9EQTVOZyJ9.eyJpc3MiOiJodHRwczovL2ZlbGxlci5ldS5hdXRoMC5jb20vIiwic3ViIjoiWk9POEx5SkROdVZiakRveHhDZzJyQ0l2OU5VYTNCeXNAY2xpZW50cyIsImF1ZCI6Imh0dHBzOi8vaWQuZmVsbGVyLmNoL3dpc2VyLXBhcnRuZXJzIiwiaWF0IjoxNzIxMjIwMjkxLCJleHAiOjE3MjEzMDY2OTEsImd0eSI6ImNsaWVudC1jcmVkZW50aWFscyIsImF6cCI6IlpPTzhMeUpETnVWYmpEb3h4Q2cyckNJdjlOVWEzQnlzIn0.lxko2VgQcPSNnaHBzBVSmtnbsgejrfT1RNHoojvdSurL0s3StDlbAUkKYUUar4jFtosaC5_f3t_rJXJHItCRczhVopPvxRwx_s9dTVI30htMqBgft9X41MkOWyqph49J8YO3DXxO68jadoV2XH7dgUdk1ri4-JGdGo7XOpkmPsWwCJNs8uM-9xB95LDU47Gmq7gAGX5cwmTEvrrCyCHV8_0qVRR0FBprWfkPfKfUBuERR-31HQWe_xsB9jO_S4jSFdIccT64MM29uG9zzsKvA0-M15IeEg5cCbmL3bqwlh5YsxfWAkEKxkQic7Y3_oTHDt0x0ePppMzDDIVXSa7lYA'
```

***

## API Documentation

* [Site](#site)
    * [Get all accessible sites](#get-all-accessible-site-details)
    * [Get site details](#get-site-details)
* [Heating-controls](#heating-controls)
    * [Boost using default boost temperature for site](#boost-using-default-boost-temperature-for-site)
    * [Set boost temperature by hvacgroup](#set-boost-temperature-by-hvac-group)
    * [Set target temperature by hvac group](#set-target-temperature-by-hvac-group)
    * [Set desired valve position by hvac group](#set-desired-valve-position-by-hvac-group)

## Site
A site is a Wiser installation. If the user has granted access to your application, the site is considered 'accessible'.

### Get all accessible sites
* **Path:** `https://user.nubes.feller.ch/api/partner/sites`
* **Method:** `GET`
* **Description:** Get all accessible sites. Returns the site ID and indicates whether the site is online or not.

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
Site details provide all available information about the site. The 'hvac' field contains the id of the heating group.

* **Path:** `https://user.nubes.feller.ch/api/partner/sites/{siteId}`
* **Method:** `GET`
* **Description:** Get all site details*
* **Parameters:**
    * **siteId** `(path, string, required)` - The id of the site.

```json
{
        "id": "4c788521-9001-4bdd-9287-0e9b544c411e",
        "online": true,
        "hvac": [
            {
                "id": 1,
                "ambientTemperature": 21.5,
                "targetTemperature": 25.5,
                "boost_temperature": 2,     
                "unit": "C"
            },
            {
                "id": 2,
                "ambientTemperature": 21.5,
                "targetTemperature": 25.5,
                 "boost_temperature": 2,     
                "unit": "C"
             }
        ]
}
```

## Heating-controls
* "hvac group" = heating zone (e.g. room) defined by electrician in the eSetup
* "Boost" = this function increases room temperature from the current temperature set point
* "Target temperature" = Desired temperature for the heating zone (i.e. room temperature)
* "Valve position" = **Approximation** of the valve opening: 0 = closed, 5000 = 50 %, 10000 fully open)
***

### Boost using default boost temperature for the site

* **Path:** `https://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/boost`
* **Method:** `PUT`
* **Description:** Boost all room temperature using default (2 deg) boost temperature for the site. 
* **Parameters:**
    * **siteId** `(path, string, required)`
* **Request Body:**
  ```json
  {
    "enabled": true
  }
  ```

### Set boost temperature by hvac group

* **Path:** `https://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/{hvacGroupId}/boost-temperature`
* **Method:** `PUT`
* **Description:** Set boost temperature by hvac group
* **Parameters:**
    * **siteId** `(path, string, required)` - The id of the site.
    * **hvacGroupId** `(path, string, required)` - The id of the hvac group.
* **Request Body:**
  ```json
  {
    "value": 2.5
  }
  ```

### Set target temperature by hvac group

* **Path:** `https://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/{hvacGroupId}/target-temperature`
* **Method:** `PUT`
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

### Set desired valve position by hvac group

* **Path:** `https://user.nubes.feller.ch/api/partner/sites/{siteId}/hvac/{hvacGroupId}/valve-position`
* **Method:** `PUT`
* **Description:** Set desired valve level by hvac group. 0 = closed, 10000 = open
* **Parameters:**
    * **siteId** `(path, string, required)` - The id of the site.
    * **hvacGroupId** `(path, string, required)` - The id of the hvac group.
* **Request Body:**
  ```json
  {
    "value": 10000
  }
  ```
