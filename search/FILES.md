# search/domain_query.go  
**Package/Component Name**    
`search`  
  
---  
  
### Imports  
No external imports are declared in this file.  
  
### External Data / Input Sources  
The `DomainQuery` struct is intended to hold query parameters for searching a domain, including:  
- The target domain name (`Domain`)  
- Whether the search should include historical data (`History`)  
- The type of record being queried (`RecordType`)  
- Which page of results to retrieve (`Page`)  
  
### TODOs  
No TODO comments are present in this file.  
  
---  
  
## Summary of Major Code Parts  
  
#### `type DomainQuery struct`  
The core element of this file is the definition of a simple data structure used for domain queries.    
* **Fields**    
  * `Domain string` – The domain name to search.    
  * `History bool` – Flag indicating whether historical results should be included.    
  * `RecordType string` – Specifies the type of record (e.g., A, MX, TXT).    
  * `Page int` – Page number for pagination; a comment notes that pages after the first require enterprise access.  
  
This struct will likely be instantiated and passed to functions elsewhere in the `search` package to perform domain lookups or analytics.  
  
# search/exploits_query.go  
# search package  
  
## Imports    
| Import | Purpose |  
|--------|---------|  
| `fmt` | Provides formatting utilities for the `String()` method. |  
| `github.com/shadowscatcher/shodan/search/exploit_types` | Supplies the `ExploitType` type used in the struct field `Type`. |  
  
---  
  
## External data sources    
The `ExploitsQuery` struct represents a single search query that can be sent to Shodan’s exploit‑search API. Each field is annotated with a `shodan_search:"<key>"` tag, which indicates how the value will be marshalled into the HTTP request.  
  
| Field | Tag key | Description |  
|-------|---------|-------------|  
| `Text` | – | The raw search string entered by the user. |  
| `Author` | `author` | Exploit author name. |  
| `BID` | `bid` | Bugtraq ID of the exploit. |  
| `Code` | `code` | Raw code snippet of the exploit. |  
| `CVE` | `cve` | CVE identifier for the vulnerability. |  
| `Date` | `date` | Release date of the exploit. |  
| `Description` | `description` | Short description of how the exploit works. |  
| `MSB` | `msb` | Microsoft Security Bulletin ID. |  
| `OSVDB` | `osvdb` | Open‑Source Vulnerability Database ID. |  
| `Platform` | `platform` | Target operating system. |  
| `Port` | `port` | Remote service port number (if applicable). |  
| `Title` | `title` | Title or short description of the exploit. |  
| `Type` | `type` | Exploit category, defined by `exploit_types.ExploitType`. |  
  
---  
  
## TODOs    
No explicit TODO comments are present in this file.  
  
---  
  
### Struct definition – `ExploitsQuery`  
  
The struct holds all parameters that can be supplied to a Shodan exploit search. The tags map each field to the corresponding query parameter name expected by the API. All fields are exported, so they can be accessed and modified from other packages within the same module.  
  
### Method – `String() string`  
  
```go  
func (q *ExploitsQuery) String() string {  
    marshaled := marshalQueryParam(q, true, "")  
    switch {  
    case marshaled == "":  
        return q.Text  
    case q.Text == "":  
        return marshaled  
    }  
  
    return fmt.Sprintf("%q %q", q.Text, marshaled)  
}  
```  
  
* `marshalQueryParam` (assumed to be defined elsewhere) serialises the struct into a query string.    
* The method first checks whether the marshalled part is empty; if so it simply returns the user‑supplied text.    
* If the original text is empty, it falls back to the marshalled representation.    
* Otherwise both parts are combined with `fmt.Sprintf("%q %q", q.Text, marshaled)` and returned.  
  
This method allows an `ExploitsQuery` instance to be converted into a single string that can be appended to a URL or passed to an HTTP client.  
  
---  
  
# search/query.go  
**Package name**    
`search`  
  
**Imports**    
  
```go  
import (  
	"fmt"  
	"github.com/shadowscatcher/shodan/search/link_types"  
	"github.com/shadowscatcher/shodan/search/ssl_versions"  
	"reflect"  
	"strings"  
)  
```  
  
---  
  
## External data / input sources  
  
| Source | Description |  
|--------|-------------|  
| `link_types` | Provides the type used for the `Link` field in `Query`. |  
| `ssl_versions` | Supplies the SSL version enum used inside `SSLOpts.Version`. |  
| `reflect`, `strings` | Used by helper functions to marshal struct fields into a query string. |  
  
---  
  
## TODO comments  
  
No explicit `TODO:` markers were found in this file.  
  
---  
  
# Summary of major code parts  
  
### 1. `Query` struct    
The central data holder for building a search request. It contains many fields that map directly to the Shodan API parameters (e.g., `Text`, `IP`, `Geo`, `SSL`, etc.). Each field is tagged with `shodan_search:"..."` so that the marshaling helpers can read the tag name and build the query string.  
  
