# 🚗 ch-parking-endpoints

> A public catalog of parking availability endpoints, feeds, and data sources discovered on public Swiss websites.

🇨🇭 **Scope:** Switzerland  ·  📦 **Sources:** 2  ·  🅿️ **Parkings:** 14  ·  🔌 **Endpoints:** 2

---

## 📚 What's inside

Each source describes **one operator** and lives in [`sources/`](sources/) as a single JSON file named `ch-<city>-<operator>.json`.

- 🔌 **Endpoints** — live data feeds (URL, method, format, auth, response shape, documented flag)
- 🅿️ **Parkings** — the garages those endpoints cover (name, address, capacity, max height, info URL)

## 🗺️ Sources

| File | Operator | City | Parkings | Endpoints |
| --- | --- | --- | --- | --- |
| [`ch-thun-parkhausthun.json`](sources/ch-thun-parkhausthun.json) | Parkhaus Thun AG | 📍 Thun (BE) | 4 | 1 (undocumented JSON) |
| [`ch-bern-parkingbern.json`](sources/ch-bern-parkingbern.json) | Parking Bern (aggregator) | 📍 Bern (BE) | 10 | 1 (undocumented XML) |

## 🧪 Try it

### Thun — JSON

```bash
curl -s https://www.parkhausthun.ch/update-carparks.php | jq
```

Returns an object keyed by parking identifier — `free`, `obstructed`, and `capacity` are zero-padded strings:

```json
{
  "CityNord": { "identifier": "CityNord", "free": "441", "obstructed": "134", "capacity": "575" },
  "CityOst":  { "identifier": "CityOst",  "free": "116", "obstructed": "187", "capacity": "303" },
  "CitySued": { "identifier": "CitySued", "free": "055", "obstructed": "030", "capacity": "085" },
  "CityWest": { "identifier": "CityWest", "free": "402", "obstructed": "153", "capacity": "555" }
}
```

### Bern — XML

```bash
curl -s https://www.parking-bern.ch/parkdata.xml | xmllint --format -
```

Returns a `<parkdata>` root with a feed timestamp and one `<parking>` element per garage — all data lives in attributes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<parkdata updated="11.05.2026 01:24:30">
  <parking name="P01"  state="1" spacecount="598" spacefree="494" open="00:00" close="00:00"/>
  <parking name="P02"  state="1" spacecount="425" spacefree="353" open="00:00" close="00:00"/>
  <parking name="P+R"  state="1" spacecount="600" spacefree="518" open="00:00" close="00:00"/>
  <parking name="P09"  state="1" spacecount="-1"  spacefree="0"   open="00:00" close="00:00"/>
  <!-- … 8 more entries -->
</parkdata>
```

Note `spacecount="-1"` for P09 — sentinel for closed / unknown. Values change in real time.

## 🤝 Contributing

Found a new public endpoint? Open a PR adding a file under [`sources/`](sources/) that matches the existing shape. Please only catalog sources discovered on publicly accessible websites, and be considerate with request rates when probing endpoints.

## ⚠️ Disclaimer

This repo is a collection of parking endpoints I've stumbled across on public websites. **No guarantees whatsoever, for either the code or the endpoints.**

- 🪦 Endpoints may break, change, move, or vanish at any time — they are not contracts.
- 🐛 The code in this repo is provided **as-is**, may contain mistakes, and comes with no warranty.
- 🔍 Verify any data before relying on it for anything that matters.
- ✉️ Operators who want their endpoint removed: open an issue or PR.

## 📜 License

MIT — see [LICENSE](LICENSE).
