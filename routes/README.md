# `routes`

The **`routes`** package is a lightweight helper that centralises all URL fragments used by the Shodan API client.  
It contains only constant definitions – no functions or types – so it can be imported wherever an HTTP request needs to be built.

---

## File structure

```
routes/
├── routes.go
```

* `routes/routes.go` – defines a set of constants that represent base URLs and specific endpoints for the Shodan API, its exploits service, and stream service.

---

## What the code does

| Section | Purpose |
|---------|---------|
| **Base URLs** | `ApiRoot`, `ApiExploits`, `ApiStream` – root paths for the three Shodan services. |
| **Search & count helpers** | `Search`, `Count` – generic query fragments used by many other routes. |
| **Host‑related routes** | Paths beginning with `/shodan/host/...` (view, count, search, tokens, filters, facets). These are the primary endpoints for host data retrieval and manipulation. |
| **Port & service routes** | `ShodanPorts`, `ShodanServices`, `ShodanProtocols` – endpoints for port/service/protocol information. |
| **Scan‑related routes** | Paths beginning with `/shodan/scan/...` (view, internet, scans). These are used to query and manage scan data. |
| **Alert & notifier routes** | Paths under `/shodan/alert/...` and `/notifier/...` – endpoints for alert creation, notification, and provider configuration. |
| **Stream‑specific routes** | Routes prefixed with `ShodanBanners`, `ShodanAsn`, etc., used when interacting with the stream API. |
| **Miscellaneous helpers** | Paths such as `/shodan/data/%s`, `/org/member/%s`, `/account/profile`, `/dns/...` – additional endpoints for data, organization, DNS resolution and tools. |

All constants are grouped logically by functional area, making it easy to reference them when building HTTP requests elsewhere in the package.

---

## Environment variables / flags

The file itself does not expose any environment variables or command‑line flags; it simply provides constants that other packages can import.  
If a consumer wants to override a base URL (e.g., for testing), they could set an env var such as `ROUTES_API_ROOT` and use the value in code that imports this package.

---

## How the application can be launched

* **Library usage** – Import `"github.com/yourorg/shodan/routes"` (or whatever module path) and reference constants like `routes.ApiRoot` when constructing URLs for HTTP requests.
* **CLI / main package** – If a command‑line tool needs to query hosts, scans or alerts, it can import this package and use the grouped constants to build request paths.  
  Example:  

```go
url := fmt.Sprintf("%s/shodan/host/view/%d", routes.ApiRoot, hostID)
```

The file contains no `main()` function; it is intended as a shared helper.

---

## Summary

`routes/routes.go` centralises all Shodan API endpoint fragments in one place.  
It defines constants for base URLs and specific paths for hosts, scans, alerts, stream resources, etc., grouped by functional area.  
Other packages can import `routes` to build HTTP requests without hard‑coding string literals.