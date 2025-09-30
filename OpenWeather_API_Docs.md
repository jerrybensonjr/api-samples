# OpenWeather Current Weather API Documentation (Portfolio Sample)

> **Portfolio Sample:** This documentation is authored by Jerry Benson to demonstrate API documentation structure, technical accuracy, and developer-centric clarity for the OpenWeather Current Weather API.  
> **Note:** Examples are provided for learning; running real requests may require a paid plan. See **Testing Without an API Key (Mocking Options)**.

---

## Overview

The OpenWeather Current Weather API provides real-time weather data for any global location. Query by city name or geographic coordinates to retrieve temperature, humidity, wind conditions, atmospheric pressure, and weather descriptions. The API supports multiple unit systems (metric, imperial, standard) and returns data in JSON, XML, or HTML formats.

**Use Cases:**

- Weather dashboard applications
- Location-based service features
- Travel and outdoor activity planning
- IoT weather monitoring systems

---

## Endpoint

`GET https://api.openweathermap.org/data/2.5/weather`

---

## Authentication

All requests require an API key passed as the `appid` query parameter.

**Getting an API Key:**

1. Sign up for an account at [openweathermap.org](https://openweathermap.org/)
2. Navigate to API Keys section in your account
3. Generate a new key (activation may take several minutes)

**Rate Limits:**

- Limits are **per API key** and depend on plan
- Free plans may be limited or unavailable; paid tiers provide higher limits

---

## Query Parameters

| Parameter | Type   | Required | Description | Example |
|-----------|--------|----------|-------------|---------|
| `q` | string | Yes* | City name. Format: `{city}` or `{city},{country_code}`. Use ISO 3166 country codes for precision. | `Shreveport,US` or `London,GB` |
| `lat` | number | Yes* | Latitude coordinate (decimal degrees). | `32.5252` |
| `lon` | number | Yes* | Longitude coordinate (decimal degrees). | `-93.7502` |
| `appid` | string | **Yes** | Your API key from OpenWeather account. | `your_api_key_here` |
| `units` | string | No | Unit system for temperature and wind speed. See [Temperature Units](#temperature-units). Options: `standard` (Kelvin), `metric` (Celsius), `imperial` (Fahrenheit). Default: `standard`. | `imperial` |
| `mode` | string | No | Response format. Options: `json` (default), `xml`, `html`. | `json` |
| `lang` | string | No | Language for weather description. Supports 40+ languages using ISO 639-1 codes. Default: `en`. | `es` for Spanish |

**\* Location Requirement:** Must provide either `q` (city name) OR both `lat` and `lon` (coordinates). Coordinates provide more precise results.

---

## Example Requests (Sample Only)

> These are illustrative request shapes. Running them against the live API may require a paid plan.

**By City Name:**

```http
GET https://api.openweathermap.org/data/2.5/weather?q=Shreveport,US&units=imperial&appid=YOUR_API_KEY
```

**By Coordinates:**

```http
GET https://api.openweathermap.org/data/2.5/weather?lat=32.5252&lon=-93.7502&units=metric&appid=YOUR_API_KEY
```

**With Language Support:**

```http
GET https://api.openweathermap.org/data/2.5/weather?q=Paris,FR&units=metric&lang=fr&appid=YOUR_API_KEY
```

**Failed Request (Missing API Key):**

```bash
curl "https://api.openweathermap.org/data/2.5/weather?q=London,GB"
```

**Sample 401 Response:**

```json
{
  "cod": 401,
  "message": "Invalid API key. Please see https://openweathermap.org/faq#error401 for more info."
}
```

---

## Example Response (Sample Fixture)

```json
{
  "coord": {
    "lon": -93.7502,
    "lat": 32.5252
  },
  "weather": [
    {
      "id": 800,
      "main": "Clear",
      "description": "clear sky",
      "icon": "01d"
    }
  ],
  "base": "stations",
  "main": {
    "temp": 75.2,
    "feels_like": 76.1,
    "temp_min": 73.0,
    "temp_max": 77.0,
    "pressure": 1013,
    "humidity": 60
  },
  "visibility": 10000,
  "wind": {
    "speed": 5.75,
    "deg": 180,
    "gust": 8.5
  },
  "clouds": {
    "all": 0
  },
  "dt": 1695650000,
  "sys": {
    "type": 1,
    "id": 3645,
    "country": "US",
    "sunrise": 1695630000,
    "sunset": 1695672000
  },
  "timezone": -18000,
  "id": 4349697,
  "name": "Shreveport",
  "cod": 200
}
```

---

## Response Fields

### Top-Level Fields

| Field | Type | Description |
|-------|------|-------------|
| `coord` | object | Geographic coordinates. See [Coordinates Object](#coordinates-object). |
| `weather` | array | Array of weather condition objects (typically one object). See [Weather Object](#weather-object). |
| `base` | string | Internal data source parameter. |
| `main` | object | Primary weather measurements. See [Main Object](#main-object). |
| `visibility` | number | Visibility in meters (max 10000). |
| `wind` | object | Wind measurements. See [Wind Object](#wind-object). |
| `clouds` | object | Cloudiness data. See [Clouds Object](#clouds-object). |
| `dt` | number | Data calculation timestamp (UNIX UTC). See [Timestamps](#timestamps). |
| `sys` | object | System and location metadata. See [System Object](#system-object). |
| `timezone` | number | Timezone offset from UTC in seconds. See [Timestamps](#timestamps). |
| `id` | number | City ID (OpenWeather internal identifier). |
| `name` | string | City name. |
| `cod` | number | HTTP status code. |

### Coordinates Object

| Field | Type | Description |
|-------|------|-------------|
| `coord.lon` | number | Longitude (decimal degrees). |
| `coord.lat` | number | Latitude (decimal degrees). |

### Weather Object

| Field | Type | Description |
|-------|------|-------------|
| `weather[].id` | number | Weather condition ID. See [Weather Condition Codes](#weather-condition-codes). |
| `weather[].main` | string | General weather category (e.g., "Clear", "Rain", "Snow"). |
| `weather[].description` | string | Detailed weather description (e.g., "clear sky", "light rain"). |
| `weather[].icon` | string | Weather icon code. Use with `https://openweathermap.org/img/wn/{icon}@2x.png`. |

### Main Object

| Field | Type | Description |
|-------|------|-------------|
| `main.temp` | number | Current temperature. Units depend on `units` parameter. See [Temperature Units](#temperature-units). |
| `main.feels_like` | number | Perceived temperature accounting for wind chill and humidity. |
| `main.temp_min` | number | Minimum temperature currently observed (for large areas/cities). |
| `main.temp_max` | number | Maximum temperature currently observed. |
| `main.pressure` | number | Atmospheric pressure at sea level (hPa). |
| `main.humidity` | number | Humidity percentage. |

### Wind Object

| Field | Type | Description |
|-------|------|-------------|
| `wind.speed` | number | Wind speed. Units: m/s (metric), mph (imperial). See [Temperature Units](#temperature-units). |
| `wind.deg` | number | Wind direction in degrees (meteorological, 0° = North). |
| `wind.gust` | number | Wind gust speed (Optional – not always present). |

### Clouds Object

| Field | Type | Description |
|-------|------|-------------|
| `clouds.all` | number | Cloudiness percentage (0-100). |

### System Object

| Field | Type | Description |
|-------|------|-------------|
| `sys.type` | number | Internal parameter (for OpenWeather use; safe to ignore). |
| `sys.id` | number | Internal parameter (for OpenWeather use; safe to ignore). |
| `sys.country` | string | ISO 3166 country code. |
| `sys.sunrise` | number | Sunrise time (UNIX UTC). See [Timestamps](#timestamps). |
| `sys.sunset` | number | Sunset time (UNIX UTC). See [Timestamps](#timestamps). |

---

## Weather Condition Codes

Weather condition IDs map to specific weather phenomena:

| ID Range | Description |
|----------|-------------|
| 2xx | Thunderstorm |
| 3xx | Drizzle |
| 5xx | Rain |
| 6xx | Snow |
| 7xx | Atmosphere (fog, dust, sand, ash, etc.) |
| 800 | Clear sky |
| 801-804 | Clouds (varying cloudiness levels) |

**Icon Mapping:**
Use the `icon` field to display weather icons:

```http
https://openweathermap.org/img/wn/{icon}@2x.png
```

Example: `01d` → `https://openweathermap.org/img/wn/01d@2x.png`

Full condition code reference available at [OpenWeather Conditions](https://openweathermap.org/weather-conditions).

---

## Error Responses (Sample Formats)

**401 Unauthorized - Invalid API Key:**

```json
{
  "cod": 401,
  "message": "Invalid API key. Please see https://openweathermap.org/faq#error401 for more info."
}
```

**404 Not Found - City Not Found:**

```json
{
  "cod": "404",
  "message": "city not found"
}
```

**429 Too Many Requests - Rate Limit Exceeded:**

```json
{
  "cod": 429,
  "message": "Your account has exceeded the rate limit. Please wait before making additional requests."
}
```

**500 Internal Server Error - API Issues:**

```json
{
  "cod": 500,
  "message": "Internal server error"
}
```

---

## Testing Without an API Key (Mocking Options)

You can validate request/response shapes with **no paid plan** using one of the approaches below.

### Option A — Postman Mock Server (Free)

1. **Create request examples**  
   - In Postman, create a `GET` request with this exact path (no base URL):  
     `/data/2.5/weather?q=Shreveport,US&units=imperial&appid=YOUR_API_KEY`
   - Save it and attach example responses by pasting in the provided fixture files.

2. **Attach fixture files as examples**
   - Success → `fixtures/openweather-weather.json`  
   - Invalid API Key → `fixtures/openweather-401.json`  
   - City Not Found → `fixtures/openweather-404.json`  
   - Rate Limit Exceeded → `fixtures/openweather-429.json`  
   - Internal Server Error → `fixtures/openweather-500.json`

3. **Create a Mock Server**  
   - In Postman: **New → Mock Server → Select the collection** containing your examples.  
   - Use the generated mock server URL in place of the live API base.

4. **Call your mock**  
    - Example:  

     ```http
     https://<YOUR_MOCK_SUBDOMAIN>.mock.pstmn.io/data/2.5/weather?q=Shreveport,US&units=imperial&appid=YOUR_API_KEY
     ```

> You’ll receive the example JSON back depending on which fixture you’ve attached.

### Option B — Local JSON Fixture (No Network)

1. Save the fixture files in a `fixtures/` folder in your repo:
   - `fixtures/openweather-weather.json`
   - `fixtures/openweather-401.json`
   - `fixtures/openweather-404.json`
   - `fixtures/openweather-429.json`
   - `fixtures/openweather-500.json`
  
2. Use them in your code to simulate API responses without hitting the live API.

**Node.js example:**

```js
const fs = require("fs");

// Success response
const success = JSON.parse(fs.readFileSync("fixtures/openweather-weather.json", "utf8"));
console.log(success.main.temp, success.weather[0].description);

// Error response (404)
const notFound = JSON.parse(fs.readFileSync("fixtures/openweather-404.json", "utf8"));
console.log(notFound);
```

> Using local fixture files is ideal for portfolio demos because it provides deterministic, repeatable responses without requiring API calls.

---

## Notes

### City Name Format

- Use ISO 3166 country codes for accuracy: `London,GB` not `London,UK`
- For US cities, include state code: `Springfield,MO,US`
- City names with spaces work without URL encoding: `New York,US`
- **City Aliases**: Many cities share names (e.g., Paris, TX vs Paris, FR). If multiple cities match your query, the API returns the first match, which may not be the expected location. Use coordinates or include country/state codes for precision.

### Coordinate Precision

- Latitude: -90 to 90 (decimal degrees)
- Longitude: -180 to 180 (decimal degrees)
- Use at least 4 decimal places for accuracy (~11 meters)

### Timestamps

- All timestamps (`dt`, `sunrise`, `sunset`) are UNIX UTC
- Convert to local time using the `timezone` offset field
- Example: `local_time = dt + timezone`

### Temperature Units

| Parameter Value | Temperature | Wind Speed |
|-----------------|-------------|------------|
| `standard` (default) | Kelvin | m/s |
| `metric` | Celsius | m/s |
| `imperial` | Fahrenheit | mph |

### Data Freshness

- Weather data updates approximately every 10 minutes
- Historical data not available via this endpoint (use History API)

### Optional Response Fields

**Optional – not always present.** Fields like `wind.gust`, `rain`, `snow`, and others may be absent depending on current conditions. Always implement null/undefined checks when parsing the response.

### City ID Alternative

- You can use `id` parameter instead of `q` for exact city matching
- Download city list: [city.list.json.gz](http://bulk.openweathermap.org/sample/)

---

## Quick Start (Mock-Friendly)

> Replace the live base URL with your **Postman Mock base URL** if you’re not using a paid key.

**City (imperial) — mock call shape:**

```bash
curl "https://<YOUR_MOCK_SUBDOMAIN>.mock.pstmn.io/data/2.5/weather?q=YourCity,US&units=imperial&appid=YOUR_API_KEY"
```

**Coords (metric) — mock call shape:**

```bash
curl "https://<YOUR_MOCK_SUBDOMAIN>.mock.pstmn.io/data/2.5/weather?lat=32.5252&lon=-93.7502&units=metric&appid=YOUR_API_KEY"
```

**Spanish descriptions (imperial) — mock call shape:**

```bash
curl "https://<YOUR_MOCK_SUBDOMAIN>.mock.pstmn.io/data/2.5/weather?q=Madrid,ES&units=imperial&lang=es&appid=YOUR_API_KEY"
```

---

**API Version:** 2.5  
**Doc Version:** 1.0 (Portfolio Sample – Mocking Enabled)  
**Maintainer:** Jerry Benson  
**Last Updated:** September 2025

For support or questions about this documentation, please check my [project repository](https://github.com/jerrybensonjr/api-samples). For the official OpenWeather API documentation and technical support, visit [OpenWeather API Docs](https://openweathermap.org/current).
