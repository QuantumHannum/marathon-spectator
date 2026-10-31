# Marathon Day Navigator · November 1, 2026

A free, static, single-page, iPhone-friendly spectator assistant. No API keys or build process.

## Publish free on GitHub Pages

1. Sign into https://github.com and select **New repository**.
2. Name it `marathon-spectator`, make it **Public**, and create it.
3. Choose **Add file → Upload files**, and upload **index.html** from this folder to the repository root (not inside a subfolder). Commit changes.
4. Open repository **Settings → Pages**, set **Build and deployment** to **Deploy from a branch**, select **main** and **/(root)**, then **Save**.
5. Your site URL will be `https://YOUR-GITHUB-USERNAME.github.io/marathon-spectator/` after GitHub finishes publishing. Open it on your iPhone. Add to Home Screen via Safari Share menu.

## Features

- A sequential 9-step plan with a big Next button.
- Runner checkpoint times saved in the iPhone browser's localStorage; predicted passage times adapt, using the interval between two checkpoint observations to estimate pace.
- Interactive OpenStreetMap/Leaflet map with subway station pins; **Locate me** optionally uses your phone's geolocation.
- One-tap Google Maps walking/transit and Apple Maps walking links.
- No runner-tracker API connection or live subway service feeds. Check NYRR and MTA separately.

## Critical warning

The previous plan's Harlem transfer from Lexington Ave (6) to Lenox Ave (2/3) is not a guaranteed walk: the race runs down Fifth Avenue, and NYRR does **not** list Fifth/125th as a controlled crossing. You may be trapped on the east side of the marathon. The app explicitly warns about this and offers the east-side fallback (6 to 33 St, then walk west). Leave the final sighting EARLY, or skip it if a safe return is not assured. Walking directions apps do not account reliably for race barricades.

Google Maps route links open the map app and cannot guarantee which side of the course is accessible. Timing projections and stop mileages are approximate and must be adjusted using the official runner tracker.

Leaflet and map tiles load from the internet, so the map itself is not fully offline. The guide logic and saved checkpoints still work when a previously loaded site remains open.

Data stays in local browser storage and is not shared with GitHub or a server by this application. GitHub Pages hosting is free for public repositories under GitHub Free.


## NYRR runner tracking shortcut

Open `index.html` in a text editor and find the very visible configuration line near the beginning of the `<script>` section:

```javascript
const RUNNER_BIB_NUMBER = ''; // Example: '12345'
```

Replace the blank value with the runner's bib number, **keeping the quotation marks** (for example, `'12345'`). Save the file and upload the updated `index.html` to GitHub. The **Open NYRR live tracker** button launches `https://liveresults.nyrr.org/` in a new tab. When a bib is configured, the website displays the number and attempts to copy it to the clipboard so you can paste it into the official tracker search box. If clipboard access is denied, simply type the displayed number.

**Note:** The shortcut does not auto-search the site, scrape data, or import checkpoint updates. Return to the navigator and enter actual checkpoint times manually. The official tracker requires network connectivity.
