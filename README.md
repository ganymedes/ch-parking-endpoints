# 🚗 ch-parking-endpoints

> A public catalog of parking availability endpoints, feeds, and data sources discovered on public Swiss websites.

🇨🇭 **Scope:** Switzerland  ·  📦 **Sources:** 1  ·  🅿️ **Parkings:** 4  ·  🔌 **Endpoints:** 1

---

## 📚 What's inside

Each source describes **one operator** and lives in [`sources/`](sources/) as a single JSON file named `ch-<city>-<operator>.json`.

- 🔌 **Endpoints** — live data feeds (URL, method, format, auth, response shape, documented flag)
- 🅿️ **Parkings** — the garages those endpoints cover (name, address, capacity, max height, info URL)

## 🗺️ Sources

| File | Operator | City | Parkings | Endpoints |
| --- | --- | --- | --- | --- |
| [`ch-thun-parkhausthun.json`](sources/ch-thun-parkhausthun.json) | Parkhaus Thun AG | 📍 Thun (BE) | 4 | 1 (undocumented JSON) |

## 🧪 Try it

The Thun endpoint, for example:

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

Values change in real time.

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
