# 🚗 ch-parking-endpoints

> A public catalog of parking availability endpoints, feeds, and data sources discovered on public Swiss websites.

🇨🇭 **Scope:** Switzerland  ·  📦 **Sources:** 4  ·  🅿️ **Parkings:** 66  ·  🔌 **Endpoints:** 7

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
| [`ch-luzern-plsluzern.json`](sources/ch-luzern-plsluzern.json) | PLS Parkleitsystem Luzern AG (aggregator) | 📍 Luzern (LU) | 16 | 1 (undocumented JSON, CORS-enabled) |
| [`ch-zuerich-plszh.json`](sources/ch-zuerich-plszh.json) | Stadt Zürich Parkleitsystem (official) | 📍 Zürich (ZH) | 36 | 4 (1 CC0 RSS + 3 secondary) |

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
<parkdata updated="11.05.2026 01:31:30">
  <parking name="P01"  state="1" spacecount="598" spacefree="494" open="00:00" close="00:00"/>
  <parking name="P02"  state="1" spacecount="425" spacefree="353" open="00:00" close="00:00"/>
  <parking name="P+R"  state="1" spacecount="600" spacefree="517" open="00:00" close="00:00"/>
  <parking name="P09"  state="1" spacecount="-1"  spacefree="0"   open="00:00" close="00:00"/>
  <!-- … 8 more entries -->
</parkdata>
```

Note `spacecount="-1"` for P09 — this is the real value published by the feed, used as a sentinel when a parking isn't reporting (P09 is an outdoor lot without entry-barrier counting). Values change in real time.

### Zürich — RSS (CC0, Open Data)

```bash
curl -s https://www.pls-zh.ch/plsFeed/rss | xmllint --format -
```

The official feed published as Open Data by the City of Zürich, [CC0-licensed](https://data.stadt-zuerich.ch/dataset/parkleitsystem). Each `<item>` is one parking; live free spaces are inside `<description>` as `<state> /  <free>`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:dc="http://purl.org/dc/elements/1.1/">
  <channel>
    <title>FEED Parkleitsystem Stadt Zürich</title>
    <copyright>CC-0 License</copyright>
    <item>
      <title>Parkhaus Accu / Otto-Schütz-Weg</title>
      <description>open /  156</description>
      <link>https://www.pls-zh.ch/parkhaus/accu.jsp?pid=accu</link>
      <pubDate>Sun, 10 May 2026 22:33:20 GMT</pubDate>
      <dc:date>2026-05-10T22:33:20Z</dc:date>
    </item>
    <!-- … 35 more items -->
  </channel>
</rss>
```

The RSS feed itself only has free spaces — capacity, addresses and coordinates come from three secondary endpoints documented in the source file (ParkenDD JSON mirror with CORS, WordPress REST metadata, and an internal `api.php`).

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
