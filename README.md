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

+ Implement the Telematic API specification using any frameworks or libraries as necessary
+ Provide tests for your API implementation
+ Write your code to be well structured and documented

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

### Vehicle Info

**Request:**

```
GET /vehicles/:id
```

**Response:**

```
{
  "vin": "1213231",
  "color": "Metallic Silver",
  "doorCount": 4,
  "driveTrain": "v8"
}
```

### Security

**Request:**

```
GET /vehicles/:id/doors
```

**Response:**

```
[
  {
    "location": "frontLeft",
    "locked": true
  },
  {
    "location": "frontRight",
    "locked": true
  },
  {
    "location": "backLeft",
    "locked": true
  },
  {
    "location": "backRight",
    "locked": false
  }
]
```

### Fuel Range

**Request:**

```
GET /vehicles/:id/fuel
```

**Response:**

```
{
  "percent": 30.2
}
```

### Battery Range

**Request:**

```
GET /vehicles/:id/battery
```

**Response:**

```
{
  "percent": 50.3
}
```

### Start/Stop Engine

**Request:**

```
POST /vehicles/:id/engine
Content-Type: application/json

{
  "action": "START|STOP"
}
```

**Response:**

```
{
  "status": "success|error"
}
```
