# Competitive Audience API Documentation

**Endpoint:** `https://o9glv549pc.execute-api.eu-north-1.amazonaws.com/dev`

**Method:** `GET`

## Description

Provides aggregated data from the `competitive_audience` MongoDB collection, including total revenue, total conversions, and an overview by customer class.

## Query Parameters

All parameters are optional and can be combined to filter the data.

- **start_date** (`YYYY-MM-DD`): Start of the date range to filter `email_sent_date`.
- **end_date** (`YYYY-MM-DD`): End of the date range to filter `email_sent_date`.
- **class_type** (`string`): Filter data by customer class (e.g., `Class A`).
- **matchback_rule** (`string`): Filter data by specific matchback rule (e.g., `formula151`).
- **specific_date** (`YYYY-MM-DD`): Filter data for a specific date based on `email_sent_date`.

## Response

### Success (200 OK)

```json
{
    "total_revenue": 991.46,
    "total_conversions": 9,
    "overview_by_class": [
        {
            "total_emails": 5065,
            "total_conversions": 3,
            "conversion_rate": 0.0005923000987166831,
            "sum_converted_amount": 249.85,
            "class": "B",
            "matched_rules": [
                "formula1",
                "formula24"
            ]
        },
        {
            "total_emails": 5201,
            "total_conversions": 3,
            "conversion_rate": 0.0005768121515093252,
            "sum_converted_amount": 322.13,
            "class": "C",
            "matched_rules": [
                "formula1",
                "formula3"
            ]
        },
        {
            "total_emails": 1491,
            "total_conversions": 1,
            "conversion_rate": 0.0006706908115358819,
            "sum_converted_amount": 176.79,
            "class": "A",
            "matched_rules": [
                "formula1"
            ]
        },
        {
            "total_emails": 4625,
            "total_conversions": 0,
            "conversion_rate": 0.0,
            "sum_converted_amount": 0,
            "class": "D",
            "matched_rules": []
        },
        {
            "total_emails": 11858,
            "total_conversions": 2,
            "conversion_rate": 0.00016866250632484398,
            "sum_converted_amount": 242.69,
            "class": "E",
            "matched_rules": [
                "formula151"
            ]
        }
    ]
}
```

### Error Responses

- **400 Bad Request**

  - **Invalid `start_date` or `end_date` format:**

    ```json
    {
        "error": "Invalid start_date format. Expected YYYY-MM-DD."
    }
    ```

  - **Invalid `specific_date` format:**

    ```json
    {
        "error": "Invalid specific_date format. Expected YYYY-MM-DD."
    }
    ```

- **500 Internal Server Error**

    ```json
    {
        "error": "Internal Server Error"
    }
    ```

## Examples

### 1. Get Overview Without Filters

**Request:**

```
GET https://o9glv549pc.execute-api.eu-north-1.amazonaws.com/dev
```

**Response:**

As shown in the "Success" section above.

### 2. Get Data with Date Range and Class Type

**Request:**

```
GET https://o9glv549pc.execute-api.eu-north-1.amazonaws.com/dev?start_date=2024-10-01&end_date=2024-10-31&class_type=A
```

**Response:**

```json
{
    "total_revenue": 500.00,
    "total_conversions": 5,
    "overview_by_class": [
        {
            "total_emails": 1000,
            "total_conversions": 5,
            "conversion_rate": 0.005,
            "sum_converted_amount": 500.00,
            "class": "A",
            "matched_rules": [
                "formula1",
                "formula2"
            ]
        }
    ]
}
```

### 3. Get Data for a Specific Matchback Rule

**Request:**

```
GET https://o9glv549pc.execute-api.eu-north-1.amazonaws.com/dev?matchback_rule=formula151
```

**Response:**

```json
{
    "total_revenue": 242.69,
    "total_conversions": 2,
    "overview_by_class": [
        {
            "total_conversions": 2,
            "sum_converted_amount": 242.69,
            "class": "E",
            "matched_rules": [
                "formula151"
            ]
        }
    ]
}
```

## Notes

- All date parameters (`start_date`, `end_date`, `specific_date`) must be in `YYYY-MM-DD` format.
- If both `start_date` and `end_date` are provided, the API will filter records where `email_sent_date` is between `start_date` and `end_date` inclusive.
- Providing `specific_date` will override the `start_date` and `end_date` filters to retrieve records for that specific day.
- The `overview_by_class` provides aggregated data for each customer class, including the sum of converted amounts and the matchback rules used.
