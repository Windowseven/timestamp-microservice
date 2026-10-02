# Timestamp Microservice

A RESTful microservice that converts dates to Unix timestamps and back, plus a small
static landing page documenting the API.

Built for the freeCodeCamp course *Build a Timestamp Microservice*
(Back-End Development and APIs certification).

## Setup

```bash
npm install
npm start
```

The server listens on port `8000`. Restart it after code changes.

## Endpoints

| Method | Path | Response |
| --- | --- | --- |
| GET | `/api` | The current time |
| GET | `/api/:date` | The parsed date |
| GET | `/` | Static landing page (`views/index.html`) |

```bash
curl localhost:8000/api/2016-12-25
# {"unix":1482624000000,"utc":"Sun, 25 Dec 2016 00:00:00 GMT"}

curl localhost:8000/api/1451001600000
# {"unix":1451001600000,"utc":"Fri, 25 Dec 2015 00:00:00 GMT"}

curl localhost:8000/api/this-is-not-a-date
# {"error":"Invalid Date"}
```

## How dates are parsed

`parseDate` in `server.js` handles three cases:

1. **All digits** (`1451001600000`) — treated as a Unix timestamp in milliseconds.
2. **Anything else** — parsed with ` UTC` appended, so strings like `05 October 2011`
   resolve to UTC midnight rather than local midnight. Without this the result shifts by
   the machine's timezone offset.
3. **Fallback** — if appending ` UTC` makes the string unparseable (for example
   `2016-12-25T00:00:00Z`), the raw string is parsed instead.

If the result is an invalid date, the API responds with `{ "error": "Invalid Date" }`.