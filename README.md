# 🗑️ GarbageRoute — VRP Optimizer

> A browser-based Vehicle Routing Problem (VRP) optimizer for municipal garbage collection. Plan optimal truck routes across city zones using real road distances — no backend required.

![HTML](https://img.shields.io/badge/frontend-HTML%2FCSS%2FJS-green) ![Leaflet](https://img.shields.io/badge/map-Leaflet.js-blue) ![ORS](https://img.shields.io/badge/routing-OpenRouteService-orange) ![License](https://img.shields.io/badge/license-MIT-violet)

---

## 🌍 Live Demo

Open `vrp_fixed.html` directly in any browser — no server or installation needed.

---

## 📸 What It Does

GarbageRoute helps city planners and waste management teams answer one question:

> **"Given N garbage collection zones with different waste amounts, what is the most efficient set of truck routes?"**

You input zones, set truck capacity, and the algorithm automatically calculates how many trucks are needed and assigns optimal routes — visualized live on a real map.

---

## ✨ Features

### Core
- 🗺️ **Interactive Leaflet Map** — real OpenStreetMap tiles with live route drawing
- 📍 **Click-to-Place Zones** — click anywhere on the map to place the depot or add a zone
- 🚛 **Auto Truck Calculation** — trucks needed = `⌈total waste ÷ capacity⌉`, computed live
- ⚡ **VRP Algorithm** — nearest-neighbor heuristic assigns zones to trucks optimally
- 📏 **Real Road Distances** — uses OpenRouteService Matrix API for actual driving distances
- 📐 **Euclidean Fallback** — works offline without an API key using straight-line distances

### Input Modes
- **Grid Mode (X/Y)** — simple 0–100 coordinate grid for quick testing
- **Real Coords Mode (Lat/Lng)** — use actual GPS coordinates for real-world routing

### Map Interaction
- ⭐ **Place Depot on Map** — click to set the depot location directly on the map
- 📍 **Add Zone on Map** — click to keep adding zones one by one
- 🖱️ **Crosshair cursor** activates during placement mode
- **Esc key** cancels placement at any time

### Results
- Color-coded routes per truck (up to 8 colors)
- Dashed return lines back to depot
- Arrow dots showing travel direction
- Per-truck load, distance, and stop list
- Warning if any zones are unserved (capacity too low)
- Summary pills: total trucks, total distance, total waste collected

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Map | Leaflet.js v1.9.4 |
| Map Tiles | OpenStreetMap |
| Road Distances | OpenRouteService Matrix API |
| Fonts | DM Sans + Space Mono (Google Fonts) |
| Algorithm | Nearest-Neighbor VRP Heuristic |

---

## 🚀 Getting Started

### No Installation Needed

Just open the file in your browser:

```bash
# Option 1 — Direct open
open vrp_fixed.html

# Option 2 — Local server (optional, for CORS safety)
npx serve .
# then visit http://localhost:3000/vrp_fixed.html
```

### Getting a Free ORS API Key

1. Go to [openrouteservice.org/dev/#/signup](https://openrouteservice.org/dev/#/signup)
2. Create a free account
3. Copy your API key
4. Paste it into the **ORS API Key** field in the app
5. Click **Test** to verify it works

> Without an API key, the app still works using Euclidean (straight-line) distance estimation.

---

## 🧭 How To Use

**1. Set your API key** *(optional but recommended)*
Paste your OpenRouteService key and click Test.

**2. Set truck capacity**
Enter the maximum waste (kg) each truck can carry. Truck count auto-updates.

**3. Add your zones**
- Click **Place Depot on Map** → click your depot location on the map
- Click **Add Zone on Map** → click each collection zone on the map
- Or use **+ Add Zone (manual entry)** to type coordinates directly
- Switch between **Grid** and **Real Coords** modes as needed

**4. Optimize**
Click **▶ Optimize Routes** — the algorithm runs and draws color-coded routes on the map.

**5. Read the results**
Each truck card shows: load, distance, and full stop sequence (Depot → Zone → Zone → Depot).

---

## 🧠 Algorithm Explained

### Step 1 — Auto Truck Count
```
trucks_needed = ⌈ total_waste_kg ÷ truck_capacity_kg ⌉
```

### Step 2 — Distance Matrix
If an ORS API key is provided, fetches a real **N×N road distance matrix** (km) for all zones via the ORS Matrix API. Otherwise falls back to Euclidean distance.

### Step 3 — Nearest-Neighbor VRP
For each truck:
1. Start at the depot
2. Find the nearest unvisited zone that fits within remaining capacity
3. Visit it, reduce remaining capacity
4. Repeat until no more zones fit
5. Return to depot
6. Next truck picks up remaining unvisited zones

This is a **greedy heuristic** — fast and practical for real-world city routing with up to ~20 zones.

---

## 📡 API Reference

### OpenRouteService Matrix API

```
POST https://api.openrouteservice.org/v2/matrix/driving-car
Authorization: YOUR_API_KEY
Content-Type: application/json

{
  "locations": [[lng, lat], [lng, lat], ...],
  "metrics": ["distance"],
  "units": "km"
}
```

Returns an `N×N` matrix of driving distances in km between all zone pairs.

---

## 📁 Project Structure

```
vrp_fixed.html         # Complete single-file app
README.md
```

Everything — HTML, CSS, JavaScript — is in one self-contained file. No build step, no dependencies to install.

---

## 🗺️ Default Sample Data

The app loads with 8 real zones from **Chennai, India**:

| Zone | Waste (kg) |
|---|---|
| Depot (Central Chennai) | — |
| T. Nagar | 500 |
| Anna Nagar | 300 |
| Adyar | 450 |
| Velachery | 200 |
| Tambaram | 600 |
| Guindy | 350 |
| Perambur | 250 |
| Sholinganallur | 400 |

Default truck capacity: **1000 kg** → requires **3 trucks** to cover all 3050 kg of waste.

---

## ⚠️ Limitations

- **Nearest-neighbor heuristic** is not globally optimal — for true optimization, use algorithms like Clarke-Wright Savings or Genetic Algorithms
- **ORS free tier** has rate limits (500 req/day, max 50 locations per matrix call)
- **No route persistence** — results are not saved between sessions
- Works best with **up to ~20 zones** for fast performance

---

## 🗺️ Roadmap

- [ ] Export routes as CSV / PDF report
- [ ] Time windows per zone (arrive between 8am–10am)
- [ ] Multiple depots support
- [ ] Clarke-Wright savings algorithm for better optimization
- [ ] Import zones from CSV file
- [ ] Save and load route configurations

---

## 🤝 Contributing

1. Fork the repository
2. Create a branch — `git checkout -b feature/my-feature`
3. Make your changes to `vrp_fixed.html`
4. Submit a Pull Request

---

## 📄 License

MIT License — free to use, modify and distribute.

---

## 👤 Author

Built with Leaflet.js + OpenRouteService for real-world waste management route planning.

> GarbageRoute — smarter cities start with smarter routes. 🌿
