# Playlist Forge

Pull every liked song from your Spotify, let Claude cluster them into genre-grouped playlists, and create them all in your account in one shot.

## Deploy to Vercel (~3 min)

```bash
cd playlist-forge
vercel --prod
```

When prompted, set up the project (defaults are fine — it's a static site with one serverless function).

After first deploy, set your Anthropic API key:

```bash
vercel env add ANTHROPIC_API_KEY
# Paste your key when prompted, select "Production"
vercel --prod   # redeploy to pick up the env var
```

Get a key at: https://console.anthropic.com/settings/keys

## Configure Spotify

1. Go to https://developer.spotify.com/dashboard
2. Create app → name it anything
3. Add Redirect URI: `https://YOUR-VERCEL-URL.vercel.app/` (the exact URL from Vercel)
4. Select "Web API" under APIs used
5. Save, then copy your Client ID

## Use it

Open the deployed URL, paste your Client ID, hit Connect. Done.

## Architecture

- `index.html` — single-page React app (CDN React + Tailwind, no build step)
- `api/claude.js` — Vercel serverless function proxying to Claude (keeps API key off the client)
- Spotify OAuth runs client-side via PKCE flow

## Notes

- Your Spotify Client ID is stored in `localStorage` so you don't re-enter it
- Spotify access tokens last 1 hour — re-auth if you need to run again later
- Large libraries (1000+ tracks) work fine; the AI step takes 30-90 seconds
- Playlists are created as private by default
