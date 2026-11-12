Vercel deployment notes

The client uses `VITE_API_URL` when it is set. Otherwise, requests use `/api/v1`; the Vercel rewrite in `client/vercel.json` proxies those requests to the Render API and serves the React app for direct visits to nested routes.

## Vercel

Set the Vercel project Root Directory to `client`, then build with `npm run build` and publish `dist`. The rewrite configuration in this directory supports refreshing or directly opening routes such as `/orchards/:slug`.

To use a different API host, set `VITE_API_URL` in the Vercel project environment variables to its API base (for example, `https://api.example.com/api/v1`) and redeploy. This value is used in production and development.

## Troubleshooting

- A page route returning 404 on refresh usually means the deployment is not using this directory's `vercel.json`.
- API requests returning 502/503 indicate the upstream backend is unavailable. Check the Render service's deployment logs, health at `/health`, and its MongoDB/environment configuration.

Notes:
- If you use cookies for authentication (refresh tokens), ensure backend CORS allows the frontend origin and that cookies use `SameSite=None` and `Secure` in production.
- Backend env vars to set:
- `CLIENT_URL=https://orchardslease.vercel.app` (or your deployed frontend origin)
- `NODE_ENV=production`
- Optionally `COOKIE_DOMAIN` (e.g. `.example.com`) if sharing cookies across subdomains.