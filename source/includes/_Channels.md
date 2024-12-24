## Channels

> End Point **GET /api/v2/channels**

Channels are the outgoing integrations used to send alert triggers to their destinations.
Anodot supports multiple channel types. For a full list of supported channel types, please see <a href='https://support.anodot.com/hc/en-us/articles/207168999-Managing-Alert-Channels'>here</a>

Use the channel API endpoint to get the channels available in your account and link them to alerts you create using the alerts API. You may also use the same endpoint to create new channels programmaticaly. 

Authentication type: [Access Token Authentication] (#access-tokens).

### Get channels

#### Request Arguments

> Request Example: GET All channels in the account

```shell
curl -X GET \
https://app.anodot.com/api/v2/channels \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

> Request Example: GET All channels of type slack

```shell
curl -X GET \
https://app.anodot.com/api/v2/channels?type=slack \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

> Request Example: GET All channels of type slack containing "alert" in the name

```shell
curl -X GET \
https://app.anodot.com/api/v2/channels?type=slack&name=alert \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
```

Argument | Type | Description
---------|------|------------
type [**Optional**] | string | Limit the response to this channel type.<br/>Use single, lowercase word. Possible values are listed in the [channel list] (#channel-list) below.
name [**Optional**] | string | Limit the response to anodot channel names containing this string

#### Response Fields

> Response Example:

```json
[
    {
        "id": "2b7465e5-dbda-46f5-ae67-xxxxxyyyyy",
        "name": "Test",
        "type": "email"
    },
    {
        "id": "558ef7a2-7064-49be-873d-xxxxxyyyyy",
        "name": "test slack",
        "type": "slack"
    }
] 
```

The response is a list of channels

Field | Type | Description / Example
-|-|-
id | String ($uuid) | channel id. Use this id when you create alerts via API and need to provide a destination channel.
type | String | Channel type. See possible values in [channel list] (#channel-list)
name | String | Channel name.

### Create channel

#### Request Arguments

> Request Example: Create a webhook channel in the account

```shell
curl -X POST \
https://app.anodot.com/api/v2/channels/webhook \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-d '{
    "channelData":
    {
        "authenticate": false,
        "url":"https://www.acme.corp/webhookurl/"
    },
    "name":"hudson hook"
}'
```

> Request Example: Create a slack channel in the account 

```shell
curl -X POST \
https://app.anodot.com/api/v2/channels/slack \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-d '{
    "channelData": {
        "url": "https://acme.slack.com/webhookurl",
        "channel": "#nocnoc"
    },
    "name": "noc slack channel"
}'
```

> Request Example: Create an email channel in the account 

```shell
curl -X POST \
https://app.anodot.com/api/v2/channels/email \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer ${TOKEN}"
-d '{
    "channelData": {
      "emailAddresses": [
        "testAnodot@gmail.com"
      ]
    },
    "name":"test_Anodot",
    "ungroupBasedOnMetrics": true
}'
```


Argument | Type | Description
---------|------|------------
type | string [Enum] | Type of channel to be created (appended to the request URL)<br/>Use single, lowercase word. Possible values are listed in the [channel list] (#channel-list) below.
parameters  | JSON Object | A JSON object with the required parameters for the creating the relevant channel type. See examples on the right for the relevant paylod for each type. 

#### Response Fields

> Response Example (creating a webhook channel):

```json
{
    "id": "70eec7d8-da35-453a-9e84-5924abe810ec",
    "name": "hudson hook",
    "tags": "{\"type\":\"webhook\",\"owner\":\"coyote@acme.corp\"}",
    "channelData": {
        "url": "https://www.acme.corp/webhookurl/",
        "authenticate": false
    },
    "state": "ACTIVE",
    "channelMeta": {
        "id": "webhook",
        "userId": "0",
        "templateId": "1",
        "type": "WEBHOOK"
    },
    "timezone": "UTC"
}
```

> Response Example (creating a slack channel):

```json
{
    "id": "360f3c20-122d-4617-b0a2-fa766470360e",
    "name": "noc slack channel",
    "tags": "{\"type\":\"slack\",\"owner\":\"coyote@acme.corp\"}",
    "channelData": {
        "url": "https://acme.slack.com/webhookurl",
        "channel": "#nocnoc"
    },
    "state": "ACTIVE",
    "channelMeta": {
        "id": "slack",
        "userId": "0",
        "templateId": "2",
        "type": "SLACK"
    },
    "timezone": "UTC"
}
```

The response is a list of channels

Field | Type | Description / Example
-|-|-
id | String ($uuid) | channel id. 
type | String | Channel type. See possible values in [channel list] (#channel-list)
name | String | Channel name.
tags | String | Tags automatically assigned to the channel (based on the API token used when creating it)
channelData | JSON | Channel parameters as they appear in Anodot
state | String| Channel state (Should be "Active" when just created)
channelMeta | JSON | Channel metadata as stored in Anodot
timezone | String | the time zone definition of the channel.

#### Channel List

Type | Description
-----| -----------
email | Create an email distribution list and send alerts to its participants
webhook | Send the alert as a JSON object to a webhook server
slack | Post the alert as a slack message on a slack channel
pagerduty | Create an event in a PagerDuty service
jira | Open the alert as ticket in a JIRA project
opsgenie | Create an OpsGenie alert
msteams | Post the alert as an MS Teams message on an MS Teams channel
tamtam | Send the alert as a TamTam message
slackapp | Use Anodot's Slack App to handle the alerts
mattermost | Create a mattermost message
sns | Send alerts as AWS sns messages
telegram | Send alerts as telegram messages
servicenow | Send alerts to ServiceNow
salesforce | Create a salesforce case from an alert

