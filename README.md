## Surfreport API

The Surfreport API provides information about surfing conditions for specific beaches. It returns key environmental data such as surf height, water temperature, wind speed, and tide levels. It also includes an overall recommendation on whether conditions are suitable for surfing.

## Endpoints

### GET `/surfreport/{beachId}`

Returns surf conditions for a specific beach based on the provided `beachId`.

**Path Parameter:**
- `beachId` → The ID of the beach you want surf conditions for.

## Parameters

### Path Parameters

| Parameter | Description |
|----------|-------------|
| `beachId` | The ID of the beach you want to retrieve surf conditions for. Valid beach IDs are available from sampleurl.com. |

---

### Query Parameters

| Parameter | Required | Description | Type |
|-----------|----------|-------------|------|
| `days` | Optional | Number of days to include in the response. Default is 3. | Integer |
| `time` | Optional | If provided, returns surf conditions for the specified time only. Value should be in Unix time (milliseconds since 1970 UTC). | Integer |

## Sample Response

The following is a sample response from the `/surfreport/{beachId}` endpoint:

```json
{
  "surfreport": [
    {
      "beach": "Santa Cruz",
      "monday": {
        "1pm": {
          "tide": 5,
          "wind": 15,
          "watertemp": 80,
          "surfheight": 5,
          "recommendation": "Go surfing!"
        },
        "2pm": {
          "tide": -1,
          "wind": 1,
          "watertemp": 50,
          "surfheight": 3,
          "recommendation": "Surfing conditions are okay, not great."
        },
        "3pm": {
          "tide": -1,
          "wind": 10,
          "watertemp": 65,
          "surfheight": 1,
          "recommendation": "Not a good day for surfing."
        }
      }
    }
  ]
}
```

##  Response Definitions

| Response Item | Description | Data Type |
|---------------|-------------|------------|
| `beach` | The beach selected based on the provided `beachId`. | String |
| `{day}` | The day of the week returned in the response. A maximum of 3 days is returned. | Object |
| `{time}` | The specific time for the surf conditions. Included only when a time parameter is provided. | String |
| `{day}/{time}/tide` | The tide level at the beach for a specific day and time. Positive values indicate incoming tide, while negative values indicate outgoing tide. | Integer |
| `{day}/{time}/wind` | Wind speed measured in knots (nautical miles per hour). | Integer |
| `{day}/{time}/watertemp` | Water temperature returned in Fahrenheit or Celsius depending on the selected units. | Integer |
| `{day}/{time}/surfheight` | Height of the waves returned in feet or centimeters depending on the selected units. | Integer |
| `{day}/{time}/recommendation` | Overall surfing recommendation based on tide, wind, water temperature, and surf height. | String |


