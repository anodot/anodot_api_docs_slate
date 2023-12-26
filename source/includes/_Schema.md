## Schema

### The Schema Object

> End Point **/api/v2/stream-schemas**


The Schema API enables you to create schemas as a preliminary step to sending Metrics to Anodot using Metrics 3.0 protocol. 

* The schema object defines the required measures and their attributes:
  * Units
  * Aggregation
  * Counting method
  * Filling the gap policy for missing data points 
* The schema object also defines the required dimensions and attributes:
  * Name
  * Fill policy for missing dimension values

### Create Stream Schema

> Request Example: Creating a schema

```shell
curl --location --request POST 'https://app.anodot.com/api/v2/stream-schemas' \
--header 'Authorization: Bearer {{bearer-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
 "version": "1",
 "name": "schema name",
 "dimensions": [
   "OS",
   "GEO"
 ],
 "measurements": {
   "m1": {
     "units": "sec",
     "aggregation": "average",
     "countBy": "none"
   },
   "m2": {
     "units": "sec",
     "aggregation": "average",
     "countBy": "none"
   }
 },
 "missingDimPolicy": {
   "action": "fill",
   "fill": "dummy_val"
 },
 "dvpConfig": {
  "baseRollup": "LONGLONGROLLUP",
  "maxDVPDurationHours": 168,
  "preventNoData": true,
  "gaugeValue": {
    "keepLastValue": true,
    "value": 0.0
  },
  "counterValue": {
    "keepLastValue": false,
    "value": 0.0
  }
 }
}'
```

Use this request to create a new schema

**Request Arguments**

Argument | Type | Description
------|------|------------
version | String | (Optional) Describe the schema version
name | String | (Required) max 200 chars Schema name, will be later used as the stream name
Dimensions | Array | (Required) List of dimensions, max 30. Each dimension must be unique. Each dimension name is up to 30 chars. 
measurements | Array | (Required) List of measurements, max 200. Each measure name is up to 50 chars.
For each measurement: | |
units | String | (Optional) Provide the unit for the measure, this unit will be used in Anodot display 
aggregation | String | (Required) Aggregation function for the measure. Valid values: average , sum
countby | String | (Required) How to count the data points. Valid value: currently “None” is the only value supported (each bucket counts as “1”)
missingDimPolicy |  | How to treat missing dimension values in the posted data.
action | String | Valid values:</br>fill - the empty dimension value will be filled with the value in the “fill” field (see below).</br>fail - data point will be rejected.</br>ignore - allow empty dimension values, such dimensions may be missing from the metrics.
fill | String | Fill value. Applicable if action field is set to fill
dvpconfig | Section | (Optional) Defines the way missing data points will be completed in this schema
baseRollup | ENUM | Define which is the minimal Anodot rollup to complete the data for. The possible values are</br>SHORTROLLUP (1 minute)</br>MEDIUMROLLUP (5 minutes)</br>LONGROLLUP (1hour)</br>LONGLONGROLLUP (1 day)</br>WEEKLY (1 week)
maxDVPDurationHours | Integer | The number of hours Anodot will complete missing data points for a metric. The maximal completion period depends on the stream's interval. Daily streams can be filled up to 31 days (744 hours), hourly streams up to 7 days (168 hours) and less than hourly streams can be filled up to 1 day (24 hours)
preventNoData | Boolean | Keep this as "true"
gaugeValue / counterValue | section | The definition for gauge (average) or counter (sum) aggregation type measures in the stream related to this schema
keepLastValue | Boolean | true - use the last received value as the completion value. false - set the completion value manually.
value | Float | The completion value to use, in case keepLastValue is false.


#### Response fields

> Response Example

