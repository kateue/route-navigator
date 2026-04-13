# RouteNav — Live Navigation for Custom Routes

**Turn your Google My Maps routes into real, live, turn-by-turn navigation.**

🗺 [Live Demo](https://kateue.github.io/route-navigator) | 💻 [Portfolio](https://kateue.github.io/portfolio)

---

## The Problem

Google My Maps is genuinely useful. You can plan a custom bike route, a walking tour, a scenic drive, add your own waypoints, draw your own path, layer your own logic on top of a map. It feels like the right tool.

Until you try to actually *follow* the route.

Export the file and open it anywhere — Google Maps, Maps.me, any GPX viewer — and you get a static bird's-eye picture. The route sits there. There's no "start navigation." No voice telling you when to turn. No sense of where you are on the path. 

This gap is real and it affects a specific kind of user: cyclists planning custom rides (me!), hikers piecing together trails from multiple sources, anyone who builds a route in My Maps and then has to fumble through it manually while moving.

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

## Known Limitations (Prototype)

- **No rerouting** — warns when off route but does not recalculate a new path
- **GPS accuracy** — dependent on device hardware; dense urban environments may affect precision
- **Voice on iOS** — Safari requires a user gesture before speech synthesis; tap Start Navigation to activate

---

## About

Built by [Kate Ueda](https://kateue.github.io/portfolio) — full-stack developer focused on AI strategy.

This project came from a specific, real frustration: planning a bike route in Google My Maps and having no way to follow it live. RouteNav is the prototype that shouldn't have needed to exist, but does...
