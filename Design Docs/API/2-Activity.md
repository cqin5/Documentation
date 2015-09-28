## 2. Activity

An activity can be either an appointment or an event.

### 2.1 Create Appointment

**Resource URL**: `POST /api/activity?type=appointment`

**Authority**: authenticated

**Request Body**

    {
      "participants":["participantId"],
      "location": {
        "someKey": "someValue"
      },
      "time": {
        "start": aUnixTimestamp,
        "end": aUnixTimestamp,
        "duration": "P1Y2M10DT2H30M",
        "repeat": {
          "count": 10,
          "interval": "DAILY|WEEKLY|MONTHLY"
        }
      }
    }

**Response Body (200)**

    {
      "external_id": "someUUID"
    }

### 2.2 Create Event

**Resource URL**: `POST /api/activity?type=event`

**Authority**: ROLE `SALESPERSON`

**Request Body**

    {
      "name": "EventName",
      "participants":["participantId"],
      "location": {
        "someKey": "someValue"
      },
      "time": {
        "start": aUnixTimestamp,
        "end": aUnixTimestamp,
        "duration": "P1Y2M10DT2H30M"
      }
    }

**Response Body**

    {
      "external_id": "someUUID"
    }

### 2.3 Adding more participants

**Resource URL**: `POST /api/activity/$activityId/participant`

**Authority**: authenticated as creator of event

**Request Body**

    {
      "participants": ["newGuy1Id", "newGuy2Id"]
    }

**Response Code**

- `204`: success
- `401`: not creator of event

### 2.4 Decide on an activity invitation

**Resource URL**: `POST /api/activity/$activityId/decision`

**Authority**: authenticated

**Request Body**

    {
      "decision": "MAYBE|GOING|NOTGOING"
    }

**Response Code**

- `204`: success

### 2.5 Get the details of an activity

**Resource URL**: `GET /api/activity/$activityId`

**Authority**: creator or invited participants

**Response Body(200)**

    {
      "participants":["participantId"],
      "location": {
        "someKey": "someValue"
      },
      "time": {
        "start": aUnixTimestamp,
        "end": aUnixTimestamp,
        "duration": "P1Y2M10DT2H30M",
        "repeat": {
          "count": 10,
          "interval": "DAILY|WEEKLY|MONTHLY"
        }
      }
    }

**Possible Error**

- `404`: activity not found
- `401`: insufficient authority
