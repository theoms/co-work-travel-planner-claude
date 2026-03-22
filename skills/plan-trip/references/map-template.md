# Interactive Map Template

Use this template to generate the trip map HTML artifact. Customize colors, stops, and routes based on the planned itinerary.

## Technology Stack

- **Leaflet.js** (v1.9.4) — open-source, no API key, loaded from CDN
- **OpenStreetMap tiles** — free, no key needed
- **QR code generation** — inline SVG-based QR codes (no external library needed; use a minimal JS QR encoder or generate SVG path data)

## HTML Template Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Family Trip: [DESTINATION]</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.css" />
  <script src="https://cdnjs.cloudflare.com/ajax/libs/leaflet/1.9.4/leaflet.min.js"></script>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; background: #f8f7f4; color: #2d2d2d; }

    .header {
      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
      color: white; padding: 24px; text-align: center;
    }
    .header h1 { font-size: 1.5rem; font-weight: 600; margin-bottom: 4px; }
    .header p { font-size: 0.9rem; opacity: 0.9; }

    #map { height: 450px; width: 100%; border-bottom: 3px solid #667eea; }

    .legend {
      display: flex; flex-wrap: wrap; gap: 12px; padding: 16px 24px;
      background: white; border-bottom: 1px solid #e5e5e5;
      justify-content: center;
    }
    .legend-item {
      display: flex; align-items: center; gap: 6px; font-size: 0.85rem;
    }
    .legend-dot {
      width: 12px; height: 12px; border-radius: 50%; flex-shrink: 0;
    }

    .day-section { padding: 20px 24px; }
    .day-title {
      font-size: 1.1rem; font-weight: 600; margin-bottom: 12px;
      padding-bottom: 8px; border-bottom: 2px solid #667eea;
    }

    .stops-list { list-style: none; }
    .stop-item {
      padding: 12px 0; border-bottom: 1px solid #eee;
      display: flex; justify-content: space-between; align-items: flex-start;
    }
    .stop-name { font-weight: 500; }
    .stop-name a { color: #667eea; text-decoration: none; }
    .stop-name a:hover { text-decoration: underline; }
    .stop-type { font-size: 0.8rem; color: #888; margin-top: 2px; }
    .stop-price { font-size: 0.85rem; color: #2d2d2d; margin-top: 2px; font-weight: 500; }
    .stop-tip { font-size: 0.85rem; color: #555; margin-top: 4px; font-style: italic; }
    .stop-links { margin-top: 6px; font-size: 0.8rem; }
    .stop-links a { color: #4285f4; text-decoration: none; margin-right: 12px; }
    .stop-links a:hover { text-decoration: underline; }

    details.research { margin-top: 8px; font-size: 0.8rem; color: #666; }
    details.research summary { cursor: pointer; color: #667eea; font-weight: 500; }
    details.research .research-content { padding: 8px 0 4px 0; line-height: 1.5; }

    .maps-link {
      display: inline-block; margin-top: 12px; padding: 10px 20px;
      background: #4285f4; color: white; text-decoration: none;
      border-radius: 8px; font-size: 0.9rem; font-weight: 500;
    }
    .maps-link:hover { background: #3367d6; }

    .qr-section {
      text-align: center; padding: 12px 0;
    }
    .qr-section p { font-size: 0.8rem; color: #888; margin-top: 6px; }

    @media (max-width: 600px) {
      .header { padding: 16px; }
      #map { height: 300px; }
      .day-section { padding: 16px; }
    }
  </style>
</head>
<body>
  <div class="header">
    <h1>[TRIP_TITLE]</h1>
    <p>[DATES] &middot; [NUM_DAYS] days</p>
  </div>
  <div id="map"></div>
  <div class="legend">
    <!-- One legend item per day -->
    <div class="legend-item"><div class="legend-dot" style="background: #FF6B6B"></div> Day 1</div>
    <div class="legend-item"><div class="legend-dot" style="background: #4ECDC4"></div> Day 2</div>
    <div class="legend-item"><div class="legend-dot" style="background: #45B7D1"></div> Day 3</div>
    <!-- Stop type icons — adapt these to the actual stop types in the plan -->
    <!-- Example categories; replace with whatever fits this family's trip -->
    <div class="legend-item"><span>&#127860;</span> Food/Drink</div>
    <div class="legend-item"><span>&#127965;</span> Activity</div>
    <div class="legend-item"><span>&#128205;</span> Stop</div>
  </div>

  <!-- Repeat per day -->
  <div class="day-section">
    <div class="day-title">Day 1 — [DAY_TITLE]</div>
    <ul class="stops-list">
      <!-- Repeat per stop -->
      <li class="stop-item">
        <div>
          <div class="stop-name"><a href="[STOP_URL]" target="_blank">[STOP_NAME]</a></div>
          <div class="stop-type">[STOP_TYPE] &middot; [TIME]</div>
          <div class="stop-price">[PRICE]</div>
          <div class="stop-tip">[PRO_TIP]</div>
          <div class="stop-links">
            <a href="[STOP_URL]" target="_blank">Website</a>
            <a href="[GOOGLE_MAPS_PIN_URL]" target="_blank">Map</a>
            <!-- Add booking link if applicable -->
          </div>
          <details class="research">
            <summary>Why this?</summary>
            <div class="research-content">[RESEARCH_EVIDENCE]</div>
          </details>
        </div>
      </li>
    </ul>
    <a class="maps-link" href="[GOOGLE_MAPS_URL]" target="_blank">
      Open Day 1 Route in Google Maps
    </a>
    <div class="qr-section">
      <!-- Generate QR code SVG inline here -->
      <p>Scan to open on your phone</p>
    </div>
  </div>

  <script>
    // ---- MAP SETUP ----
    const map = L.map('map').setView([LAT, LNG], ZOOM);
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      attribution: '&copy; OpenStreetMap contributors',
      maxZoom: 18
    }).addTo(map);

    // ---- DAY COLORS ----
    const dayColors = ['#FF6B6B', '#4ECDC4', '#45B7D1', '#96CEB4', '#FFEAA7'];

    // ---- STOPS DATA ----
    // Replace with actual stops data. Every stop MUST have a url.
    const stops = [
      // { lat: 40.68, lng: -73.97, name: "Home", day: 0, type: "start", tip: "", url: "", price: "" },
      // { lat: 41.05, lng: -75.27, name: "Local Bakery", day: 0, type: "food", tip: "Known for their sourdough", url: "https://...", price: "$12/person" },
    ];

    // ---- ADD MARKERS ----
    stops.forEach(stop => {
      const color = dayColors[stop.day] || '#999';
      const marker = L.circleMarker([stop.lat, stop.lng], {
        radius: 8, fillColor: color, color: '#fff',
        weight: 2, fillOpacity: 0.9
      }).addTo(map);

      let popup = stop.url
        ? `<strong><a href="${stop.url}" target="_blank">${stop.name}</a></strong>`
        : `<strong>${stop.name}</strong>`;
      if (stop.price) popup += `<br>${stop.price}`;
      if (stop.tip) popup += `<br><em>${stop.tip}</em>`;
      marker.bindPopup(popup);
    });

    // ---- DRAW ROUTES ----
    // Group stops by day and draw polylines
    const days = {};
    stops.forEach(s => {
      if (!days[s.day]) days[s.day] = [];
      days[s.day].push([s.lat, s.lng]);
    });
    Object.keys(days).forEach(day => {
      L.polyline(days[day], {
        color: dayColors[day], weight: 3, opacity: 0.7, dashArray: '8 4'
      }).addTo(map);
    });

    // ---- FIT BOUNDS ----
    if (stops.length > 0) {
      const bounds = L.latLngBounds(stops.map(s => [s.lat, s.lng]));
      map.fitBounds(bounds, { padding: [30, 30] });
    }
  </script>
</body>
</html>
```

## Google Maps URL Construction

Build directions URLs using this format (no API key needed):

```
https://www.google.com/maps/dir/?api=1&origin=ADDRESS_OR_COORDS&destination=ADDRESS_OR_COORDS&waypoints=STOP1|STOP2|STOP3&travelmode=driving
```

Rules:
- URL-encode all addresses: spaces become `+` or `%20`
- Coordinates format: `LAT,LNG` (no spaces)
- Separate waypoints with `|` (URL-encoded as `%7C`)
- Maximum ~10 waypoints per URL (Google Maps limitation)
- For more than 10 stops, split into multiple day links
- The `api=1` parameter is required

Example:
```
https://www.google.com/maps/dir/?api=1&origin=Brooklyn+NY&destination=Jim+Thorpe+PA&waypoints=Red+Bank+NJ%7CColumbia+NJ&travelmode=driving
```

## QR Code Generation

**Preferred approach — inline JS generation (no external dependency):**

Generate QR codes using a lightweight inline JavaScript QR encoder. Include a minimal QR code library directly in the HTML `<script>` block (such as qrcode-generator or a hand-rolled encoder). This avoids external API calls that may be blocked in sandboxed environments.

**Fallback — external API (if inline generation is too complex):**

```html
<img src="https://api.qrserver.com/v1/create-qr-code/?size=150x150&data=ENCODED_GOOGLE_MAPS_URL" alt="QR Code" />
```

This is free and requires no API key, but depends on an external service. URL-encode the Google Maps link before embedding.

**If neither works**, simply present the Google Maps URLs as clickable links — the user can tap them on mobile or copy-paste them. QR codes are a nice-to-have, not essential.

## Color Palette

| Day   | Hex     | Name        |
|-------|---------|-------------|
| Day 1 | #FF6B6B | Coral Red   |
| Day 2 | #4ECDC4 | Teal        |
| Day 3 | #45B7D1 | Sky Blue    |
| Day 4 | #96CEB4 | Sage Green  |
| Day 5 | #FFEAA7 | Warm Yellow |

Stop type categories and their emojis should be generated dynamically based on the actual stops in this family's plan. Don't use a fixed set of categories — derive them from the itinerary. Some example mappings:

| Stop Type   | Emoji |
|-------------|-------|
| Food/Drink  | 🍽    |
| Activity    | 🏕    |
| Nature      | 🌿    |
| Water/Swim  | 🏊    |
| Rest Stop   | 📍    |
| Culture     | 🎨    |
| Shopping    | 🛍    |
| Scenic      | 📸    |

Pick only the categories that appear in the actual plan.

## Pin Popup Content

Each pin popup should include:
- **Name** as a clickable link to the business website or listing
- **Type** (Food, Activity, Lodging, etc.)
- **Price** (actual number, not a range symbol)
- **Time** if scheduled ("~10:30am")
- **Pro tip** (italic, one line — the kind of thing a local would tell you)
- **Hours** if relevant ("Open 7am-3pm, closed Mon")
- **Quick links**: Website | Book | Directions
