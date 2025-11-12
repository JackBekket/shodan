# models/domain.go  
## Package / Component Name    
**models**  
  
---  
  
### Imports    
No explicit imports are present in this file; it relies solely on the Go standard library for struct tags.  
  
### External Data / Input Sources    
The structs are designed to be serialized/deserialized via JSON, as indicated by the struct field tags. They can serve as data models for:  
- DNS records (`NsRecord`)  
- Domain metadata and associated subdomains (`Domain`)  
  
---  
  
## TODOs    
No `TODO:` comments were found in this file.  
  
---  
  
## Summary of Major Code Parts    
  
### 1. `NsRecord` Struct    
| Field | Type | JSON Tag | Description |  
|-------|------|----------|-------------|  
| `Subdomain` | `string` | `"subdomain"` | The sub‑domain name (e.g., “www”). |  
| `Type` | `string` | `"type"` | Record type, typically DNS record type such as "A", "CNAME". |  
| `Value` | `string` | `"value"` | The value of the DNS record. |  
| `LastSeen` | `string` | `"last_seen"` | Timestamp or date when this record was last observed. |  
  
This struct represents a single DNS record entry that can be part of a domain’s data set.  
  
### 2. `Domain` Struct    
| Field | Type | JSON Tag | Description |  
|-------|------|----------|-------------|  
| `More` | `bool` | `"more"` | Flag indicating if there are more records beyond the current page (useful for pagination). |  
| `Domain` | `string` | `"domain"` | The domain name (e.g., “example.com”). |  
| `Tags` | `[]string` | `"tags"` | Arbitrary tags attached to the domain. |  
| `Data` | `[]NsRecord` | `"data"` | Slice of DNS records belonging to this domain. |  
| `Subdomains` | `[]string` | `"subdomains"` | List of sub‑domain names for quick reference. |  
  
The `Domain` struct aggregates all information about a domain, including its DNS records and related metadata.  
  
---  
  
### Overall Purpose    
This file defines the core data models used by the *models* package: an individual DNS record (`NsRecord`) and a composite domain object (`Domain`). These structs are intended for JSON marshaling/unmarshaling when interacting with external services or databases that provide domain and DNS information.  
  
# models/exploit.go  
**Package / Component**    
`models.Exploit`  
  
---  
  
### Imports  
No external imports are required for this file – it only defines a Go struct in the `models` package.  
  
---  
  
## External Data & Input Sources  
  
| Field | JSON key | Description |  
|-------|----------|-------------|  
| `Id` | `_id` | Unique identifier for the exploit/vulnerability. |  
| `Author` | `author` | Exploit author. |  
| `BID` | `bid` | Bugtraq ID. |  
| `Code` | `code` | Raw code of the exploit. |  
| `CVE` | `cve` | List of CVE identifiers. |  
| `Date` | `date` | Release date. |  
| `Description` | `description` | Exploit description and target details. |  
| `Source` | `source` | Data source (e.g., CVE, ExploitDB, Metasploit). |  
| `MSB` | `msb` | Microsoft Security Bulletin ID. |  
| `OSVDB` | `osvdb` | Open Source Vulnerability Database ID. |  
| `Platform` | `platform` | Target OS/architecture (aix, cgi, freebsd, …). |  
| `Port` | `port` | Remote service port number. |  
| `Title` | `title` | Short title or description. |  
| `Type` | `type` | Exploit type. |  
| `Alias` | `alias` | Optional alias field. |  
| `Rank` | `rank` | Ranking of the exploit. |  
| `Arch` | `arch` | Architecture information. |  
| `Privileged` | `privileged` | Boolean flag for privileged status. |  
| `Version` | `version` | Exploit version. |  
  
---  
  
## TODOs  
  
No explicit `TODO:` comments are present in this file, but the following items could be considered for future work:  
  
1. Add validation logic for required fields (`Id`, `Author`, etc.).    
2. Implement JSON marshaling/unmarshaling helpers if needed.    
3. Consider adding methods to compute a unique key or hash of the exploit.  
  
