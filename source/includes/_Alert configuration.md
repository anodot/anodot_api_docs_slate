## Alert Configuration

> End Point prefix is **/api/v2/alerts**


Use the *Alert Config API* to:

* Fetch all alert configurations
* Create a new alert
* Edit an existing alert
* Pause / Resume an alert
* Delete an existing alert

Authentication type: [Access Token Authentication] (#access-tokens).

### List all alert configurations

> Request Example: Get All Alert Configurations

```shell
curl -X GET \
"https://app.anodot.com/api/v2/alerts" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to get All Alert Configurations and their respective ID's

<aside class="success">
A Pro Tip:</br>
Use this API to retrieve the alert IDs that are used to delete, edit, pause/resume alerts.
</aside>

#### Response Fields

> Response Example:

```json
[
  {
    "id": "20262fb3-00ba-412c-a515-aec74c4824cd",
    "meta": {
      "createdTime": 1513760272,
      "modifiedTime": 1513760272,
      "createdBy": "string",
      "modifiedBy": "string",
      "creatorId": "20262fb3-00ba-412c-a515-aec74c4824cd",
      "modifierId": "20262fb3-00ba-412c-a515-aec74c4824cd",
      "associatedDashboardTile": {
        "dashboardId": "20262fb3-00ba-412c-a515-aec74c4824cd",
        "tileId": "30262fb3-00ba-412c-a515-aec74c4824cd"
      }
    },
    "configuration": {
      "labels": [
          {
              "name": "documentation"
          },
          {
              "name": "API"
          }
      ],
      "timeScale": "5m",
      "type": [
        "anomaly"
      ],
      "title": "Jetty servers availability",
      "owner": "user@domain.com",
      "description": "This alerts monitors the availablity of the jetty servers",
      "search": {
        "type": "expression",
        "expression": {
          "expression": [
            {
              "type": "string",
              "key": "host",
              "value": "anodot13",
              "isExact": true
            }
          ]
        },
        "compositeId": "20262fb3-00ba-412c-a515-aec74c4824cd"
      },
      "severity": "critical",
      "isOnlyOpen": false,
      "channels": [
        "20262fb3-00ba-412c-a515-aec74c4824cd"
      ],
      "subscribers": [
        "20262fb3-00ba-412c-a515-aec74c4824cd"
      ],
      "conditions": [
        {
          "type": "delta",
          "absolute": 46.85,
          "percentage": 10,
          "deltaDuration": null,
          "enableAutoTuning": true
        },
        {
          "type": "significance",
          "id": "82be-a4e815888888",
          "significance": 0.7
        },
        {
          "type": "duration",
          "id": "82be-a4e81599999",
          "duration": 86400
        },
        {
          "type": "direction",
          "id": "82be-a4e81598765",
          "direction": "up"
        },
        {
          "type": "volume",
          "id": null,
          "value": 2596.68,
          "rollup": "weekly",
          "bound": "LOWER",
          "numLastPoints": 1,
          "enableAutoTuning": true,
          "enabled": true
        },
        {
          "type": "minParticipating",
          "id": "82be-a4e815b12345",
          "absolute": 1,
          "percentage": null
        },
        {
          "type": "threshold",
          "id": "82be-a4e815baxxxx",
          "upperBound": "greaterThan",
          "lowerBound": "lessThan",
          "upperValue": 100,
          "lowerValue": 5
        }
      ],
      "correlatedEvents": {
        "query": {
          "expression": [
            {
              "type": "string",
              "key": "host",
              "value": "anodot13",
              "isExact": true
            }
          ]
        }
      }
    },
    "state": {
      "state": "string",
      "resumeTime": 1513760272,
      "pausedTime": 1513760272,
      "pausedBy": "string",
      "pausedId": "string"
    },
    "validation": {
      "isValid": true,
      "general": [
        {
          "id": 2987,
          "message": "Alert excceded maximum amount of allowed metrics."
        }
      ]
    }
  }
]
```

This call will return an array of alert configurations, each one with the following structure:

Field | Type | Description / Example
-|-|-
id | String | Alert id. This id can be used in future calls for alert creation.
Meta | Array | Alert meta deta such as creation details (time, owner, etc.). 
Configuration | Array | The configuration details for the alert. For a detailed breakdown, see [Create a new alert] (#create-a-new-alert).
State | Array | The current state of the alert (paused/live)

### Create a new alert

> Request Example: Create a new alert

```shell
curl -X POST \
"https://app.anodot.com/api/v2/alerts" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

>Request JSON Example:

```json
{
  "timeScale": "5m",
  "type": [
    "anomaly"
  ],
  "title": "Jetty servers availability",
  "description": "This alerts monitors the availablity of the jetty servers",
  "search": {
    "type": "expression",
    "expression": {
      "expression": [
        {
          "type": "string",
          "key": "host",
          "value": "anodot13",
          "isExact": true
        }
      ]
    },
    "compositeId": "20262fb3-00ba-412c-a515-aec74c4824cd"
  },
  "severity": "critical",
  "isOnlyOpen": false,
  "channels": [
    "20262fb3-00ba-412c-a515-aec74c4824cd"
  ],
  "subscribers": [
    "20262fb3-00ba-412c-a515-aec74c4824cd"
  ],
  "conditions": [
    {
      "type": "delta",
      "absolute": 46.85,
      "percentage": 10,
      "deltaDuration": null,
      "enableAutoTuning": true
    },
    {
        "type": "significance",
        "significance": 0.7
    },
    {
        "type": "duration",
        "duration": 86400
    },
    {
        "type": "direction",
        "direction": "both"
    },
    {
        "type": "volume",
        "id": null,
        "value": 2596.686320664465,
        "rollup": "weekly",
        "bound": "LOWER",
        "numLastPoints": 1,
        "enableAutoTuning": true,
        "enabled": true
    },
    {
        "type": "minParticipating",
        "absolute": 1,
        "percentage": null
    },
    {
        "type": "maxParticipating",
        "absolute": null,
        "percentage": null
    },
    {
        "type": "threshold",
        "upperBound": "greaterThan",
        "lowerBound": "lessThan",
        "upperValue": 100,
        "lowerValue": 5
    }
  ],
  "correlatedEvents": {
    "query": {
      "expression": [
        {
          "type": "string",
          "key": "host",
          "value": "anodot13",
          "isExact": true
        }
      ]
    }
  },
  "labels": [
    {
        "name": "new label"
    },
    {
        "name": "API"
    }
  ]
}
```

Create a new alert configuration.</br>
Please note, the new alert owner will be the token owner.

<aside class="success">
A Pro Tip:</br>
Get example alert configurations by calling the GET alerts API. You can then use the "configuration": {} section as a jumping off point to creating your own alerts.
</aside>

**Request Arguments**

Selected Request Arguments:

Argument | Type | Description
---------|------|------------
timescale | String | The timescale of the alert, valid options include (1m,5m,1h,1d,1w)
type | String | The type of anomaly (anomaly, static, no data)
title | String | The title that will appear when an alert is fired
description | String | The description that will appear when an alert is fired
search | Array | The alert query. Search by a key/value pair or via a stream ID. To get stream IDs, please see [GET data streams] (#get-data-streams).
severity | String | Add additional meta data (critical,high,medium,low and info)
isOnlyOpen | Boolean | Enable to get notifications on alert start and alert close
channels | String | Use this to determine which channels the alert will fire to. To get channel IDs, please see [GET channels] (#get-channels).
subscribers | String | Use this to determine which user's email the alert will fire to. To get subscriber IDs, use the GET alerts API to see an example of an alert that has already been set up with the relevant subscriber ID.
conditions | Array | Anomaly alerts require: direction, significance and duration conditions. Static alers require duration and threshold conditions. No data alerts require the no data duration condition. 
correlatedEvents | Array | Determines if static events that are sent via the events API are included in the alert. Filter out events based on a key:value pair.
labels | Array | Assigns labels to the alert. Notice that if a label specified in the call does not exist in the system it will be created. 

### Get Alert by ID

>Request Example:

```shell
curl -X GET \
"https://app.anodot.com/api/alerts/{alertID}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to get a single alert configuration

> Response Example:

```json
{
    "id": "864fe6b3-32db-46f3-a7bf-bd1ca2422ef6",
    "meta": {
        "createdTime": 1617569462,
        "modifiedTime": 1627973755,
        "owner": "andt-group",
        "modifiedBy": "coyote@acme.corp",
        "ownerId": "5ffc6662c4b329000e9d1066",
        "modifierId": "5ffc6744c4b329000e9d1067",
        "associatedDashboardTile": null
    },
    "configuration": {
        "labels": [
            {
                "name": "documentation"
            },
            {
                "name": "API"
            }
        ],
        "timeScale": "1h",
        "type": [
            "anomaly"
        ],
        "title": "Anomaly on Pageviews by {{Page_Title}} from Slate API views",
        "description": "",
        "severity": "high",
        "conditions": [
            {
                "type": "direction",
                "id": "3e04-072646de894d",
                "direction": "both"
            },
            {
                "type": "duration",
                "id": "1254-7113a0ad468e",
                "duration": 7200
            },
            {
                "type": "significance",
                "id": "3d64-739d8d5b2b4c",
                "significance": 0.75
            },
            {
                "type": "delta",
                "id": "b9e4-5a5ba2e13a9d",
                "absolute": 1.0135593220338988,
                "percentage": 10,
                "deltaDuration": {
                    "enabled": false,
                    "rollup": "long",
                    "minDuration": 7200
                },
                "enableAutoTuning": true
            },
            {
                "type": "volume",
                "id": null,
                "value": 8,
                "rollup": "longlong",
                "bound": "LOWER",
                "numLastPoints": 1,
                "enableAutoTuning": true,
                "enabled": true
            }
        ],
        "channels": [
            "c5ab76da-8c30-4cee-9b70-389a3176f3cf"
        ],
        "subscribers": [
            "roadrunner@acme.corp"
        ],
        "search": {
            "type": "composite",
            "compositeId": "bcbb62cc-8112-4b04-8826-827a5d7588c9"
        },
        "correlatedEvents": null
    },
    "state": {
        "state": "active",
        "resumeTime": null,
        "pausedTime": null,
        "pausedBy": null,
        "pausedId": null
    },
    "validation": {
        "isValid": true
    }
}
```

**Request Arguments**

Argument | Type | Description
---------|------|------------
id | String | Alert id to retrieve.

**Response Fields**

Similar to the results returned by the [List all alert configurations] (#list-all-alert-configurations) call. 

### Update Alert Configuration

>Request Example:

```shell
curl -X PUT \
"https://app.anodot.com/api/v2/alerts/{alertID}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to edit an alert

**Request Arguments**

Argument | Type | Description
---------|------|------------
id | String | Alert id to edit.

<aside class="notice">
Pro Tip Reminder:</br>
Get the alert configuration by calling the GET alerts API, and then extract the "configuration": {} section. Use it as the basis to update and create alerts.
</aside>

**Response**

The updated configuration of the alert (same structure as the Get Alert call)

### Delete alert

>Request Example:

```shell
curl -X DELETE \
"https://app.anodot.com/api/v2/alerts/{alertID}" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to delete an alert

**Request Arguments**

Argument | Type | Description
---------|------|------------
id | String | Alert id to delete.

### Pause Alert

>Request Example:

```shell
curl -X POST \
"https://app.anodot.com/api/v2/alerts/{alertID}/pause" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to pause an alert.

**Request Arguments**

Argument | Type | Description
---------|------|------------
id | String | Alert id to pause.

### Resume Alert

>Request Example:

```shell
curl -X POST \
"https://app.anodot.com/api/v2/alerts/{alertID}/resume" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Use this API to resume an alert.

**Request Arguments**

Argument | Type | Description
---------|------|------------
id | String | Alert id to resume.
