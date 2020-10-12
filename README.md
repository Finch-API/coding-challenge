# Backend Coding Challenge

We are the company Telematic and our product is to build a unified API across physical vehicles.

The Generic Motors (GM) car company has a terrible API. It returns badly
structured JSON which isn't always consistent. Telematic needs to adapt the API
into a cleaner format.

## Instructions

There are two API specifications provided below, the GM API and the Telematic API
Spec. Your task is to implement the Telematic spec by making HTTP requests to the
GM API.

The flow looks like this:

```
client --request--> Telematic API --request--> GM API
```

The GM API server is running at **gmapi.azurewebsites.net**. The documentation
below provides more details on the exact request/responses. Here is an example:

```bash
$ curl http://gmapi.azurewebsites.net/getVehicleInfoService \
       -X 'POST' \
       -H 'Content-Type: application/json' \
       -d '{"id": "1234", "responseType": "JSON"}'
```

Your tasks are as follows:

- Design the Telematic API to expose the GM API. The design should be extensible. What will happen when the Telematic API begins to expose Tesla's API, which is quite different to GM's.
- Implement the Telematic API of your specification using any frameworks or libraries as necessary.
- Write your code to be well structured.
- Cleanly handle inconsistent data from GM's API. Keep in mind GM's API is flaky, think about how to handle-
  - dropped connections
  - `5xx` and `429` errors from GM.
  - inconsistent data. What happens if GM returns typos in a data type?
  - anything else that can go wrong. How do we handle these issues while maintaining the best possible developer experiece?
- Provide tests for your API implementation. Add automated testing that will allow you to move fast but safely as you expand the Telematic API.
- An API call to a production grade server will go through many middleware and methods, add logging to trace through the lifecycle of a request.
- Provide documentation for the users of Telematic API to integrate as fast as possible.
- Add a readme with instructions to setup your server locally.

**Note**: Our test (GM API) server will not return all possible error scenarios. However, our test suite will simulate API flakiness that your server (Telematic API) should be able to handle.

## The GM API

Available IDs: **1234**, **1235**

### Vehicle Info

**Request:**

```
POST /getVehicleInfoService
Content-Type: application/json

{
  "id": "1234",
  "responseType": "JSON"
}
```

**Response:**

```
{
  "service": "getVehicleInfo",
  "status": "200",
  "data": {
    "vin": {
      "type": "String",
      "value": "123123412412"
    },
    "color": {
      "type": "String",
      "value": "Metallic Silver"
    },
    "fourDoorSedan": {
      "type": "Boolean",
      "value": "True"
    },
    "twoDoorCoupe": {
      "type": "Boolean",
      "value": "False"
    },
    "driveTrain": {
      "type": "String",
      "value": "v8"
    }
  }
}
```

### Security

**Request:**

```
POST /getSecurityStatusService
Content-Type: application/json

{
  "id": "1234",
  "responseType": "JSON"
}
```

**Response:**

```
{
  "service": "getSecurityStatus",
  "status": "200",
  "data": {
    "doors": {
      "type": "Array",
      "values": [
        {
          "location": {
            "type": "String",
            "value": "frontLeft"
          },
          "locked": {
            "type": "Boolean",
            "value": "False"
          }
        },
        {
          "location": {
            "type": "String",
            "value": "frontRight"
          },
          "locked": {
            "type": "Boolean",
            "value": "True"
          }
        },
        {
          "location": {
            "type": "String",
            "value": "backLeft"
          },
          "locked": {
            "type": "Boolean",
            "value": "False"
          }
        },
        {
          "location": {
            "type": "String",
            "value": "backRight"
          },
          "locked": {
            "type": "Boolean",
            "value": "True"
          }
        }
      ]
    }
  }
}
```

### Fuel/Battery Level

**Request:**

```
POST /getEnergyService
Content-Type: application/json

{
  "id": "1234",
  "responseType": "JSON"
}
```

**Response:**

```
{
  "service": "getEnergy",
  "status": "200",
  "data": {
    "tankLevel": {
      "type": "Number",
      "value": "30.2"
    },
    "batteryLevel": {
      "type": "Null",
      "value": "null"
    }
  }
}
```

### Start/Stop Engine

**Request:**

```
POST /actionEngineService
Content-Type: application/json

{
  "id": "1234",
  "command": "START_VEHICLE|STOP_VEHICLE",
  "responseType": "JSON"
}
```

**Response:**

```
{
  "service": "actionEngine",
  "status": "200",
  "actionResult": {
    "status": "EXECUTED|FAILED"
  }
}
```

## The Telematic API Spec
Specify and implement the Telematic API to expose the GM API described above.
