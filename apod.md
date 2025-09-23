# NASA Astronomy Picture of the Day (APOD) - Quick Reference
## What this API does
Returns a daily astronomy image with title and explanation
## Base Endpoint
`GET https://api.nasa.gov/planetary/apod`
## Parameters
- `api_key` (required) – Use `DEMO_KEY` or your personal key.
- `date` (optional) – Format `YYYY-MM-DD`.
## Example Request
GET https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY&date=2020-01-01

## Example JSON Response (trimmed)
```json
{
  "date": "2020-01-01",
  "title": "Quadrantid Meteor and Aurora",
  "url": "https://apod.nasa.gov/apod/image/2001/Quadrantids_Aurora.jpg",
  "explanation": "Short description...",
  "media_type": "image"
}
```
## Field Guide
- `date`: Which day the image belongs to.
- `title`: Human-friendly title of the picture.
- `url`: Direct link to image or video.
- `explanation`: Plain-English description.
- `media_type`: image or video.

## Errors
- **403** – Bad or missing `api_key`.
- **400** – Wrong date format.
- **429** – Too many requests (rate limit).
