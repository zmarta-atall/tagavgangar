# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Single-file web app (`tagavgangar.html`) displaying real-time train and bus departures for Ängelholm, Sweden. Pure HTML/CSS/JavaScript — no build system, no dependencies, no framework.

Open `tagavgangar.html` directly in a browser to run. API keys are stored in localStorage.

## Architecture

The app combines three APIs:

- **Trafikverket API** (trains) — POST XML queries to `api.trafikinfo.trafikverket.se/v2/data.json`. Batches 4 queries (departures/arrivals for both directions) in one request. Matches departures with arrivals by `AdvertisedTrainIdent`, filtering out matches where arrival < departure (reused train numbers).

- **ResRobot Trip Planner** (bus routes) — GET `api.resrobot.se/v2.1/trip`. Max `numF: 6`. Pre-configured stop IDs: Stortorget `740031349`, Station `740000064`.

- **Trafiklab Realtime API** (bus delays) — GET `realtime-api.trafiklab.se/v1/departures/{stopId}`. Enriches ResRobot trips with delay/cancellation/platform data.

## Key Functions

- `matchTrains(departures, arrivals)` — pairs departures with arrivals, shows 2 per direction
- `fetchBuses()` → `parseTrips()` → `enrichWithRealtime()` — fetches 6 trips, displays 5 per direction
- Auto-refreshes every 60 seconds via `setInterval`

## Status Colors

- Green: on time / departed
- Orange: delayed
- Red: cancelled
- Light blue: early (ahead of schedule)
