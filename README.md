# EdgeConnect Service Specification (Portuguese Pilot)

## Services Functional Specifications

### General Information and Purpose
**Service Name/Title:** EdgeConnect

**Description and purpose:**
The EdgeConnect platform provides stakeholders (i.e., consumers, flexibility service providers,
aggregators, DSOs and TSOs) along the value chain of flexibility provision with an integrated
ecosystem to support all main activities in this value chain, to help identify, unlock and make
use of all available flexibility potential. As a multi-stakeholder platform, it comprehends several
views, providing distinct value propositions for each stakeholder.

**Owner/Contact Information:**
INESC TEC

---

### Functional Requirements
- The system must be able to onboard all its stakeholders
- The system must allow consumers to register their flexible assets
- The system must match consumers with the relevant energy community service provider
- The system should allow technical platforms to test activation commands on the
consumers flexible assets, to verify their capability to provide flexibility
- The system must support the certification of Aggregators to ensure they are financially
and technically eligible to participate in the flexibility market
- The system must recognize that market and pre-qualification processes can occur
outside its ecosystem and provide functionalities to interface with these external
processes
- The system must support crucial flexibility value chain operations, namely share
flexibility needs, share selected winning bids, distribute activation setpoints, validation
of mobilized flexibility and calculation of the settlement

---

### Non-Functional Requirements
- The system must store data securely in an encrypted manner
- The system must exchange data securely
- The system should have an uptime of 99%
- The system must use the OAuth 2.0 protocol for user authorization and role
  management
- Requests should not take longer than 10 seconds to produce a response

---

### Service Interfaces

The complete list of endpoints, using swagger format, can be found here. Below, there is a
description of the most important endpoints of the EdgeConnect platform for the use cases of
the PT pilot.

#### Endpoint 1: Create a qualification request

- **URL:** `/api/v1/qualification`
- **Method:** `POST`
- **Description:** Creates a qualification for a given requester.
- **Headers:**
  - Content-Type: application/json
  - Authorization: Bearer <token>
- **Request Body:**
```json
{
   "requesterId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
   "objectToQualifyId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
}
```
- **Response Examples:**
  - `201 Created`: `{ "message": "qualification created" }`
  - `400 Bad Request`: `{ "message": "invalid data format" }`
  - `401 Unauthorized`: `{ "message": "Unauthorized access" }`

---

#### Endpoint 2: Create flexibility needs

- **URL:** `/api/v1/flexibility/needs`
- **Method:** `POST`
- **Description:** Stores information about the flexibility needs of a DSO/TSO
- **Headers:**
  - Content-Type: application/json
  - Authorization: Bearer <token>
- **Request Body:**
```json
{
   "contractDuration": 300,
   "contractId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
   "type": "longFlexibilityNeeds",
   "startDatetime": "2024-11-08T17:48:07.220Z",
   "endDatetime": "2024-11-08T17:48:07.220Z",
   "gateOpenTimestamp": "2024-11-08T17:48:07.220Z",
   "gateCloseTimestamp": "2024-11-08T17:48:07.220Z",
   "productId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
   "requesterId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
   "flexibilityZoneId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
   "volume": 500,
   "maxReservationPrice": 300.5,
   "maxActivationPrice": 1000.65
}
```
- **Response Examples:**
  - `201 Created`: `{ "message": "flexibility need created" }`
  - `400 Bad Request`: `{ "message": "invalid data format" }`
  - `401 Unauthorized`: `{ "message": "Unauthorized access" }`

---

#### Endpoint 3: Create flexibility bid

- **URL:** `/api/v1/flexibility/bids`
- **Method:** `POST`
- **Description:** Creates a new flexibility bid.
- **Headers:**
  - Content-Type: application/json
  - Authorization: Bearer <token>
- **Request Body:**
```json
{
   "minQuantity": 2,
   "maxQuantity": 4,
   "startDatetime": "2024-11-08T17:50:18.482Z",
   "endDatetime": "2024-11-08T17:50:18.482Z",
   "maxNumberActivations": 4,
   "activationPrice": 55.5,
   "reservationPrice": 65.5,
   "divisible": true,
   "recoveryTime": 60,
   "businessPartnerId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
   "flexibilityZoneId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
   "serviceId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
   "flexibilityNeedsId": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
}
```
- **Response Examples:**
  - `201 Created`: `{ "message": "flexibility bid created" }`
  - `400 Bad Request`: `{ "message": "invalid data format" }`
  - `401 Unauthorized`: `{ "message": "Unauthorized access" }`

---

#### Endpoint 4: Create baseline

- **URL:** `/api/v1/flexibility/baselines`
- **Method:** `POST`
- **Description:** Creates a baseline data entry.
- **Headers:**
  - Content-Type: application/json
  - Authorization: Bearer <token>
- **Request Body:**
```json
{
   "startDatetime": "2022-10-30T12:45:00Z",
   "endDatetime": "2022-10-30T20:45:00Z",
   "type": "baseline",
   "periodNumber": 60,
   "serviceId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
   "assetIds": [
      "3fa85f64-5717-4562-b3fc-2c963f66afa6"
   ],
   "podId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
   "intervalDataEntries": [
      {
         "datetime": "2022-12-30T20:45:00Z",
         "units": "w",
         "dataValue": "600",
         "variableName": "power"
      }
   ]
}
```
- **Response Examples:**
  - `201 Created`: `{ "message": "flexibility baseline created" }`
  - `400 Bad Request`: `{ "message": "invalid data format" }`
  - `401 Unauthorized`: `{ "message": "Unauthorized access" }`

---

#### Endpoint 5: Flexibility bid activation

- **URL:** `/api/v1/flexibility/activation/{flexibilityBidId}`
- **Method:** `PATCH`
- **Description:** Activate a flexibility bid by its id.
- **Headers:**
  - Content-Type: application/json
  - Authorization: Bearer <token>
- **Path:**
  - flexibilityBidId: string UUID
- **Response Examples:**
  - `204 No Content`
  - `400 Bad Request`: `{ "message": "invalid data format" }`
  - `401 Unauthorized`: `{ "message": "Unauthorized access" }`

