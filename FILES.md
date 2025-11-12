# client.go  
**Package:** `shodan`    
**Imports**  
  
| Package | Purpose |  
|---------|---------|  
| `context` | Context propagation for HTTP requests |  
| `encoding/json` | JSON marshaling/unmarshaling |  
| `errors` | Error handling |  
| `fmt` | Formatting error messages |  
| `io` | Reader/Writer interfaces |  
| `ioutil` | Reading response bodies |  
| `net/http` | HTTP client & request types |  
| `net/url` | URL building and query parameters |  
| `github.com/shadowscatcher/shodan/models` | Data models (e.g. `models.Error`) |  
| `github.com/shadowscatcher/shodan/routes` | API route constants (`ApiRoot`, `ApiExploits`) |  
| `sync` | Mutex for concurrent request handling |  
| `time` | Timing utilities |  
  
---  
  
### External data / input sources  
* `routes.ApiRoot` – base URL for the Shodan root API    
* `routes.ApiExploits` – base URL for the Shodan exploits API    
* `models.Error` – structure used to parse error responses  
  
---  
  
### TODOs  
No explicit `TODO:` comments were found in this file.  
  
---  
  
## Summary of major code parts  
  
### 1. Constants & Client struct  
```go  
const intSecond = int64(time.Second)  
```  
Defines a constant for the wait interval (1 second).    
The `Client` type holds API key, last request timestamp, a mutex, and an HTTP client.  
  
### 2. Wait helper  
```go  
func waitASecond(client *Client) { … }  
```  
Calculates elapsed time since the last request and sleeps if less than one second has passed.  
  
### 3. Client constructor  
```go  
func GetClient(apiKey string, client *http.Client, wait bool) (*Client, error)  
```  
Creates a `Client` instance, validates inputs, assigns the wait function (either `waitASecond` or no‑op), and returns it.  
  
### 4. Parameter handling  
* `defaultParams()` – builds default URL query values with the API key.  
* `ensureRequestParams(params url.Values)` – guarantees that every request contains the key parameter.  
  
### 5. Request creation helpers  
* `createRequest(...)` – generic helper that turns method, URI, params, body and header into an `http.Request`.  
* `createRootRequest(...)` – builds a request for the root API (`routes.ApiRoot + endpoint`).  
* `createExploitRequest(...)` – same but for the exploits API (`routes.ApiExploits + endpoint`).  
  
### 6. Response handling  
```go  
func errFromResponse(response *http.Response) error { … }  
```  
Reads the response body, unmarshals it into a `models.Error`, and returns any error.  
  
```go  
func (c *Client) readResponse(to interface{}, body io.Reader) error { … }  
```  
Copies or decodes the HTTP body into the supplied destination.  
  
### 7. Request execution pipeline  
* `requestAndRead(req *http.Request, result interface{})` – performs the request with mutex protection, updates timing, and reads the response.  
* `request(...)` – convenience wrapper that creates a root request and calls `requestAndRead`.  
* `requestExploits(...)` – same but for exploits API.  
  
### 8. Convenience GET helper  
```go  
func (c *Client) get(ctx context.Context, route string, params url.Values, result interface{}) error { … }  
```  
Simplifies issuing a GET request to the root API and unmarshaling its response into `result`.  
  
---  
  
All functions together provide a lightweight client for interacting with Shodan’s REST endpoints, handling rate‑limiting, parameter injection, and JSON decoding in a single package.  
  
# methods.go  
# shodan.Client API Wrapper    
  
The file implements a comprehensive set of methods on the `*Client` type in the **shodan** package.    
It provides high‑level wrappers around Shodan’s REST endpoints, turning HTTP requests into typed Go structures and handling request/response marshaling.  
  
---  
  
## Imports  
  
```go  
import (  
    "bytes"  
    "context"  
    "encoding/json"  
    "errors"  
    "fmt"  
    "net/http"  
    "net/url"  
    "strings"  
  
    "github.com/shadowscatcher/shodan/models"  
    "github.com/shadowscatcher/shodan/routes"  
    "github.com/shadowscatcher/shodan/search"  
)  
```  
  
* Standard library packages for HTTP, JSON, URL handling and string manipulation.    
* Local packages: `models` (data structures), `routes` (URL templates), `search` (query helpers).  
  
---  
  
## External Data Sources  
  
