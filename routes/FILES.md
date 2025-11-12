# routes/routes.go  
**Package name**    
`routes`  
  
---  
  
### Imports  
No external imports are declared in this file; it only defines a set of constants.  
  
### External data / input sources  
All values defined here represent URL fragments used by the Shodan API client:  
* Base URLs for the main API, exploits and stream services.  
* Endpoints for searching, counting, viewing, and manipulating hosts, scans, alerts, datasets, etc.  
* Additional routes for stream‑specific resources (banners, ASN, countries, ports list, alerts, tags, vulns) and notifier resources.  
  
### TODOs  
No `TODO` comments are present in this file.  
  
---  
  
## Summary of major code parts  
  
| Section | Purpose |  
|---------|---------|  
| **Base URLs** | `ApiRoot`, `ApiExploits`, `ApiStream` – the root endpoints for the three Shodan services. |  
| **Search & count helpers** | `Search`, `Count` – generic query paths used by many other routes. |  
| **Host‑related routes** | Paths beginning with `/shodan/host/...` (view, count, search, tokens, filters, facets). These are the primary endpoints for host data retrieval and manipulation. |  
| **Port & service routes** | `ShodanPorts`, `ShodanServices`, `ShodanProtocols` – endpoints for port/service/protocol information. |  
| **Scan‑related routes** | Paths beginning with `/shodan/scan/...` (view, internet, scans). These are used to query and manage scan data. |  
| **Alert & notifier routes** | Paths under `/shodan/alert/...` and `/notifier/...` – endpoints for alert creation, notification, and provider configuration. |  
| **Stream‑specific routes** | Routes prefixed with `ShodanBanners`, `ShodanAsn`, etc., used when interacting with the stream API. |  
| **Miscellaneous helpers** | Paths such as `/shodan/data/%s`, `/org/member/%s`, `/account/profile`, `/dns/...` – additional endpoints for data, organization, DNS resolution and tools. |  
  
All constants are grouped logically by their functional area, making it easy to reference them when building HTTP requests elsewhere in the package.  
  
