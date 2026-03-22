# Family Trip Planner

A comprehensive travel concierge plugin for Claude Cowork that takes the mental load off family trip planning.

## What it does

Tell Claude you want to plan a trip and this plugin handles everything: understanding your family's unique preferences, finding the perfect destination, planning the journey with personalized stops, building a day-by-day itinerary packed with local hidden gems, and generating an interactive map with QR codes you can scan to open each day's route on your phone.

Designed for busy parents who don't have time to research. The plugin learns what your specific family loves — dietary needs, activity preferences, energy levels, kids' ages and interests — then tailors every single recommendation to match. It thinks like a local, digging into food blogs, community calendars, and off-the-beaten-path recommendations instead of just pulling up generic Google results.

## How to use it

Just say things like:

- "Plan a long weekend getaway for our family"
- "We want a road trip with the kids this spring"
- "Find us a family-friendly weekend destination within 2 hours"
- "Plan our summer vacation"

Claude will ask a few questions about your family, preferences, and budget, then build the entire trip plan.

## What you get

1. **Destination recommendations** — 2-3 options with a "vibe check" for each, matched to your family's stated preferences
2. **Travel plan** — Departure times, stops tailored to what your family enjoys, return timing
3. **Day-by-day itinerary** — Activities, meals, and local events personalized to your interests, dietary needs, and kids' ages, with practical details (hours, parking, reservations, pro tips)
4. **Interactive map** — A beautiful HTML map with all stops pinned, color-coded by day
5. **Google Maps links** — One-tap links to open each day's route on your phone, plus QR codes
6. **Packing & logistics checklist** — Trip-specific packing lists, budget breakdown, pre-trip to-dos

## Installation

Download the latest `.plugin` file from [Releases](../../releases) and open it in Claude Desktop. The plugin will appear in your skills list.

## Setup

**None required.** The plugin works out of the box with zero configuration. It uses Claude's built-in web search for research and free open-source mapping tools.

### Optional connectors

For richer data, you can optionally connect:

- **Maps** (e.g., Google Maps MCP) — adds real-time traffic, exact drive times, verified business hours
- **Weather** (e.g., OpenWeather MCP) — adds trip-date forecasts and rainy-day backup plans

See `CONNECTORS.md` for details.

## Components

| Component | Name      | Description                                    |
| --------- | --------- | ---------------------------------------------- |
| Skill     | plan-trip | The main orchestrator — runs the full workflow  |
