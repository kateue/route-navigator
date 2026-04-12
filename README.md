# RouteNav — Live Navigation for Custom Routes

**Turn your Google My Maps routes into real, live, turn-by-turn navigation.**

🗺 [Live Demo](https://kateue.github.io/route-navigator) | 💻 [Portfolio](https://kateue.github.io/portfolio)

---

## The Problem

Google My Maps is genuinely useful. You can plan a custom bike route, a walking tour, a scenic drive — add your own waypoints, draw your own path, layer your own logic on top of a map. It feels like the right tool.

Until you try to actually *follow* the route.

Export the file and open it anywhere — Google Maps, Maps.me, any GPX viewer — and you get a static bird's-eye picture. The route sits there. There's no "start navigation." No voice telling you when to turn. No sense of where you are on the path. If you look down at your phone for a second you've already missed the turn, because nothing told you it was coming.

This gap is real and it affects a specific kind of user: cyclists planning custom rides, hikers piecing together trails from multiple sources, anyone who builds a route in My Maps and then has to fumble through it manually while moving.

---

## The Solution

RouteNav is a browser-based prototype that bridges that gap. It takes any `.kml` or `.gpx` file — the same file you'd export from Google My Maps — and turns it into a live, GPS-tracked navigation session with turn-by-turn voice directions.

No app install. No account. No API key. Works for anyone with the link.

---

## Features

- **KML & GPX support** — direct export from Google My Maps, Komoot, Strava, Garmin, or any standard GPS tool
- **Automatic turn detection** — parses the route geometry, identifies bearing changes, and classifies each turn (bear left, turn right, sharp right, U-turn)
- **Live GPS tracking** — uses the browser's built-in Geolocation API to track your position and snap it to the nearest route segment
- **Voice-guided directions** — speaks upcoming turns using the Web Speech API: *"In 300 metres, turn right"* / *"Turn left now"*
- **Distance cues** — voice alerts at 500m, 200m, and 50m before each turn
- **Off-route detection** — warns you audibly and visually when you drift more than ~55m from the path
- **Speed + remaining distance** — live stats shown at the bottom of the navigation screen
- **Arrival detection** — announces when you've reached your destination
- **Dark map tiles** — uses free CartoDB/OpenStreetMap tiles, no Google Maps API required
- **Mobile-first** — designed to be used on a phone while moving

---

## How to Use

### Step 1 — Export your route from Google My Maps

1. Open [Google My Maps](https://mymaps.google.com) and open your map
2. Click the **⋮** (three dots) next to the layer name
3. Select **Export to KML/KMZ**
4. Uncheck *"Export as KMZ"*
5. Click **Download** — you'll get a `.kml` file

> GPX files from Strava, Komoot, Garmin, or any other app also work.

### Step 2 — Open RouteNav

Go to **[kateue.github.io/route-navigator](https://kateue.github.io/route-navigator)**

### Step 3 — Upload your file

Drag and drop your `.kml` or `.gpx` file, or click **Browse file**.

### Step 4 — Navigate

Hit **▶ Start Navigation**. Allow location access when prompted.

RouteNav will track your position, announce upcoming turns, warn if you go off route, and tell you when you've arrived.

---

## Technical Overview

### Architecture

Zero-dependency, single-file web application. Runs entirely in the browser — no server, no backend, no API calls.

```
route-navigator/
└── index.html     ← everything: HTML, CSS, JS in one file
```

### Key Technologies

| Layer | Technology | Why |
|---|---|---|
| Map rendering | Leaflet.js | Lightweight, open-source, no API key |
| Map tiles | CartoDB Dark Matter via OpenStreetMap | Free, dark aesthetic, no key required |
| GPS positioning | Browser Geolocation API | Native to every modern browser |
| Voice directions | Browser Web Speech API | Built-in text-to-speech, zero cost |
| Route parsing | Native DOMParser | Parses KML/GPX XML without a library |
| Hosting | GitHub Pages | Static file, deploys directly from repo |

### Navigation Algorithm

**Turn detection** — the route is simplified to remove near-duplicate GPS points (< 6m apart). For each consecutive triple of points, the incoming and outgoing bearings are computed using the Haversine formula. A bearing change over 22° is classified as a turn:

| Angle change | Classification |
|---|---|
| 22° – 45° | Bear left / Bear right |
| 45° – 110° | Turn left / Turn right |
| 110° – 155° | Sharp left / Sharp right |
| > 155° | U-turn |

**Route snapping** — on each GPS update, the app finds the nearest segment using perpendicular projection, advancing forward-only to prevent backtracking.

**Off-route detection** — if GPS position exceeds 55m from the nearest route segment, the user is flagged and notified by voice and visual indicator.

---

## Running Locally

```bash
git clone https://github.com/kateue/route-navigator.git
cd route-navigator
# Serve locally (GPS requires HTTPS or localhost)
python3 -m http.server 8080
# Open http://localhost:8080
```

---

## Known Limitations (Prototype)

- **No rerouting** — warns when off route but does not recalculate a new path
- **GPS accuracy** — dependent on device hardware; dense urban environments may affect precision
- **Voice on iOS** — Safari requires a user gesture before speech synthesis; tap Start Navigation to activate

---

## What's Next

- Offline tile caching for routes without data coverage
- Route re-entry guidance when off route
- Elevation profile from GPX altitude data
- Multi-stop ETA based on current speed
- Share link with embedded route via URL parameter

---

## About

Built by [Kate Ueda](https://kateue.github.io/portfolio) — full-stack developer focused on AI strategy and bridging real-world problems with practical software.

This project came from a specific, real frustration: planning a bike route in Google My Maps and having no way to follow it live. RouteNav is the prototype that shouldn't have needed to exist — but does.