```json
{
  "schema": {
    "id": "111111-22222-3333-4444",
    "version": "1",
    "name": "schema name",
    "dimensions": [
      "OS",
      "GEO"
    ],
    "measurements": {
      "m1": {
        "units": "sec",
        "aggregation": "average",
        "countBy": "none"
      },
      "m2": {
        "units": "sec",
        "aggregation": "average",
        "countBy": "none"
      }
    },
    "missingDimPolicy": {
      "action": "fill",
      "fill": "dummy_val"
    }
  },
  "dvpConfig": {
            "baseRollup": "LONGLONGROLLUP",
            "maxDVPDurationHours": 168,
            "preventNoData": true,
            "gaugeValue": {
                "keepLastValue": true,
                "value": 0.0
            },
            "counterValue": {
                "keepLastValue": false,
                "value": 0.0
            }
        }
    },
  "meta": {
    "createdTime": “1547630800000”,
    "modifiedTime": “1547630800000”
  }
}
```

The response contains the schema details and provides a schema id for you to use when posting metrics to this schema.

### Get all Account Stream Schemas

> Request Example: Get all schemas

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/stream-schemas/schemas' \
--header 'Authorization: Bearer {{bearer-token}}'
```

After you have created the schema, you can use this call to get the list of all schemas in the account. 

#### Response fields

> Response Example

```json 
[
   {
       "streamSchemaWrapper": {
           "schema": {
               "id": "0MCop0RjSsWk3W-QGmpnWA",
               "version": "1",
               "name": "Anodot Usage 002 - Hourly",
               "dimensions": [
                   "Event_Action",
                   "Event_Category",
                   "Event_Label",
                   "Page_path_level_2",
                   "Page_path_level_3",
                   "Page_path_level_4",
                   "Version"
               ],
               "measurements": {
                   "Total_Events": {
                       "aggregation": "sum",
                       "countBy": "none"
                   },
                   "Pageviews": {
                       "aggregation": "sum",
                       "countBy": "none"
                   }
               },
               "missingDimPolicy": {
                   "action": "ignore"
               }
           },
           "meta": {
               "createdTime": 1533540645730,
               "modifiedTime": 1533540645730
           }
       },
       "schemaCubesWrapper": {}
   }
```

If the request is successfull, you will get an array of "streamsSchemaWrapper" objects. Each of these objects has the following properties:

Argument | Description
---------|------------
schema | Schema object (see above for definition)
meta | Create and modified time of the schema.

### Get Stream Schema

> Request Example: Get a schema by Id

```shell
curl --location --request GET 'https://app.anodot.com/api/v2/stream-schemas/11111-22222-33333-4444' \
--header 'Authorization: Bearer {{bearer-token}}'
```

Use this API to get a specific schema by Id.

Name | Description
-----|------------
id (string) | Schema Id to retrieve

#### Response fields

> Respone Example:

```json
{
  "schema": {
    "id": "111111-22222-3333-4444",
    "version": "1",
    "name": "revenue schema",
    "dimensions": [
      "OS",
      "GEO"
    ],
    "measurements": {
      "m1": {
        "units": "sec",
        "aggregation": "average",
        "countBy": "samples"
      },
      "m2": {
        "units": "sec",
        "aggregation": "average",
        "countBy": "samples"
      }
    },
    "missingDimPolicy": {
      "action": "fill",
      "fill": "dummy_val"
    }
  },
  "meta": {
    "createdTime": 1547630800000,
    "modifiedTime": 1547630800000
  }
}
```

If the request is successfull, you will get an array of "streamsSchemaWrapper" objects. Each of these objects has the following properties:

Argument | Description
---------|------------
schema | Schema object (see above for definition)
meta | Create and modified time of the schema.

### Delete Stream Schema

> Request Example: Delete a schema by Id

```shell
curl --location --request DELETE 'https://app.anodot.com/api/v2/stream-schemas/11111-22222-33333-4444' \
--header 'Authorization: Bearer {{bearer-token}}'
```

Use this API to delete a stream schema by Id.

Name | Description
-----|------------
id (string) | Schema Id to delete.

#### Response fields

> Respone Example: 

```json
{
  "deleted": "111111-22222-3333-4444"
}
```

If successful - the call will return the id of the schema which was deleted.

### Update Stream Schema

The process to update a schema is listed here.

1. Perform a **GET schema** request by ID to get the relevant schema's configuration, including it's ID.
2. Delete the schema.
3. In a text editor - Update the schema's configuration according to your requirements.
4. Perform a **Create schema** request, **remember to include the original schema ID as part of the request**.</br>This will direct Anodot to create the schema using the same ID.


