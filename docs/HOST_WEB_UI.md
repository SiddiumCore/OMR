# Host the CRATOR Web UI

The CRATOR Web UI is a static React/Vite/Tailwind site. It talks to the Local Agent on `http://127.0.0.1:5757`.

## Build

```bat
cd web
npm install
npm run build
```

This creates `web/dist/`.

## Hosting Options
- GitHub Pages
- Netlify
- Vercel
- Azure Static Web Apps
- AWS S3 + CloudFront

Upload the contents of `web/dist/` and serve the site over HTTPS.

## CORS Setup

Update `agent/config.py` and add your hosted UI origin to `allowed_origins`, then restart the agent.

Example:

```python
"https://yourdomain.com"
```

## Verifying
1. Start the Local Agent.
2. Confirm `http://127.0.0.1:5757/health` returns `ok`.
3. Open the hosted UI and confirm `Agent: Connected`.
4. Pick an input folder, edit the main/no-marker templates if needed, run processing, and confirm outputs.