| Method | Endpoint (from `routes`) | Purpose |  
|--------|---------------------------|---------|  
| `Host` | `ShodanHostView` | Retrieve services for a given host IP. |  
| `Count` | `ShodanHostCount` | Return total result count for a search query. |  
| `Search` | `ShodanHostSearch` | Full search with facets. |  
| `SearchTokens` | `ShodanHostSearchTokens` | Token‑based search breakdown. |  
| `Ports` | `ShodanPorts` | List available port numbers. |  
| `Protocols` | `ShodanProtocols` | Map of supported protocols. |  
| `Services` | `ShodanServices` | Map of detectable services. |  
| `Filters` | `ShodanHostSearchFilters` | Search filter list. |  
| `Facets` | `ShodanHostSearchFacets` | Facet list for search queries. |  
| `SubmitScan` | `ShodanScan` | Submit a scan request. |  
| `ListScans` | `ShodanScans` | List all scans. |  
| `GetScan` | `ShodanScanView` | Get status of a specific scan. |  
| `ScanInternet` | `ShodanScanInternet` | Scan the Internet for a port. |  
| `QueryList` | `ShodanQuery` | Retrieve saved search queries. |  
| `QuerySearch` | `ShodanQuerySearch` | Search within saved queries. |  
| `QueryTags` | `ShodanQueryTags` | List popular tags. |  
| `Datasets` | `ShodanData` | List downloadable datasets. |  
| `DatasetFiles` | `ShodanDataset` | Files for a dataset. |  
| `Org` | `Org` | Organization info. |  
| `AddOrgMember` | `OrgMember` | Add member to organization. |  
| `DeleteOrgMember` | `OrgMember` | Remove member from organization. |  
| `AccountProfile` | `AccountProfile` | Account profile data. |  
| `DnsResolve` | `DnsResolve` | Resolve hostnames → IPs. |  
| `DnsReverse` | `DnsReverse` | Reverse lookup of hostnames for IPs. |  
| `DnsDomain` | `DnsDomain` | Historical NS records for a domain. |  
| `HttpHeaders` | `ToolsHTTPHeaders` | HTTP headers sent by client. |  
| `MyIP` | `ToolsMyIP` | Client’s public IP. |  
| `ApiInfo` | `ApiInfo` | API plan information. |  
| `Honeyscore` | `LabsHoneyscore` | Honeypot probability score. |  
| `CreateAlert` | `ShodanAlert` | Create a network alert. |  
| `EditAlert` | `ShodanAlertId` | Edit an existing alert. |  
| `AlertInfo` | `ShodanAlertIdInfo` | Alert details. |  
| `DeleteAlert` | `ShodanAlertId` | Delete an alert. |  
| `ListAlerts` | `ShodanAlertInfo` | List all alerts. |  
| `ListTriggers` | `ShodanAlertTriggers` | List triggers for alerts. |  
| `CreateAlertTrigger` | `ShodanAlertTriggerAction` | Enable a trigger on an alert. |  
| `DeleteAlertTrigger` | `ShodanAlertTriggerAction` | Disable a trigger. |  
| `CreateTriggerIgnore` | `ShodanAlertTriggerNotificationAction` | Ignore service for a trigger. |  
| `DeleteTriggerIgnore` | `ShodanAlertTriggerNotificationAction` | Re‑enable ignored service. |  
| `ExploitSearch` | `Search` | Search exploits with facets. |  
| `ExploitCount` | `Count` | Count exploit search results. |  
| `ListNotifierProviders` | `NotifierProvider` | List notifier provider types. |  
| `ListNotifiers` | `Notifier` | List registered notifiers. |  
| `CreateNotifier` | `Notifier` | Create a new notifier. |  
| `GetNotifier` | `NotifierId` | Retrieve a notifier descriptor. |  
| `EditNotifier` | `NotifierId` | Edit an existing notifier. |  
| `DeleteNotifier` | `NotifierId` | Delete a notifier. |  
| `AddAlertNotifier` | `ShodanAlertNotifierId` | Attach a notifier to an alert. |  
| `DeleteAlertNotifier` | `ShodanAlertNotifierId` | Detach a notifier from an alert. |  
  
---  
  
## TODOs  
  
* **Search** – “todo: check: minify seems ignored” (line 30)  
  
---  
  
## Method Group Summary  
  
### Host & Search Operations  
- **Host** – fetches all services for a given IP.  
- **Count** – returns total result count for a search query.  
- **Search** – performs a full search with facets; uses `params.ToURLValues()` and calls `c.get`.  
- **SearchTokens** – token‑based breakdown of the same query.  
  
### Scan & Internet Operations  
- **SubmitScan** – posts a scan request for one or more IPs, optionally forcing.  
- **ListScans / GetScan** – list all scans and get status of a specific scan.  
- **ScanInternet** – requests an internet‑wide scan for a port/protocol pair.  
  
### Query & Dataset Operations  
- **QueryList / QuerySearch / QueryTags** – manage saved search queries, including pagination and sorting.  
- **Datasets / DatasetFiles** – list datasets and their downloadable files.  
  
### Organization & Alert Management  
- **Org / AddOrgMember / DeleteOrgMember** – organization info and member management.  
- **AccountProfile** – account plan details.  
- **DnsResolve / DnsReverse / DnsDomain** – DNS lookups for hostnames/IPs/domains.  
- **HttpHeaders / MyIP / ApiInfo / Honeyscore** – miscellaneous client utilities.  
  
### Alert & Notifier Operations  
- **CreateAlert / EditAlert / AlertInfo / DeleteAlert / ListAlerts** – CRUD on network alerts.  
- **ListTriggers / CreateAlertTrigger / DeleteAlertTrigger** – manage alert triggers.  
- **CreateTriggerIgnore / DeleteTriggerIgnore** – ignore/re‑enable services for a trigger.  
- **ListNotifierProviders / ListNotifiers / CreateNotifier / GetNotifier / EditNotifier / DeleteNotifier** – notifier lifecycle.  
- **AddAlertNotifier / DeleteAlertNotifier** – attach/detach notifiers to alerts.  
  