---  
  
## Summary of Major Code Parts  
  
### 1. Package Declaration  
```go  
package models  
```  
Defines that this file belongs to the `models` package, which likely contains data structures for application domain entities.  
  
### 2. Struct Definition – `Exploit`  
The core of the file is a single struct named `Exploit`. It represents an exploit/vulnerability record with many fields that map directly to JSON keys used in persistence or API communication.  
  
#### Field Breakdown  
- **Id** (`interface{}`): Generic ID field; can hold any type (string, int, etc.).    
- **Author**, **BID**, **Code**, **CVE**, **Date**, **Description**, **Source**, **MSB**, **OSVDB**, **Platform**, **Port**, **Title**, **Type**: All are straightforward data holders with JSON tags.    
- **Alias**, **Rank**, **Arch**, **Privileged**, **Version**: Additional metadata fields.  
  
The struct uses Go's `json` struct tags to map each field to a specific key in serialized JSON, enabling seamless encoding/decoding when interacting with databases or APIs.  
  
### 3. Field Types & Comments  
- Most fields are typed as `interface{}` or simple primitives (`string`, `int`, `bool`).    
- The comment block above the struct provides a concise description of each field’s purpose and expected values, which aids maintainers in understanding the data model.  
  
---  
  
**End of output**  
  
# models/host.go  
**Package / Component**    
`models`  
  
---  
  
### Imports  
No explicit import statements are present in this file; all types used are defined within the same package or built‑in Go types.  
  
---  
  
### External Data & Input Sources  
| Field | Type | JSON key | Description |  
|-------|------|----------|-------------|  
| `Ports` | `[]int` | `"ports"` | List of open ports on the host. |  
| `Vulns` | `[]string` | `"vulns"` | Vulnerability identifiers for the host. |  
| `LastUpdate` | `string` | `"last_update"` | Timestamp of the last update to this record. |  
| `Services` | `[]*Service` | `"data"` | Slice of pointers to `Service` structs (defined elsewhere in the package). |  
| `Location` | embedded struct | – | Geographic location information for the host. |  
| `HostInfo` | embedded struct | – | Common fields shared by `/host/{ip}` and `/host/search` endpoints. |  
  
The embedded `HostInfo` struct contains:  
- `IPstr` (`"ip_str"`): IP address as a string.  
- `ASN` (`"asn,omitempty"`): Autonomous system number (optional).  
- `OS` (`"os"`): Operating system name.  
- `Org` (`"org"`): Organization owning the IP space.  
- `ISP` (`"isp"`): ISP providing the organization with IP space.  
- `Hostnames` (`"hostnames"`): All host names assigned to this IP.  
- `Tags` (`"tags,omitempty"`): Tags describing device characteristics.  
- `HTML` (`"html,omitempty"`): Raw HTML response.  
  
---  
  
### TODOs  
No explicit `TODO:` comments are present in the file.  
  
---  
  
## Summary of Major Code Parts  
  
#### 1. `Host` struct definition    
The top‑level struct aggregates all host‑related data: ports, vulnerabilities, last update timestamp, services, and location/host info. It is annotated for JSON serialization with keys matching API responses (`"ports"`, `"vulns"`, `"last_update"`, `"data"`). The embedded `Location` and `HostInfo` structs allow direct access to those fields without nesting.  
  
#### 2. Embedded `HostInfo` struct    
Provides a concise representation of host metadata that is reused across multiple API endpoints (`/host/{ip}` and `/host/search`). Each field has a clear JSON tag, ensuring proper marshaling/unmarshaling when interacting with external services or databases.  
  
---  
  
# models/location.go  
**Package / Component Name**    
`models`  
  
---  
  
### Imports  
No external imports are declared in this file; the struct relies solely on Go’s built‑in types and JSON tags for serialization.  
  
