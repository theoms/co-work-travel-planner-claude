# Connectors

## How tool references work

Plugin files use `~~category` as a placeholder for whatever tool the user connects in that category. For example, `~~maps` might mean a Google Maps MCP server, a Mapbox integration, or any other mapping service.

This plugin works great **without any connectors** — it uses Claude's built-in web search for research and free/open-source tools (Leaflet.js, OpenStreetMap, Google Maps URLs) for maps. Connectors are optional upgrades that add richer data.

## Connectors for this plugin

| Category | Placeholder  | Included servers | Other options                        |
| -------- | ------------ | ---------------- | ------------------------------------ |
| Maps     | `~~maps`     | (none)           | Google Maps MCP, Mapbox MCP          |
| Weather  | `~~weather`  | (none)           | OpenWeather MCP, Weather.gov MCP     |
