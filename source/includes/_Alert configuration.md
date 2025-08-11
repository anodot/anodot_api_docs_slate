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
      "modifiedBy": "string",
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
      "owner": "string",
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
          "type": "impact",
          "id": "82be-a4e8151234",
          "impact": 10000
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
        "type": "impact",
        "impact": 10000
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
    "id": "1f11979f-de3e-4d6b-81c8-2f2f4beb1111",
    "meta": {
        "createdTime": 1663145355,
        "modifiedTime": 1722166145,
        "owner": "andt-group",
        "modifiedBy": "test@anodot.com",
        "ownerId": "5e16d1c31edb9e000d2b1111",
        "modifierId": "58e5f6b3ba01b264935d1111",
        "associatedDashboardTile": null
    },
    "configuration": {
        "labels": [
            {
                "name": "Ella"
            },
            {
                "name": "two"
            }
        ],
        "timeScale": "1d",
        "type": [
            "anomaly",
            "noData"
        ],
        "title": "Demo 100",
        "description": "Test Description, trigger time: {{trigger_time}}",
        "severity": "high",
        "conditions": [
            {
                "type": "direction",
                "id": "bb3f-bd512a5561bd",
                "direction": "down"
            },
            {
                "type": "significance",
                "id": "5d61-0191fe6f3e10",
                "significance": 0.35000000000000003
            },
            {
                "type": "delta",
                "id": "1aad-323c29a3d598",
                "absolute": 45.50929567122151,
                "percentage": 10,
                "deltaDuration": {
                    "enabled": false,
                    "rollup": "longlong",
                    "minDuration": 86400
                },
                "enableAutoTuning": true
            },
            {
                "type": "duration",
                "id": "77e3-a6e0038dfccd",
                "duration": 86400
            },
            {
                "type": "impact",
                "id": "82be-a4e8151234",
                "impact": 10000
            },
            {
                "type": "influencing",
                "id": "ebfb-7dc2e167854c",
                "matchingProperties": [
                    "lineItem_ProductCode",
                    "lineItem_LineItemType"
                ],
                "rollup": "longlong",
                "useAnomalyValues": false,
                "subConditions": [
                    {
                        "type": "ABOVE",
                        "upperBound": "UPPER",
                        "maxValue": 100
                    }
                ],
                "expressionTreeModel": {
                    "title": null,
                    "id": "dab8-88c484d761db",
                    "name": null,
                    "displayOnly": true,
                    "mtype": "ALERT",
                    "namingSchema": "COMPOSITE_V2",
                    "expressionTree": {
                        "root": {
                            "type": "function",
                            "id": "da65-2f8fed1116c3",
                            "children": [
                                {
                                    "type": "metric",
                                    "id": "7a3b-c9f6a7b95934",
                                    "children": [],
                                    "searchObject": {
                                        "q": null,
                                        "expression": [
                                            {
                                                "type": "property",
                                                "key": "what",
                                                "value": "lineItem_BlendedCost",
                                                "exact": true
                                            }
                                        ],
                                        "ids": null,
                                        "originSourceIds": null
                                    },
                                    "leaf": true
                                }
                            ],
                            "function": "groupBy",
                            "parameters": [
                                {
                                    "name": "Aggregation",
                                    "value": "Sum"
                                },
                                {
                                    "name": "Group By",
                                    "value": "{\"properties\":[\"lineItem_ProductCode\",\"lineItem_LineItemType\"]}"
                                }
                            ],
                            "leaf": false
                        }
                    },
                    "alias": null,
                    "filter": null,
                    "scalarTransforms": null,
                    "calculationDelay": null,
                    "originId": null,
                    "owner": null,
                    "ownerId": null,
                    "excludeComposites": true,
                    "includeCubes": null,
                    "startBucketMode": false,
                    "description": null,
                    "watermarkBased": null
                },
                "enableAutoTuning": false,
                "failOnAbsence": true
            },
            {
                "type": "volume",
                "id": null,
                "value": 3090.966857482553,
                "rollup": "weekly",
                "bound": "LOWER",
                "numLastPoints": 1,
                "enableAutoTuning": true,
                "enabled": false
            }
        ],
        "channels": [],
        "subscribers": [
            "test@anodot.com",
            "test1@anodot.com"
        ],
        "notifyOnlyOpen": false,
        "noDataAlert": true,
        "noDataDuration": 7200,
        "expressionTreeModel": {
            "title": "Demo_100_lineItem_BlendedCost",
            "id": "61b85eab-f91e-4c42-95dc-4255795ec8b3",
            "name": {
                "prefix": "what=lineItem_BlendedCost.func=groupBy",
                "auto": false
            },
            "displayOnly": false,
            "mtype": "ALERT",
            "namingSchema": "COMPOSITE_V2",
            "expressionTree": {
                "root": {
                    "type": "function",
                    "id": "8a29-985a4d878a50",
                    "children": [
                        {
                            "type": "metric",
                            "id": "5342-99389c314ca5",
                            "children": [],
                            "searchObject": {
                                "q": null,
                                "expression": [
                                    {
                                        "type": "origin",
                                        "originType": "STREAM",
                                        "key": "originId",
                                        "value": "SSSKg4EXKZ111",
                                        "exact": true
                                    },
                                    {
                                        "type": "property",
                                        "key": "what",
                                        "value": "lineItem_BlendedCost",
                                        "exact": true
                                    }
                                ],
                                "ids": null,
                                "originSourceIds": null
                            },
                            "leaf": true
                        }
                    ],
                    "function": "groupBy",
                    "parameters": [
                        {
                            "name": "Aggregation",
                            "value": "Sum"
                        },
                        {
                            "name": "Group By",
                            "value": "{\"properties\":[\"lineItem_LineItemType\",\"lineItem_ProductCode\"]}"
                        }
                    ],
                    "leaf": false
                }
            },
            "alias": null,
            "filter": null,
            "scalarTransforms": null,
            "calculationDelay": null,
            "originId": "1f11979f-de3e-4d6b-81c8-2f2f4beb1111",
            "owner": null,
            "ownerId": "5e16d1c31edb9e000d2b49ec",
            "excludeComposites": true,
            "includeCubes": null,
            "startBucketMode": false,
            "description": null,
            "watermarkBased": null
        },
        "search": {
            "type": "composite",
            "compositeId": "61b85eab-f91e-4c42-95dc-4255795e1111"
        },
        "correlatedEvents": null,
        "correlatedEventsFilters": [
            {
                "aggregation": {
                    "aggregationField": "category",
                    "resolution": "longlong",
                    "topEventSize": 10,
                    "maxBuckets": null
                },
                "filter": {
                    "categories": [],
                    "type": "SUPPRESS",
                    "q": {
                        "q": null,
                        "expression": [
                            {
                                "type": "property",
                                "key": "category",
                                "value": "other",
                                "exact": true
                            },
                            {
                                "type": "property",
                                "key": "Product",
                                "value": "*",
                                "exact": false
                            }
                        ],
                        "ids": null,
                        "originSourceIds": null
                    }
                },
                "lookupTable": null,
                "eventMetricFilter": [
                    {
                        "eventProperty": "Product",
                        "metricDimension": "lineItem_ProductCode"
                    }
                ]
            }
        ],
        "alertActions": [
            "66a62a857e6957242786ead9"
        ],
        "updatePolicy": {
            "time": 1800
        }
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

<aside class="success">
A Pro Tip:</br>
This response includes various configuration settings, including a composite expression, conditions, event correlation and influencing metrics.
</aside>

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