### External Data & Input Sources  
The `Location` struct is designed to capture all geolocation details of a device. It expects data that can be marshalled/unmarshalled from/to JSON, with fields such as latitude, longitude, city name, country codes (2‑letter, 3‑letter), country name, area code, region code, DMA code, and postal code.  
  
### TODOs  
No `TODO` comments are present in the current file.  
  
---  
  
## Summary of Major Code Parts  
  
#### Struct Definition: `Location`  
- **Purpose**: Holds comprehensive location information for a device.  
- **Fields**:  
  - `Latitude *float32`: Latitude coordinate (JSON key `"latitude"`).  
  - `Longitude *float32`: Longitude coordinate (JSON key `"longitude"`).  
  - `City *string`: City name (JSON key `"city"`).  
  - `CountryCode *string`: Two‑letter country code (JSON key `"country_code"`).  
  - `CountryCode3 *string`: Three‑letter country code (JSON key `"country_code3"`).  
  - `CountryName *string`: Full country name (JSON key `"country_name"`).  
  - `AreaCode *int`: US area code, if applicable (JSON key `"area_code"`).  
  - `RegionCode *string`: Region identifier (JSON key `"region_code"`).  
  - `DmaCode *int`: Designated market area code for the US (JSON key `"dma_code"`).  
  - `PostalCode *string`: Postal/ZIP code (JSON key `"postal_code"`).  
  
The struct is fully annotated with JSON tags, making it ready for encoding/decoding in API calls or database interactions.  
  
---  
  
# models/notifications.go  
# Package / Component    
**models**  
  
## Imports  
```go  
import (  
	"fmt"  
	"io"  
	"net/url"  
	"strings"  
)  
```  
The file pulls in the standard library packages `fmt`, `io`, `net/url` and `strings`.  
  
---  
  
## External Data & Input Sources    
| Type | Description |  
|------|-------------|  
| `NotifierProviderType` | Alias for a string that represents the provider kind (email, gitter, pagerduty, slack, telegram, webhook, phone). |  
| `ProviderArgs` | Map of key/value pairs used to build request bodies. |  
  
The file also references an external type `SimpleResponse`, which is expected to be defined elsewhere in the same package.  
  
---  
  
## TODOs    
No explicit `TODO:` comments are present; this section can be expanded later if needed.  
  
---  
  
## Summary of Major Code Parts    
  
### 1. Constants for Provider Types    
```go  
const (  
	EmailProviderType     NotifierProviderType = "email"  
	GitterProviderType    NotifierProviderType = "gitter"  
	PagerdutyProviderType NotifierProviderType = "pagerduty"  
	SlackProviderType     NotifierProviderType = "slack"  
	TelegramProviderType  NotifierProviderType = "telegram"  
	WebhookProviderType   NotifierProviderType = "webhook"  
	PhoneProviderType     NotifierProviderType = "phone"  
)  
```  
Defines the supported provider kinds as string constants.  
  
### 2. Core Data Structures    
* `NotifierProvider` – holds a description, type and arguments for a notification provider.    
* `NotifierResponse` – extends an external `SimpleResponse` with an ID field.    
* `NotifierDescriptor` – JSON‑serialisable descriptor used when listing providers.    
* `NotifierList` – wrapper for a list of descriptors plus a total count.    
* `ProviderRequirements` – helper struct that lists required fields (currently unused in the code).  
  
### 3. Request Body Builder    
```go  
func (p NotifierProvider) ToRequestBody() io.Reader {  
	values := make(url.Values)  
	if p.Description != "" {  
		values.Set("description", p.Description)  
	}  
	values.Set("provider", string(p.ProviderType))  
  
	for key, value := range p.ProviderArgs {  
		values.Set(key, value)  
	}  
  
	return strings.NewReader(values.Encode())  
}  
```  
Creates a URL‑encoded request body from a `NotifierProvider`. It populates the description and provider type, then iterates over all arguments to add them as query parameters.  
  
