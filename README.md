# PerSQL on Railway — starter

A minimal Node web service backed by an isolated [PerSQL](https://persql.com)
SQLite database. Each page load writes a row and reads the count — that
round trip is the whole integration.

## Deploy

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/deploy/persql?referralCode=yRh8j2&utm_medium=integration&utm_source=readme&utm_campaign=persql-railway)

1. **Get a database.** Visit **[railway.persql.com/connect](https://railway.persql.com/connect)**,
   sign in, and provision a database. You'll get three values:

   | Var | What |
   |---|---|
   | `PERSQL_API_URL` | `https://api.persql.com` |
   | `PERSQL_DATABASE` | `<namespace>/<db-slug>` |
   | `PERSQL_TOKEN` | a token scoped to that one database |

2. **Deploy.** Click the button above. Railway prompts for `PERSQL_DATABASE`
   and `PERSQL_TOKEN` — paste the values from step 1 (`PERSQL_API_URL` is
   pre-filled). Prefer the dashboard? **New Project → Deploy from GitHub repo →
   `persql/railway-starter`** works too; `railway.json` pins the build, start
   command, and healthcheck.

3. **Open the service URL.** The visit counter increments on each refresh.

## How it connects

```js
import { PerSQL } from "@persql/sdk";

const db = new PerSQL({
  token: process.env.PERSQL_TOKEN,
  baseURL: process.env.PERSQL_API_URL, // defaults to https://api.persql.com
}).database(process.env.PERSQL_DATABASE); // "namespace/db-slug"

await db.query("SELECT count(*) AS n FROM visits");
```

See [`server.js`](./server.js) for the full example and
[docs.persql.com/integrations/railway](https://docs.persql.com/integrations/railway)
for the integration guide, including per-PR preview databases.

## Billing

PerSQL is usage-metered against a prepaid balance — no per-database fee.
A schema-only database that you don't query costs effectively nothing;
you pay for the requests, rows, and storage you actually use.
