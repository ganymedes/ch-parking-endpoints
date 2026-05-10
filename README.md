# ch-parking-endpoints

A public catalog of parking availability endpoints, feeds, and data sources discovered on public websites — focused on Switzerland.

## Structure

Each source lives in [`sources/`](sources/) as a single JSON file named `ch-<city>-<operator>.json`. A source describes one operator and contains:

- one or more **endpoints** — live data feeds (URL, HTTP method, response format, auth, response shape, whether the endpoint is officially documented)
- the **parkings** the endpoints cover — name, address, structural capacity, max vehicle height, info URL

## Sources

| File | Operator | City | Parkings | Endpoints |
| --- | --- | --- | --- | --- |
| [`ch-thun-parkhausthun.json`](sources/ch-thun-parkhausthun.json) | Parkhaus Thun AG | Thun (BE) | 4 | 1 (undocumented JSON) |

## License

MIT — see [LICENSE](LICENSE). All sources listed here were discovered on publicly accessible websites.