### 4. Convenience Constructors    
Each of the following functions creates a `NotifierProvider` for a specific provider kind by delegating to the generic `CreateProvider` helper:  
  
| Function | Parameters | Provider Type |  
|----------|------------|---------------|  
| `CreateEmailProvider(to, description string)` | `to`, `description` | Email |  
| `CreatePhoneProvider(to, description string)` | `to`, `description` | Phone |  
| `CreatePagerdutyProvider(routingKey, description string)` | `routing_key`, `description` | PagerDuty |  
| `CreateGitterProvider(roomID, token, description string)` | `room_id`, `token`, `description` | Gitter |  
| `CreateSlackProvider(webhookURL, description string)` | `webhook_url`, `description` | Slack |  
| `CreateTelegramProvider(chatID, token, description string)` | `chat_id`, `token`, `description` | Telegram |  
| `CreateWebhookProvider(URL, description string)` | `url`, `description` | Webhook |  
  
Each function simply calls `CreateProvider` with the appropriate constant and a map of required arguments.  
  
### 5. Generic Provider Builder    
```go  
func CreateProvider(t NotifierProviderType, description string, requiredArgs ProviderArgs) (p NotifierProvider, err error) {  
	for argName, arg := range requiredArgs {  
		if arg == "" {  
			err = fmt.Errorf("%s is required", argName)  
		}  
	}  
  
	p.ProviderType = t  
	p.Description = description  
	p.ProviderArgs = requiredArgs  
  
	return  
}  
```  
Validates that all supplied arguments are non‑empty (though the error handling could be improved), assigns the provider type, description and argument map to a new `NotifierProvider`, and returns it.  
  
---  
  
All of these pieces together allow the package to construct notification providers for various services, serialize them into request bodies, and expose helper constructors for each provider kind.  
  
# models/search_result.go  
**Package / Component**    
`models`  
  
---  
  
### Imports  
No external imports are declared in this file; all types referenced (`Service`, `Exploit`, `Facet`) are assumed to be defined elsewhere within the same package or imported implicitly by other files.  
  
### External Data / Input Sources  
* The comment indicates that these structs represent responses from two HTTP endpoints:  
  * `"host/search"` – returns a list of services matched by a search query.  
  * `"host/search/count"` – returns a count and facet information for the same query.  
  
---  
  
## TODOs  
No `TODO` comments are present in this file.  
  
---  
  
## Summary of Major Code Parts  
  
### `SearchResult`  
* **Purpose** – Holds the result set returned from the `"host/search"` endpoint.  
* **Fields**  
  * `Matches []Service` – slice of matched services; JSON key `"matches"`.  
  * `Total int` – total number of matches; JSON key `"total"`.  
  * `Facets map[string][]Facet` – optional facet data keyed by string; JSON key `"facets,omitempty"`.  
  
### `ExploitResult`  
* **Purpose** – Holds the result set returned from the `"host/search/count"` endpoint.  
* **Fields**  
  * `Matches []Exploit` – slice of matched exploits; JSON key `"matches"`.  
  * `Total int` – total number of matches; JSON key `"total"`.  
  * `Facets map[string][]Facet` – optional facet data keyed by string; JSON key `"facets,omitempty"`.  
  
### `Facet`  
* **Purpose** – Represents a single facet entry used in both result structs.  
* **Fields**  
  * `Count int` – number of items for this facet; JSON key `"count"`.  
  * `Value interface{}` – generic value (could be string, int, etc.); JSON key `"value"`.  
  
These three types together provide a lightweight data model for handling search results and associated facets in the application’s host‑search domain.  
  
# models/service.go  
## Package & Imports  
- **Package**: `models`  
- **Imports**:  
  - `fmt` – for string formatting in the helper methods.  
  - `github.com/shadowscatcher/shodan/models/services` – provides concrete service types (Cassandra, DB2, DNS, etc.) that are embedded into the `Service` struct.  
  
---  
  
