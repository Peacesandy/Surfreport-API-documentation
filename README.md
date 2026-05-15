# Surfreport API

The Surfreport API provides real-time surfing conditions for beaches. It returns environmental data such as surf height, water temperature, wind speed, and tide levels, along with an overall recommendation indicating whether conditions are suitable for surfing.

---

# Base URL

```http
https://api.surfreport.com/v1
```

---

# Authentication

All requests must include an API key in the `Authorization` header.

```http
Authorization: Bearer YOUR_API_KEY
```

---

# Endpoint

## Get Surf Report

Returns surf conditions for a specific beach.

```http
GET /surfreport/{beachId}
```

### Example Request

```http
GET /surfreport/123?days=2&units=imperial
```

### cURL Example

```bash
curl -X GET \
  "https://api.surfreport.com/v1/surfreport/123?days=2&units=imperial" \
  -H "Authorization: Bearer YOUR_API_KEY"
```

---

# Path Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| beachId | Integer | Yes | The unique ID of the beach to retrieve surf conditions for. |

> Valid beach IDs can be retrieved from the `GET /beaches` endpoint.

---

# Query Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| days | Integer | No | Number of forecast days to return. Default is `3`. Maximum is `7`. |
| time | Integer | No | Unix timestamp in milliseconds. If provided, returns surf conditions for the specified time only. Example: `1715788800000` |
| units | String | No | Unit system for the response. Allowed values: `imperial`, `metric`. Default is `imperial`. |

---

# Response Structure

The response contains:

- Beach information
- Daily surf forecasts
- Hourly surf conditions
- Surfing recommendations

Dynamic keys are used for:
- Days of the week (for example: `monday`, `tuesday`)
- Time entries (for example: `1pm`, `2pm`)

---

# Sample Response

```json
{
  "surfreport": [
    {
      "beach": "Santa Cruz",
      "forecast": {
        "monday": {
          "1pm": {
            "tide": 5,
            "wind": 15,
            "waterTemp": 80,
            "surfHeight": 5,
            "recommendation": "Go surfing!"
          },
          "2pm": {
            "tide": -1,
            "wind": 1,
            "waterTemp": 50,
            "surfHeight": 3,
            "recommendation": "Surfing conditions are okay, not great."
          },
          "3pm": {
            "tide": -1,
            "wind": 10,
            "waterTemp": 65,
            "surfHeight": 1,
            "recommendation": "Not a good day for surfing."
          }
        }
      }
    }
  ]
}
```

---

# Response Fields

| Field | Type | Description |
|---|---|---|
| beach | String | Name of the beach associated with the provided `beachId`. |
| forecast | Object | Collection of daily surf forecasts. |
| forecast.{day} | Object | Forecast data for a specific day of the week. |
| forecast.{day}.{time} | Object | Surf conditions for a specific time. |
| tide | Integer | Tide level for the specified time. Positive values indicate incoming tide, while negative values indicate outgoing tide. |
| wind | Integer | Wind speed measured in knots. |
| waterTemp | Integer | Water temperature. Returned in Fahrenheit for `imperial` units or Celsius for `metric` units. |
| surfHeight | Integer | Height of the waves. Returned in feet for `imperial` units or centimeters for `metric` units. |
| recommendation | String | Overall surfing recommendation based on tide, wind speed, water temperature, and surf height. |

---

# Recommendation Logic

Recommendations are generated using:
- Wave height
- Wind speed
- Tide conditions
- Water temperature

Possible recommendation values include:

- `Go surfing!`
- `Surfing conditions are okay, not great.`
- `Not a good day for surfing.`

---

# HTTP Status Codes

| Status Code | Description |
|---|---|
| 200 | Request successful |
| 400 | Invalid request |
| 401 | Unauthorized request |
| 404 | Beach not found |
| 500 | Internal server error |

---

# Error Response Example

## 404 Not Found

```json
{
  "error": {
    "code": 404,
    "message": "Beach not found"
  }
}
```

---

# Notes

- The default forecast range is 3 days.
- The maximum number of forecast days supported is 7.
- All timestamps are returned in UTC.
- Wind speed is measured in knots.
- Metric responses use Celsius and centimeters.
- Imperial responses use Fahrenheit and feet.
