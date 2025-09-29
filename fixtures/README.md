# Fixtures for Mock Testing

This folder contains **sample JSON responses** used in the OpenWeather API documentation project.  

The goal is to let others **test or demo API calls without requiring a paid OpenWeather account**. These files can be imported into tools like **Postman** or loaded locally in code (Node.js, Python, etc.) to simulate real API responses.

---

## Files Included

- `openweather-weather.json` → Example success response (200)  
- `openweather-401.json` → Invalid API key (401 Unauthorized)  
- `openweather-404.json` → City not found (404 Not Found)  
- `openweather-429.json` → Too many requests (429 Rate Limit Exceeded)  
- `openweather-500.json` → Internal server error (500)  

---

## How to Use

- **In Postman**: attach these as example responses to a request, then serve them via a Postman Mock Server.  
- **In local code**: load them from the `fixtures/` folder to simulate API responses.  

Example (Node.js):

```js
const fs = require("fs");

const success = JSON.parse(fs.readFileSync("fixtures/openweather-weather.json", "utf8"));
console.log(success.main.temp, success.weather[0].description);
```

---

**Note:** These are **mock data files** for portfolio/demo purposes. They are not live API responses and will not change over time.