## Service Struct Overview  
| Field | Type | JSON tag | Purpose |  
|-------|------|----------|---------|  
| `HostInfo` | embedded | – | Inherits host‑level information (IP, hostname, etc.). |  
| `Location` | `Location` | `json:"location"` | Physical or logical location of the host. |  
| `Data` | `string` | `json:"data"` | Raw banner data captured from the service. |  
| `IP` | `*int` | `json:"ip,omitempty"` | Integer representation of the IPv4 address (optional). |  
| `IPv6` | `*string` | `json:"ipv6,omitempty"` | String representation of the IPv6 address (optional). |  
| `Port` | `int` | `json:"port"` | Service port number. |  
| `Timestamp` | `string` | `json:"timestamp"` | UTC timestamp when the banner was captured. |  
| `Hash` | `int` | `json:"hash"` | Numeric hash of the `Data` field. |  
| `Domains` | `[]string` | `json:"domains"` | Top‑level domains for hostnames (useful for TLD filtering). |  
| `Link` | `*string` | `json:"link,omitempty"` | Network link type description. |  
| `Opts` | `map[string]interface{}` | `json:"opts"` | Experimental/supplemental data (SSL certs, robots.txt, etc.). |  
| `Uptime` | `*int` | `json:"uptime,omitempty"` | Uptime of the IP in minutes. |  
| `Transport` | `string` | `json:"transport"` | Transport protocol (“udp” or “tcp”). |  
| `Product` | `interface{}` | `json:"product,omitempty"` | Name of the software running the service (may be string or number). |  
| `Version` | `interface{}` | `json:"version,omitempty"` | Version of the software (string or number). |  
| `CPE` | `interface{}` | `json:"cpe,omitempty"` | Common Platform Enumeration – can be a single string or slice. |  
| `Title` | `*string` | `json:"title,omitempty"` | Website title extracted from HTML source. |  
| `DeviceType` | `*string` | `json:"devicetype,omitempty"` | Device type (webcam, router, etc.). |  
| `Info` | `*string` | `json:"info,omitempty"` | Miscellaneous product information. |  
| `Shodan` | `Shodan` | `json:"_shodan"` | Metadata about how the banner was generated. |  
| `Vulns` | `map[string]Vulnerability` | `json:"vulns,omitempty"` | Vulnerability data inferred from the banner. |  
| `SSL` | `*SSL` | `json:"ssl,omitempty"` | SSL/TLS information for services that support it. |  
| `Cassandra` | `*services.Cassandra` | `json:"cassandra,omitempty"` | Cassandra database service details. |  
| `DB2` | `*services.DB2` | `json:"db2,omitempty"` | IBM DB2 DRDA protocol data. |  
| `DNS` | `*services.DNS` | `json:"dns,omitempty"` | DNS server information (UDP/TCP). |  
| `Docker` | `*services.Docker` | `json:"docker,omitempty"` | Docker service details. |  
| `Elastic` | `*services.Elastic` | `json:"elastic,omitempty"` | Elastic search service data. |  
| `Etcd` | `*services.Etcd` | `json:"etcd,omitempty"` | etcd key/value store information. |  
| `EthernetIP` | `*services.EthernetIP` | `json:"ethernetip,omitempty"` | Industrial Ethernet/IP handshake data. |  
| `FTP` | `*services.FTP` | `json:"ftp,omitempty"` | FTP service details (port 21/TCP). |  
| `Hive` | `*services.Hive` | `json:"hive,omitempty"` | Apache Hive server data. |  
| `HTTP` | `*services.HTTP` | `json:"http,omitempty"` | HTTP module banner information. |  
| `ISAKMP` | `*services.ISAKMP` | `json:"isakmp,omitempty"` | ISAKMP VPN service data. |  
| `Lantronix` | `*services.Lantronix` | `json:"lantronix,omitempty"` | Lantronix configuration service data. |  
| `Monero` | `*services.Monero` | `json:"monero,omitempty"` | Monero RPC service data (port 18081). |  
| `MongoDB` | `*services.Mongo` | `json:"mongodb,omitempty"` | MongoDB binary protocol data. |  
| `MQTT` | `*services.MQTT` | `json:"mqtt,omitempty"` | MQTT service details. |  
| `Netbios` | `*services.Netbios` | `json:"netbios,omitempty"` | NetBIOS handshake data (port 137). |  
| `NTP` | `*services.NTP` | `json:"ntp,omitempty"` | NTP daemon data. |  
| `Redis` | `*services.Redis` | `json:"redis,omitempty"` | Redis service details (port 6379/TCP). |  
| `RIP` | `*services.RIP` | `json:"rip,omitempty"` | RIP request response data (port 520). |  
| `Rsync` | `*services.Rsync` | `json:"rsync,omitempty"` | Rsync service data. |  
| `SMB` | `*services.SMB` | `json:"smb,omitempty"` | SMBv1/v2 service data (port 445). |  
| `SNMP` | `*services.SNMP` | `json:"snmp,omitempty"` | SNMP module banner data (port 161/UDP). |  
| `SSH` | `*services.SSH` | `json:"ssh,omitempty"` | SSH handshake data. |  
| `Vertx` | `*services.Vertx` | `json:"vertx,omitempty"` | VertX/Edge door controller data. |  
| `Minecraft` | `*services.Minecraft` | `json:"minecraft"` | Minecraft game server data. |  
| `InfluxDb` | `*services.InfluxDb` | `json:"influx_db"` | InfluxDB time‑series database data. |  
| `CoAP` | `*services.CoAP` | `json:"coap"` | CoAP IoT protocol service data. |  
  
