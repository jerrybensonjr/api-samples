# REST Countries API

## What this API does

Look up standard country data (names, codes, population, languages, currencies, flags, time zones, maps, and more). **No API key required**.

## Base Endpoint

<https://restcountries.com/v3.1/>

  General pattern:
  GET <https://restcountries.com/v3.1/{endpoint}>

  Available endpoints include:

- `/all` - All countries
- `/name/{name}` - By country name
- `/region/{region}` - By region
- `/alpha/{code}` - By ISO country code

---

## Parameters

| **Name** | **Type** | **Required** | **Example** | **Notes** |
|----------|----------|--------------|-------------|-----------|
|name|string|No|france|Search by country name (partial matches allowed).|
|fullText|boolean|No|true|If true, only exact matches are returned|
|region|string|No|europe|Filter by region (e.g. africa, americas, asia, europe, oceania).|
|codes|string|No|col;no;ee|Search by comma- or semicolon-separated list of ISO country codes.|
|fields|string|No|name, capital, currencies|Specify which fields to return. Useful to reduce payload size.|

---

### Resource: Search by Country Name

**GET** /name/{name}

**Path Parameters:**

- **name** - Country name or partial string. Example: Canada, Congo

**Query Parameters:**

- **fullText** *(boolean, optional)* - Exact match only.
- **fields** *(string, optional)* - Comma- or semicolon-separated list to trim payload.

**Example Requests:**

```http
GET https://restcountries.com/v3.1/name/france
GET https://restcountries.com/v3.1/name/united%20states?fullText=true
GET https://restcountries.com/v3.1/name/japan?fields=name,capital,currencies
```

---

### Resource: Search by Alpha Code

**GET** /alpha/{code}

**Path Parameters:**

- **code** - ISO 3166 code (2- or 3-letter). Example: ca, CAN

**Query Parameters:**

- **fields** *(string, optional)*

**Example Requests:**

```http
GET https://restcountries.com/v3.1/alpha/CAN
GET https://restcountries.com/v3.1/alpha/CAN?fields=name,capital,region,cca2,cca3,flags
```

---

### Resource: All Countries

**GET** /all

**Query Parameters:**

- **fields** *(string, optional)*

**Example Requests:**

```http
GET https://restcountries.com/v3.1/all
GET https://restcountries.com/v3.1/all?fields=name,cca2,region
```

### Resource: Search by Region

**GET** /region/{region}

**Path Parameters:**

- **region** - Region string. Examples: Europe, Asia, Americas.

**Query Parameters:**

- **fields** *(string, optional)*

**Example Requests:**

```http
GET https://restcountries.com/v3.1/region/europe
```

### Resource: Search by Multiple Codes

**GET** /alpha?codes={codes}

**Query Parameters:**

- **codes** - comma- or semicolon-separated list of codes.

**Example Request:**

```http
GET https://restcountries.com/v3.1/alpha?codes=col;no;ee
```

## Example Response (trimmed)

```json
[
  {
    "name": { "common": "France", "official": "French Republic" },
    "capital": ["Paris"],
    "region": "Europe",
    "subregion": "Western Europe",
    "languages": { "fra": "French" },
    "population": 67391582,
    "area": 551695,
    "flags": {
      "png": "https://flagcdn.com/w320/fr.png",
      "svg": "https://flagcdn.com/fr.svg"
    },
    "currencies": {
      "EUR": { "name": "Euro", "symbol": "€" }
    }
  }
]
```

## Field Guide (Common Fields)

- name.common, name.official
- cca2, cca3
- capital[]
- region, subregion
- population, area
- languages.{code}
- currencies.{code}.name, .symbol
- timezones[]
- idd.root, idd.suffixes[]
- latlng[]
- tld[]
- maps.googleMaps, maps.openStreetMaps
- borders[]
- flags.png, flags.svg, flags.alt
- independent, unMember

## Errors

- 404 Not Found — No match.
- 400 Bad Request — Malformed request.
- 429 Too Many Requests — Possible under heavy use. Not always documented but may occur.
- 5xx Server Error — Service issue.

## Notes & Constraints

- Authentication: None.
- CORS: Supported.
- Rate Limits: Not officially published.
- Response Shape: /name and /all return arrays; /alpha/{code} returns a single object, while /alpha?codes= returns an array.

## Usage Tips

- Use fullText=true to avoid fuzzy matches.
- Prefer /alpha/{code} when you have a code.
- Always filter with fields in production.
- Expect arrays in most responses; note that /alpha/{code} returns a single object unless using ?codes=.
- Cache results (data is stable).

## Edge Cases

- Ambiguous names (/name/congo → 2 results).
- Multi-capital countries (capital[]).
- Territories with missing fields.
- Diacritics and apostrophes require URL encoding.