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