---  
  
## Methods Overview  
- **ProductString()** – Returns a string representation of the `Product` field (handles nil case).  
- **CpeList()** – Normalises the `CPE` interface{} into a slice of strings; supports both single string and slice types.  
- **VersionString()** – Similar to `ProductString`, but for the `Version` field.  
- **IPString()** – Returns the IPv4 or IPv6 address as a string. If `IP` is present it returns `s.IPstr`; otherwise it dereferences `IPv6`. (Note: `IPstr` appears to be an implicit field from embedded `HostInfo`.)  
- **IpAndPort()** – Concatenates the IP string and port into a single “IP:port” representation.  
  
---  
  
## TODOs  
No explicit TODO comments are present in this file; all functionality is already implemented.  
  
---  
  
# models/shodan.go  
**Package/Component Name**    
`models`  
  
---  
  
### Imports  
No external packages are imported in this file; all types and fields are defined locally within the `models` package.  
  
---  
  
### External Data / Input Sources  
* The struct `Shodan` represents a crawler instance that collects data for a banner.    
  * `Crawler` – unique identifier of the crawler (JSON key `"crawler"`).    
  * `Id` – pointer to a string containing the banner ID (JSON key `"id"`).    
  * `Module` – optional name of the Shodan module used by the crawler (JSON key `"module,omitempty"`).    
  * `Ptr` – boolean flag, purpose not documented in comments.    
  * `Options` – nested struct `CrawlerOptions`, holding configuration details for the crawl.  
  
* The nested struct `CrawlerOptions` contains:    
  * `Hostname` – host name used when sending web requests (JSON key `"hostname,omitempty"`).    
  * `Referrer` – ID of the banner that triggered the scan for this port (JSON key `"referrer,omitempty"`).    
  * `Scan` – ID of the scan itself (JSON key `"scan,omitempty"`).  
  
---  
  
### TODOs  
No explicit TODO comments are present in the provided code.  
  
---  
  
## Summary of Major Code Parts  
  
#### 1. **Shodan struct**  
Defines the core data model for a Shodan crawler instance, including identifiers and configuration options. The fields are annotated with JSON tags to facilitate marshaling/unmarshaling when interacting with external systems (e.g., REST APIs or database persistence).  
  
#### 2. **CrawlerOptions struct**  
Encapsulates configuration parameters that influence how the Shodan crawler operates: hostname for web requests, referrer ID linking back to a banner, and scan ID tying the data together.  
  