### Exploit Search  
- **ExploitSearch / ExploitCount** – search exploits with facets and count results.  
  
---  
  
All methods use the helper `c.get` or `c.request`/`c.requestExploits` for HTTP communication, marshal/unmarshal JSON, and return typed results.  
  
# stream.go  
# Package `shodan`  
  
## Imports  
```go  
import (  
	"bufio"  
	"bytes"  
	"context"  
	"encoding/json"  
	"errors"  
	"fmt"  
	"io"  
	"net/http"  
	"net/url"  
	"strings"  
  
	"github.com/shadowscatcher/shodan/models"  
	"github.com/shadowscatcher/shodan/routes"  
)  
```  
*Standard libraries*: `bufio`, `bytes`, `context`, `encoding/json`, `errors`, `fmt`, `io`, `net/http`, `net/url`, `strings`.    
*External packages*: `github.com/shadowscatcher/shodan/models` (provides the `Service` type) and `github.com/shadowscatcher/shodan/routes` (contains route constants such as `ShodanBanners`, `ShodanAsn`, etc.).  
  
## External data / input sources  
- **HTTP client** – passed to `GetStreamClient`; used for all requests.  
- **API key** – stored in the `apiKey` field of `StreamClient`.  
- **Route constants** – from the `routes` package; each stream method builds a URL using one of these constants.  
- **JSON payloads** – responses are decoded into `models.Service` values.  
  
## TODO comments  
No explicit `TODO:` markers were found in this file, but the following sections could be expanded:  
- Error handling in `defaultResponseHook`.  
- Adding support for query‑parameter customization beyond the API key.  
  
---  
  
# Summary of major code parts  
  
## 1. `StreamClient` struct  
```go  
type StreamClient struct {  
	apiKey string  
	HTTP   *http.Client  
	ResponseHook func(response *http.Response, err error) error  
}  
```  
*Purpose*: Holds configuration for all stream‑related HTTP calls.    
- `apiKey`: the Shodan API key.    
- `HTTP`: a reusable `*http.Client`.    
- `ResponseHook`: callback invoked after each request; defaults to `defaultResponseHook`.  
  
## 2. Constructor – `GetStreamClient`  
```go  
func GetStreamClient(key string, client *http.Client) (*StreamClient, error)  
```  
Creates and returns a fully initialized `StreamClient`. Validates that the key and client are non‑nil.  
  
## 3. Stream methods    
Each method builds a specific route (using constants from `routes`) and delegates to `subscribe`.  
  
| Method | Description |  
|--------|-------------|  
| **Banners** | Subscribes to the full banners stream (`ShodanBanners`). |  
| **ASN** | Filters by ASNs; accepts a slice of ASN strings. |  
| **Countries** | Filters by country codes; accepts a slice of country strings. |  
| **Ports** | Filters by port numbers; converts each int to string and joins them. |  
| **Alerts** | Subscribes to alerts for all IP ranges (`ShodanAlert`). |  
| **Alert** | Subscribes to a single alert identified by `alertID`. |  
| **Tags** | Filters by tags; accepts a slice of tag strings. |  
| **Vulns** | Filters by vulnerabilities; accepts a slice of vulnerability strings. |  
  
All methods return a channel of `models.Service` and an error.  
  
## 4. Helper – `defaultParams`  
```go  
func (s *StreamClient) defaultParams() url.Values  
```  
Creates the query parameters map containing only the API key (`key=s.apiKey`).    
  
## 5. Request creation – `createRequest`  
```go  
func (s *StreamClient) createRequest(ctx context.Context, uri *url.URL) (*http.Request, error)  
```  
- Encodes default params into the URL.  
- Builds an HTTP GET request with optional context.  
  
## 6. Making a request – `makeRequest`  
```go  
func (s *StreamClient) makeRequest(ctx context.Context, route string) (response *http.Response, err error)  
```  
- Concatenates base API stream path (`routes.ApiStream`) with the supplied route.  
- Parses into a URL and calls `createRequest`.  
- Executes the request via the stored HTTP client.  
  
## 7. Subscription logic – `subscribe`  
```go  
func (s *StreamClient) subscribe(ctx context.Context, route string) (chan models.Service, error)  
```  
- Creates a channel for results.  
- Calls `makeRequest` to get an HTTP response.  
- Invokes the configured `ResponseHook`.  
- Starts a goroutine that reads the response body line‑by‑line, unmarshals each JSON line into a `models.Service`, and sends it on the channel.  
  
## 8. Default response hook – `defaultResponseHook`  
```go  
func defaultResponseHook(response *http.Response, err error) error  
```  
- Checks for non‑nil error.  
- Returns an error if the status code is not 200; otherwise nil.  
  
---  
  
All together, this file implements a flexible client that can subscribe to various Shodan streams (banners, ASN, countries, ports, alerts, tags, vulns). Each stream method simply builds its route and hands off to `subscribe`, which handles HTTP communication and streaming JSON decoding.  
  
