# Beatbox Studio (Song-website)

A lightweight music website for browsing and playing songs locally or on a small server. This repository contains the frontend HTML pages, a minimal Express server, and deployment helpers for PM2 and Nginx.

**Project Summary:**
- **Purpose:** Simple music browsing and playback demo used for small-scale personal hosting or learning web deployment.
- **Stack:** Static HTML/CSS frontend, Node.js + Express backend.

**Features:**
- Static pages for main listing, user profile, history, and downloads.
- Lightweight JSON-backed data store (`data/store.json`).
- Simple REST endpoints served by `server.js`.
- Deployment helpers: `ecosystem.config.js` for PM2 and an example Nginx config in `deploy/`.

**Quick Start (local)**
1. Install Node.js (v16+ recommended) and npm.
2. Install dependencies:

```bash
npm install
```

3. Run the app:

```bash
npm start
# or for development with auto-reload:
npm run dev
```

4. Open your browser to http://localhost:3000 (or the port shown in the server output). You can open the static pages directly from the project root as well, e.g. `Main.html`.

**Project Structure**
- `Main.html` — main landing / app page
- `profile.html` — demo user profile view
- `history.html` — playback history page
- `download.html` — download interface
- `server.js` — Express server entrypoint
- `package.json` — npm metadata and scripts
- `data/store.json` — JSON datastore used by the server
- `songs/` — directory containing song assets
- `deploy/` — example deploy configs (`nginx-beatbox.conf`)
- `scripts/start-ngrok.sh` — helper to expose local server via ngrok

**Deployment**
- PM2: use the provided `ecosystem.config.js` for a production process manager. Example commands:

```bash
# Start with PM2
npm run pm2:start

# Stop
npm run pm2:stop

# View logs
npm run pm2:logs
```

- Nginx: an example site config is in `deploy/nginx-beatbox.conf`. Use it as a starting point when configuring a reverse proxy.

**Data & Content**
- The app uses `data/store.json` for song metadata and lightweight persistence. Back up this file before making manual edits.

**Contributing**
- Feel free to open issues or submit pull requests. For quick fixes, edit the HTML/CSS or add songs under `songs/`.

**License & Contact**
- This repository does not include an explicit license file. Add a `LICENSE` if you want to define reuse rules.
- Questions or suggestions: open an issue or contact the maintainer via the repository.

---

If you'd like, I can also:
- commit these changes and push them to GitHub for you,
- or create a pull request with the update.

**Preview & API (Quick Tour)**

- Open the site: visit `http://localhost:3000` to see the main app (`Main.html`). You can also open the static pages directly in a browser:
	- `Main.html` — app landing with browse/play features
	- `profile.html` — user profile / listening stats
	- `history.html` — recently played tracks
	- `download.html` — download manager UI

- How songs are loaded: place audio files (`.mp3`, `.wav`, `.ogg`, `.m4a`) into the `songs/` directory. The server dynamically scans `songs/` and exposes metadata via the API.

- Useful API endpoints (JSON):
	- `GET /api/songs` — list available songs
	- `GET /api/songs/:id` — song details
	- `GET /api/spotlight` — rotating featured artist/track
	- `GET /api/trending` — trending tracks
	- `GET /api/suggestions?genre=Pop` — suggestions by genre
	- `GET /api/likes` and `POST /api/likes` — like/unlike tracks
	- `POST /api/recent-plays` — report a play (server updates stats)
	- `GET /api/storage` — all saved client-side lists (likes, downloads, playlists, recent plays)
	- `GET /api/stream-stats` — Server-Sent Events stream for live stats and now-playing

- Example commands (after `npm start`):

```bash
# list songs
curl http://localhost:3000/api/songs

# get spotlight
curl http://localhost:3000/api/spotlight

# stream live stats (SSE)
curl -N http://localhost:3000/api/stream-stats
```

**How to add content**
- Add audio files to the `songs/` folder. Filenames are used as track titles by default. The server watches the folder and updates the `/api/songs` response automatically.
- To provide richer metadata (artist, duration, genre), extend the server or pre-generate a small JSON manifest in `data/store.json` and adapt the express code accordingly.

**Notes & Limitations**
- This is a lightweight demo project and not intended for production-scale streaming. Use a proper media server or CDN for heavy traffic.
- `data/store.json` is the local persistence layer — it is not encrypted and should be backed up if you rely on it.

---

If you want, I can commit and push this updated `README.md` for you now.