---  
  
These two structs form the foundation of the `models` package, providing a lightweight representation of crawler configurations and their associated metadata.  
  
# models/ssl.go  
**Package / Component**    
`models`  
  
---  
  
### Imports  
```go  
import (  
	"encoding/json"  
)  
```  
The package relies on the standard `encoding/json` library for marshaling/unmarshaling of the defined structs.  
  
---  
  
## External data, input sources  
All structs in this file are annotated with JSON tags. They represent a complete SSL configuration that can be read from or written to a JSON document.    
Typical usage:    
* Load an existing SSL profile into `SSL` struct.    
* Persist changes back to JSON for storage or transmission.  
  
---  
  
## TODOs  
No explicit `TODO:` comments were found in the source file.  
  
---  
  
# Summary of major code parts  
  
### 1️⃣ `SSL`  
Represents a full SSL configuration:  
| Field | Type | JSON key | Notes |  
|-------|------|----------|-------|  
| `AcceptableCAs` | []SslAcceptableCA | `"acceptable_cas"` | List of CA certificates that the server accepts. |  
| `Alpn` | []string | `"alpn"` | Supported HTTP/2.0 versions (e.g., “h2”). |  
| `Cert` | SslCert | `"cert"` | Parsed certificate details. |  
| `Chain` | []string | `"chain"` | PEM chain of leaf and root certificates. |  
| `Cipher` | SslCipher | `"cipher"` | Preferred cipher suite. |  
| `DHparams` | *SslDHParams | `"dhparams,omitempty"` | Diffie‑Hellman parameters (optional). |  
| `TLSExt` | []SslTlsExt | `"tlsext"` | Supported TLS extensions. |  
| `Unstable` | []string | `"unstable,omitempty"` | Optional unstable features list. |  
| `Versions` | []string | `"versions"` | List of supported SSL versions (e.g., “-SSLv2”). |  
  
### 2️⃣ `SslAcceptableCA`  
Describes a single acceptable CA:  
* `Components`: nested certificate components.  
* `Hash`: numeric hash value.  
* `Raw`: raw string representation.  
  
### 3️⃣ `SslCert`  
Holds the main certificate data:  
* Boolean flags, expiration dates, extensions, fingerprint, issuer, public key, serial number, etc.    
Fields are tagged for JSON serialization.  
  
### 4️⃣ `SslCertComponents`  
Reusable component block used by both `SslCert` and `SslIssuer`.    
Contains common fields such as CN, O, OU, SN, ST, email address, serial number, etc.  
  
### 5️⃣ `SslCipher`  
Specifies cipher suite details:  
* Bits, name, version.  
  
### 6️⃣ `SslDHParams`  
Optional Diffie‑Hellman parameters:  
* Bits, fingerprint, generator (interface{}), prime, public key.  
  
### 7️⃣ `SslExtension`  
Represents a single TLS extension:  
* Critical flag, data string, name.  
  
### 8️⃣ `SslFingerprint`  
Two hash fields for SHA1 and SHA256 fingerprints.  
  
### 9️⃣ `Pubkey`  
Public key metadata: bits and type.  
  
### 10️⃣ `SslSubject`  
Extends `SslCertComponents` with business‑category, description, jurisdiction, postal code, street, etc.  
  
### 11️⃣ `SslIssuer`  
Also extends `SslCertComponents`; adds issuer name, UID, DN qualifier, subject alternative name, unstructured address, and an optional “UNDEF” field.  
  
### 12️⃣ `SslTlsExt`  
Simple TLS extension descriptor: ID and name.  
  
---  
  
All structs are designed to be marshaled/unmarshaled with JSON tags that match the expected keys in a configuration file. The hierarchy allows nested components (subject/issuer) while keeping the code modular.  
  
# models/utility.go  
**Package / Component Name**    
`models`  
  
---  
  
