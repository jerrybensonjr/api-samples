# REST Countries API Documentation

## Table of Contents

- [Overview](#overview)
- [Quick Start](#quick-start)
- [Base Endpoint](#base-endpoint)
- [Authentication](#authentication)
- [Endpoints](#endpoints)
- [Parameters](#parameters)
- [Response Format](#response-format)
- [Error Handling](#error-handling)
- [Field Reference](#field-reference)
- [Usage Tips & Best Practices](#usage-tips--best-practices)
- [Common Use Cases](#common-use-cases)
- [Troubleshooting](#troubleshooting)
- [SDK & Libraries](#sdk--libraries)

## Overview

The REST Countries API provides comprehensive country data including names, codes, population, languages, currencies, flags, time zones, and geographic information. Perfect for forms, analytics dashboards, and educational applications.

**Key Features:**

- No API key required - start using immediately
- Full CORS support for browser applications
- Data updates are infrequent; consider client-side caching
- Multiple search methods (name, code, region)
- Flexible field filtering to minimize payload size

## Quick Start

Get started in 30 seconds:

```bash
# Get all countries
curl "https://restcountries.com/v3.1/all"

# Search by country name
curl "https://restcountries.com/v3.1/name/france"

# Get specific fields only
curl "https://restcountries.com/v3.1/name/japan?fields=name,capital,currencies"
```

## Base Endpoint

```
https://restcountries.com/v3.1/
```

**Response Format:** JSON  
**Content-Type:** `application/json`  
**CORS:** Enabled for all origins  

## Authentication

**No authentication required.** This API is completely open and free to use. Simply make HTTP GET requests to start retrieving country data immediately.

*Why no API key?* We believe country data should be universally accessible for developers building educational tools, forms, and applications that benefit users worldwide.

---

## What's New in v2.0

**Recent Updates:**

- Added Quick Start guide and common use cases
- Fixed response format specifications for all endpoints  
- Expanded error handling and troubleshooting sections
- Added community SDK recommendations and performance tips
- Comprehensive field reference and best practices

---

## Endpoints

| Endpoint | Description | Returns |
|----------|-------------|---------|
| [`GET /all`](#get-all-countries) | All countries | Array |
| [`GET /name/{name}`](#search-by-name) | Search by country name | Array |
| [`GET /alpha/{code}`](#search-by-code) | Search by ISO country code | Array (length 1) |
| [`GET /alpha?codes={codes}`](#search-by-multiple-codes) | Search by multiple codes | Array |
| [`GET /region/{region}`](#search-by-region) | Search by region | Array |

## Parameters

*Note: Some parameters are path segments for specific endpoints (e.g., `/name/{name}`, `/region/{region}`), while others are query parameters (e.g., `fields`).*

| Parameter | Type | Required | Description | Example |
|-----------|------|----------|-------------|---------|
| `name` | string | No | Country name (partial matches allowed) | `france`, `united states` |
| `fullText` | boolean | No | If true, only exact matches returned | `true`, `false` |
| `region` | string | No | Filter by region | `europe`, `asia`, `americas`, `africa`, `oceania` |
| `codes` | string | No | Comma- or semicolon-separated ISO codes | `col;no;ee` or `col,no,ee` |
| `fields` | string | No | Comma-separated list of fields to return | `name,capital,currencies` |

---

## Search by Name

**GET** `/name/{name}`

Search for countries by name with fuzzy matching support.

**Parameters:**

- `name` (path) - Country name or partial string
- `fullText` (query, optional) - Set to `true` for exact matches only
- `fields` (query, optional) - Limit returned fields

**Examples:**

```bash
# Fuzzy search
curl "https://restcountries.com/v3.1/name/franc"

# Exact match only
curl "https://restcountries.com/v3.1/name/france?fullText=true"

# Limited fields
curl "https://restcountries.com/v3.1/name/japan?fields=name,capital,currencies"
```

**Success Response (200 OK):**

```json
[
  {
    "name": {
      "common": "France",
      "official": "French Republic"
    },
    "capital": ["Paris"],
    "currencies": {
      "EUR": {
        "name": "Euro",
        "symbol": "€"
      }
    }
  }
]
```

---

## Search by Code

**GET** `/alpha/{code}`

Lookup a single country by its ISO 3166-1 alpha-2 or alpha-3 code.

**Parameters:**

- `code` (path) - 2-letter (FR) or 3-letter (FRA) ISO code
- `fields` (query, optional) - Limit returned fields

**Examples:**

```bash
# Using alpha-2 code
curl "https://restcountries.com/v3.1/alpha/fr"

# Using alpha-3 code  
curl "https://restcountries.com/v3.1/alpha/FRA"

# With field filtering
curl "https://restcountries.com/v3.1/alpha/CAN?fields=name,capital,region,flags"
```

**Success Response (200 OK):**

```json
[
  {
    "name": {
      "common": "France", 
      "official": "French Republic"
    },
    "capital": ["Paris"],
    "region": "Europe",
    "flags": {
      "png": "https://flagcdn.com/w320/fr.png",
      "svg": "https://flagcdn.com/fr.svg",
      "alt": "The flag of France is composed of three equal vertical bands of blue, white and red."
    }
  }
]
```

---

## Search by Multiple Codes

**GET** `/alpha?codes={codes}`

Lookup multiple countries by their ISO codes in a single request.

**Parameters:**

- `codes` (query) - Comma- or semicolon-separated list of ISO codes
- `fields` (query, optional) - Limit returned fields

**Examples:**

```bash
# Multiple countries
curl "https://restcountries.com/v3.1/alpha?codes=col;no;ee"

# With comma separator  
curl "https://restcountries.com/v3.1/alpha?codes=fr,de,it"
```

---

## Search by Region

**GET** `/region/{region}`

Get all countries within a specific region.

**Available Regions:** `africa`, `americas`, `asia`, `europe`, `oceania`

**Examples:**

```bash
# All European countries
curl "https://restcountries.com/v3.1/region/europe"

# Asian countries with limited fields
curl "https://restcountries.com/v3.1/region/asia?fields=name,capital,population"
```

---

## Get All Countries

**GET** `/all`

Retrieve data for all countries. **Warning:** Large response (~1.5MB). Use field filtering in production.

**Examples:**

```bash
# All data (large response)
curl "https://restcountries.com/v3.1/all"

# Essential fields only
curl "https://restcountries.com/v3.1/all?fields=name,cca2,region,capital"
```

## Response Format

**Content-Type:** `application/json`

**Response Types:**

- All endpoints return **arrays** of country objects (including `/alpha/{code}`, which returns an array of length 1)
- Empty results return empty array `[]`

**Typical Response Headers:**

```
Content-Type: application/json; charset=utf-8
Access-Control-Allow-Origin: *
```

## Error Handling

| Status Code | Description | Response |
|-------------|-------------|----------|
| 200 | Success | Country data |
| 400 | Bad Request | Invalid parameters |
| 404 | Not Found | No countries match |
| 429 | Too Many Requests | Rate limit exceeded |
| 500 | Server Error | Service unavailable |

**Error Response Examples:**

```json
// 404 - No countries found
{
  "status": 404,
  "message": "Not Found"
}

// 400 - Invalid region
{
  "status": 400, 
  "message": "Bad Request"
}
```

## Field Reference

### Essential Fields

- `name.common` - Common country name
- `name.official` - Official country name  
- `cca2` - ISO 3166-1 alpha-2 code
- `cca3` - ISO 3166-1 alpha-3 code
- `capital[]` - Array of capital cities
- `region` - Geographic region
- `subregion` - Geographic subregion

### Geographic & Demographic  

- `population` - Population count
- `area` - Area in km²
- `latlng[]` - [latitude, longitude]
- `landlocked` - Boolean
- `borders[]` - Array of neighboring country codes

### Cultural & Political

- `languages.{code}` - Languages object
- `currencies.{code}.name` - Currency name
- `currencies.{code}.symbol` - Currency symbol
- `independent` - Independence status
- `unMember` - UN membership status

### Technical

- `tld[]` - Top-level domains
- `timezones[]` - Time zones
- `idd.root` - International dialing root
- `idd.suffixes[]` - Dialing suffixes

### Visual

- `flags.png` - Flag PNG URL
- `flags.svg` - Flag SVG URL  
- `flags.alt` - Flag description
- `coatOfArms.png` - Coat of arms PNG
- `maps.googleMaps` - Google Maps URL
- `maps.openStreetMaps` - OpenStreetMap URL

## Usage Tips & Best Practices

### Performance

- **Always use field filtering** in production: `?fields=name,capital,currencies`
- **Cache responses** - country data changes infrequently
- **Prefer specific endpoints** over `/all` when possible

### Rate Limiting

- No published rate limits, but implement client-side throttling
- Consider caching responses for 24+ hours
- Use bulk endpoints (`/alpha?codes=`) for multiple countries

### Search Best Practices

- Use `fullText=true` to avoid unexpected fuzzy matches
- URL-encode country names with spaces: `united%20states`
- Handle diacritics: `côte d'ivoire` → `c%C3%B4te%20d'ivoire`

### Data Handling

- All endpoints return arrays (including `/alpha/{code}` which returns array of length 1)
- Handle missing optional fields gracefully
- Some countries have multiple capitals (stored as arrays)

## Common Use Cases

### Country Selection Dropdown

```javascript
// Get minimal data for dropdowns
fetch('https://restcountries.com/v3.1/all?fields=name,cca2')
  .then(response => response.json())
  .then(countries => {
    countries.forEach(country => {
      console.log(`${country.name.common} (${country.cca2})`);
    });
  });
```

### Currency Display

```javascript
// Get currency information
fetch('https://restcountries.com/v3.1/name/france?fields=currencies')
  .then(response => response.json())
  .then(data => {
    const currencies = data[0].currencies; // Note: data is array
    Object.values(currencies).forEach(curr => {
      console.log(`${curr.name} (${curr.symbol})`);
    });
  });
```

### Regional Analysis

```javascript
// Compare European countries by population
fetch('https://restcountries.com/v3.1/region/europe?fields=name,population')
  .then(response => response.json())
  .then(countries => {
    countries.sort((a, b) => b.population - a.population);
    console.log('Largest European countries:', countries.slice(0, 5));
  });
```

## Troubleshooting

### Common Issues

**Q: Getting empty array `[]` for valid country name**  
A: Try without `fullText=true`, or check spelling. Some countries have multiple accepted names.

**Q: Special characters in country names not working**  
A: URL-encode special characters: `Côte d'Ivoire` becomes `C%C3%B4te%20d%27Ivoire`

**Q: Response is too large**  
A: Use field filtering: `?fields=name,capital,region` to reduce payload size

**Q: Getting different response structure**  
A: All endpoints return arrays. `/alpha/{code}` returns an array with one country, not a bare object.

### Data Inconsistencies

- Some territories may lack certain fields (population, currencies)
- Some datasets may include territories or special cases alongside current nations  
- Border data reflects current political boundaries
- Currency information reflects current official currencies

## SDK & Libraries

### Community Libraries

**Note:** These are community-maintained packages, not official SDK releases.

**JavaScript/Node.js:**

- [rest-countries](https://www.npmjs.com/package/rest-countries) - Typed wrapper
- [world-countries](https://www.npmjs.com/package/world-countries) - Static data

**Python:**

- [python-restcountries](https://pypi.org/project/python-restcountries/) - Python wrapper
- [pycountry](https://pypi.org/project/pycountry/) - ISO standard data

**PHP:**

- [league/iso3166](https://packagist.org/packages/league/iso3166) - Country data
- Native `file_get_contents()` or `curl` work well

**Mobile:**

- iOS: Use `URLSession` with `JSONDecoder`  
- Android: Retrofit with Gson/Moshi
- React Native: Built-in `fetch()` API

---

**API Version:** v3.1  
**Doc Version:** v2.0
**Last Updated:** Data is maintained and updated regularly

For support or questions about this documentation, please check my [project repository](https://github.com/jerrybensonjr/api-samples/blob/main/Rest_Countries_API.md). For the official REST Countries API source and issues, see the [REST Countries project](https://restcountries.com).