### 2. Nested structs    
* **SSLOpts** – SSL‑specific options such as chain count, ALPN, version, certificate and cipher details.    
* **Pubkey**, **Cipher**, **CertOptions**, **CertEntity** – nested structures that describe a public key, preferred cipher, and the full certificate chain (issuer/subject).    
* **Bitcoin** – parameters for Bitcoin‑related searches.    
* **Telnet** – options for Telnet servers.    
* **HTTP** – HTTP‑specific fields like component, status code, favicon hash, etc.    
* **NTP** – NTP server search parameters.    
* **Screenshot**, **Favicon**, **Shodan**, **SNMP**, **SSH** – additional optional blocks that can be included in the query.  
  
### 3. `marshalQueryParamField` helper    
Takes a single struct field, its value and tag name, then returns a string fragment of the form `<tag>:<value>`. It handles different Go types (`string`, `int`, `bool`) and quotes values that contain spaces.  
  
### 4. `marshalQueryParam` helper    
Iterates over all fields of an arbitrary struct (or pointer), collects each field’s tag, and concatenates the results from `marshalQueryParamField`. It supports nested structs by prefixing tags with a top‑level name when supplied.  
  
### 5. `(*Query).String()` method    
Creates a human‑readable representation of a query: it marshals all fields into a string using `marshalQueryParam`, then concatenates the raw text (`Text`) with the marshaled parameters. The result is suitable for sending to the Shodan API.  
  
---  
  
# search/reflect_types.go  
# Summary of `search` package – *types.go*  
  
## Package / Component name  
- **Package**: `search`  
  
## Imports  
```go  
import (  
	"github.com/shadowscatcher/shodan/search/exploit_types"  
	"github.com/shadowscatcher/shodan/search/link_types"  
	"github.com/shadowscatcher/shodan/search/ssl_versions"  
	"reflect"  
)  
```  
The file pulls in three internal packages (`exploit_types`, `link_types`, `ssl_versions`) and the standard library package `reflect`. These imports provide type definitions that are used to create runtime type information for marshaling.  
  
## External data / input sources  
- **`exploit_types.ExploitType`** – represents an exploit type definition.  
- **`link_types.LinkType`** – represents a link type definition.  
- **`ssl_versions.SSLVersion`** – represents an SSL version definition.  
- The Go `reflect` package is used to obtain the runtime types of these definitions.  
  
## TODO comments  
No explicit `TODO:` comments are present in this file.    
(If future work is needed, it can be added here.)  
  
---  
  
## Summary of major code parts  
  
### 1. Type variables for marshaling  
The file declares six package‑level variables that hold the Go `reflect.Type` values for various data types:  
- `stringType`: type information for a plain string.  
- `intType`: type information for an integer.  
- `boolType`: type information for a boolean.  
- `sslVersionType`: type information for an `SSLVersion` pointer dereferenced to its element type.  
- `linkTypeType`: type information for a `LinkType` pointer dereferenced to its element type.  
- `explotTypeType`: type information for an `ExploitType` pointer dereferenced to its element type.  
  
These variables are intended to be used by other parts of the package (e.g., serialization, database mapping, or API handling) where runtime type metadata is required.    
They provide a convenient way to refer to these types without repeatedly calling `reflect.TypeOf`.  
  
---  
  
# search/search_params.go  
**Package / Component**    
`search`  
  
### Imports  
```go  
import (  
	"fmt"  
	"net/url"  
	"strings"  
)  
```  
The package relies on the standard library packages `fmt`, `net/url`, and `strings`.  
  
---  
  
## External data / input sources  
| Type | Description |  
|------|-------------|  
| `Query` | A structured search query used by `Params`. The definition is expected to be in another file of the same package. |  
| `ExploitsQuery` | Query type for exploit‑specific searches, referenced by `ExploitParams`. |  
  
---  
  
## TODOs  
No explicit `TODO:` comments were found in this file.  
  
---  
  
# Summary of major code parts  
  
### 1. `Params` struct    
*Fields*:    
- `Query Query` – the core search query.    
- `Facets []string` – list of facet names (e.g., `"country:100"`).    
- `Minify bool` – flag to request minified results.    
- `Page uint`, `Offset uint` – pagination controls.  
  
*Methods*:    
- `String() string` – builds a human‑readable representation of the parameters, including query, page, offset, minify status and joined facets.    
- `ToURLValues() url.Values` – converts the struct into URL query parameters: `"query"`, optional `"facets"`, `"minify"` (if true), `"page"` (if >1) and `"offset"` (if >0).  
  
### 2. `HostParams` struct    
*Fields*:    
- `IP string` – host IP address.    
- `Minify bool`, `History bool` – flags for minification and history.  
  
*Method*:    
- `ToURLValues() url.Values` – builds a URL query map containing `"minify"` (if true) and `"history"` (if true).  
  
### 3. `ExploitParams` struct    
*Fields*:    
- `Query ExploitsQuery` – search query for exploit data.    
- `Facets []string`, `Page int`.  
  
*Method*:    
- `ToURLValues() url.Values` – similar to `Params.ToURLValues()` but only includes `"query"`, optional `"facets"` and `"page"`.  
  
These components together provide a flexible way to build search queries, host parameters, and exploit‑specific parameters for the `search` package.  
  