### Imports  
The file does not import any external packages; it only declares a set of Go struct types that represent data structures used throughout the application.  
  
---  
  
### External Data & Input Sources  
All structs in this file are designed to map JSON responses from various HTTP endpoints. The key sources include:  
  
| Endpoint | Purpose | Struct(s) |  
|----------|---------|-----------|  
| `/scan` | Single scan result | `Scan` |  
| `/scans` | List of scans | `ScanList`, `ScanReport` |  
| `/account/profile` | User profile data | `Profile` |  
| `/api-info` | API usage statistics | `ApiInfo` |  
| `/org` | Organization details | `Org`, `OrgMember` |  
| Search queries | Query metadata | `Tokens`, `SearchQueries`, `SearchQuery`, `QueryTags`, `QueryTag` |  
| Dataset handling | Dataset & file info | `Dataset`, `DatasetFile` |  
| Alerts | Network alert configuration | `Filter`, `Alert`, `AlertDetails`, `Trigger` |  
  
These structs are used throughout the application to marshal/unmarshal JSON payloads when communicating with a REST API.  
  
---  
  
### TODO Comments  
No explicit `TODO:` comments were found in this file.    
  
---  
  
## Summary of Major Code Parts  
  
#### 1. Basic Response & Scan Structures    
- **Error** – generic error container.    
- **SimpleResponse** – success flag for simple calls.    
- **Scan**, **ScanList**, **ScanReport** – represent scan data, including ID, count, credits left, status, and a list of individual scan reports.  
  
#### 2. Account & API Information    
- **Profile** – user profile details (member flag, credits, display name, creation timestamp).    
- **ApiInfo** – overall API usage statistics such as scan credits, usage limits, plan type, HTTPS support, etc.  
  
#### 3. Organization Data    
- **Org**, **OrgMember** – organization metadata with admins and members lists, upgrade type, domains, logo placeholder.  
  
#### 4. Search Query Structures    
- **Tokens** – tokenized search query representation (attributes map, errors slice, string, filters).    
- **SearchQueries**, **SearchQuery** – collection of saved queries and individual query details.    
- **QueryTags**, **QueryTag** – tags associated with queries.  
  
#### 5. Dataset & File Metadata    
- **Dataset** – dataset description fields.    
- **DatasetFile** – file information within a dataset (URL, timestamp, name, size).  
  
#### 6. Alert Configuration and Details    
- **Filter** – IP filter list for alerts.    
- **Alert** – top‑level alert definition with name, filter, optional expiration.    
- **AlertDetails** – extended alert data including creation/expiration timestamps, ID, and trigger map.    
- **Trigger** – individual trigger configuration (name, rule, description).  
  
These structs collectively form the core data model for the application’s API interactions.  
  
# models/vuln.go  
## Package / Component    
**models**  
  
### Imports    
No external imports are required for this file.  
  
### External Data & Input Sources    
| Field | JSON Tag | Description |  
|-------|----------|-------------|  
| `CVSS` | `cvss` | Holds the Common Vulnerability Scoring System value (generic interface). |  
| `References` | `references` | Slice of URLs related to the vulnerability. |  
| `Summary` | `summary` | Short textual description of the vulnerability. |  
| `Verified` | `verified` | Boolean flag indicating if the vulnerability has been verified by Shodan crawlers. |  
  
### TODOs    
No TODO comments are present in this file.  
  
## Summary of Major Code Parts    
  
- **Struct Definition** – The `Vulnerability` struct encapsulates all data needed to describe a security issue, including scoring, references, summary text, and verification status.  
- **Field Annotations** – Each field is annotated with JSON tags so that the struct can be marshalled/unmarshalled directly from/to JSON payloads.    
- **Data Types** – `CVSS` uses an empty interface to allow flexibility in storing any type of CVSS data; `References` is a slice of strings for URLs, while `Summary` and `Verified` are straightforward string/boolean fields.  
  
This concise struct serves as the core model for vulnerability records within the `models` package.  
  
