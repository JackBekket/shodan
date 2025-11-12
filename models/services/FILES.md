# models/services/cassandra.go  
## Package / Component    
**services**  
  
### Imports  
No external imports are required for this file.  
  
### External Data / Input Sources    
| Field | JSON Tag | Description |  
|-------|----------|-------------|  
| `Name` | `"name"` | Name of the Cassandra cluster |  
| `Keyspaces` | `"keyspaces"` | List of keyspaces available in the cluster |  
| `Partitioner` | `"partitioner"` | Algorithm used to partition data across nodes |  
| `Snitch` | `"snitch"` | Algorithm used to determine network topology |  
  
### TODOs  
No TODO comments are present in this file.  
  
---  
  
## Summary of Major Code Parts    
  
#### Struct Definition    
The file defines a single Go struct, **Cassandra**, which represents the configuration and state of a Cassandra cluster. The struct contains four fields that map directly to JSON keys for easy serialization/deserialization:  
  
1. **Name** – holds the cluster name.  
2. **Keyspaces** – an array of strings listing all keyspaces in the cluster.  
3. **Partitioner** – specifies which partitioning algorithm is used (e.g., Murmur3, Random).  
4. **Snitch** – indicates the network topology algorithm (e.g., RackAwareSnitch).  
  
The struct is straightforward and serves as a data model for higher‑level services that will read/write Cassandra cluster information.  
  
---  
  
# models/services/coap.go  
## Package / Component    
**services**  
  
### Imports    
No external imports are declared in this file.  
  
### External Data / Input Sources    
The `CoAP` struct contains a single field, `Resources`, which is a map from string keys to arbitrary values (`interface{}`). This field is intended to hold configuration or runtime data that can be serialized/deserialized via JSON (as indicated by the struct tag).  
  
### TODOs    
No TODO comments are present in this file.  
  
---  
  
## Summary of Major Code Parts    
  
### 1. `CoAP` Struct Definition    
- **Purpose**: Represents a CoAP (Constrained Application Protocol) service configuration or state holder within the `services` package.    
- **Field**:    
  - `Resources map[string]interface{}` – A flexible key/value store that can be marshaled to/from JSON, allowing dynamic addition of resources at runtime.    
- **JSON Tag**: The struct tag ``json:"resources"`` ensures that when this struct is serialized to JSON, the field will appear as `"resources"`.  
  
This file provides a minimal yet extensible foundation for CoAP-related services; future extensions may add methods or additional fields to enrich functionality.  
  
# models/services/db2.go  
**Package / Component Name**    
`services`  
  
---  
  
### Imports  
No explicit import statements are present in this file; the struct relies solely on Go's built‑in types and JSON tags for serialization.  
  
---  
  
### External Data & Input Sources  
The `DB2` struct is designed to be populated from external JSON data. Each field carries a JSON tag that maps the struct fields to corresponding keys when marshaling/unmarshaling:  
- `ExternalName` → `"external_name"`  
- `ServerPlatform` → `"server_platform"`  
- `InstanceName` → `"instance_name"`  
- `Version` → `"db2_version"`  
  
These tags indicate that the struct is intended for use with JSON payloads, likely coming from a configuration file or an API response.  
  
---  
  
### TODO Comments  
No TODO comments are present in this snippet. If future enhancements are required (e.g., validation, default values), they can be added as `// TODO:` notes within the struct definition or elsewhere in the package.  
  
---  
  
## Summary of Major Code Parts  
  
| Section | Description |  
|---------|-------------|  
| **Package Declaration** | Declares that this file belongs to the `services` package. |  
| **Struct Definition (`DB2`)** | Holds metadata for a DB2 database server, including external name, platform, instance name, and version. The struct is annotated with JSON tags for easy serialization. |  
  
---  
  
This concise file defines a single data structure used within the `services` component of the larger codebase. It serves as a foundational type that other parts of the package can instantiate, populate from JSON, or persist to storage.  
  
# models/services/dns.go  
## Package / Component    
**services**  
  
### Imports    
None – the file only declares a type.  
  
### External Data / Input Sources    
No external data or input sources are referenced in this snippet; it simply defines a Go struct that can be used elsewhere in the package.  
  
### TODOs    
There are no `TODO` comments present in the code.  
  
## Summary of Major Code Parts    
  
#### Struct Definition    
```go  
type DNS struct {  
    Recursive          bool   `json:"recursive"`  
    ResolverHostname   *string `json:"resolver_hostname"`  
    ResolverID         *string `json:"resolver_id"`  
    Software           *string `json:"software"`  
}  
```  
* **Purpose** – The `DNS` type represents a DNS resolver configuration.    
  - `Recursive`: indicates whether the server performs recursive lookups.    
  - `ResolverHostname`: pointer to the hostname of the resolver used for queries.    
  - `ResolverID`: pointer to a unique identifier for that resolver.    
  - `Software`: pointer to the name of the DNS software in use.  
  
#### Field Descriptions    
| Field | Type | JSON tag | Notes |  
|-------|------|----------|-------|  
| `Recursive` | `bool` | `"recursive"` | Boolean flag for recursive capability. |  
| `ResolverHostname` | `*string` | `"resolver_hostname"` | Optional hostname; pointer allows nil values. |  
| `ResolverID` | `*string` | `"resolver_id"` | Optional unique ID; pointer allows nil values. |  
| `Software` | `*string` | `"software"` | Optional software name; pointer allows nil values. |  
  
The struct is ready to be marshalled/unmarshalled with JSON tags, making it convenient for configuration or API payloads within the `services` package.  
  
# models/services/docker.go  
**Package / Component name**    
`services`  
  
---  
  
### Imports  
No explicit imports are declared in the file; all types rely solely on Go’s built‑in packages and JSON tags.  
  
### External data & input sources  
The structs expose a set of fields that map directly to JSON keys. These keys will be used when serializing/deserializing Docker service information, e.g., via `encoding/json`.    
Key external data points include:  
  
| Struct | Field | JSON key | Notes |  
|--------|-------|----------|-------|  
| `Docker` | `APIVersion`, `Arch`, `BuildTime`, … | `ApiVersion`, `Arch`, `BuildTime`, … | Top‑level service metadata |  
| `DockerComponent` | `Details`, `Name`, `Version` | `Details`, `Name`, `Version` | Component description |  
| `DockerContainer` | `Command`, `Created`, `FinishedAt`, … | `Command`, `Created`, `FinishedAt`, … | Container runtime data |  
| `DockerComponentDetails` | `APIVersion`, `Arch`, … | `ApiVersion`, `Arch`, … | Detailed component metadata |  
| `DockerContainerHostConfig` | `NetworkMode` | `NetworkMode` | Host configuration for a container |  
| `DockerPlatform` | `Name` | `Name` | Platform name |  
  
### TODO comments  
No TODO markers are present in the file.  
  
---  
  
## Summary of major code parts  
  
### `Docker`  
Represents an entire Docker service instance.    
Fields cover API version, architecture, build time, component list, container list, engine type, experimental flag, Git commit hash, Go and kernel versions, minimum API support, Euler version, OS, package version, platform details, and overall version. The struct is annotated for JSON marshaling.  
  
### `DockerComponent`  
Describes a single component of the Docker service.    
Contains a nested `Details` field (type `DockerComponentDetails`) plus simple name and version fields.  
  
### `DockerContainer`  
Captures runtime information about an individual container: command, creation timestamps, host configuration, image details, labels, mounts, names, network settings, ports, start/finish times, state, and status. The struct is also JSON‑tagged for serialization.  
  
### `DockerComponentDetails`  
Provides a more granular view of a component’s metadata (API version, architecture, build time, experimental flag, Git commit, Go version, kernel, minimum API, OS). These fields are optional except for the mandatory ones (`GitCommit`).  
  
### `DockerContainerHostConfig`  
A lightweight struct that currently only stores the network mode string. It is embedded in `DockerContainer`.  
  
### `DockerPlatform`  
Simple container for platform name; used by `Docker.Platform`.  
  
---  
  
These types together form a data model that can be marshaled to/from JSON, enabling Docker services to expose their configuration and runtime state as structured Go objects.  
  
# models/services/elastic.go  
**Package / Component**    
`services`  
  
---  
  
### Imports  
No external imports are declared in this file; all types are defined locally within the package.  
  
### External Data & Input Sources  
The struct tags (`json:"..."`) indicate that this code is intended to be marshalled/unmarshalled from JSON data, likely coming from an Elasticsearch cluster API response.    
* `Elastic` – top‑level container for a cluster snapshot.    
* `ElasticCluster` – detailed cluster metadata.    
* `ElasticNodesMore`, `ElasticFailure`, and `ElasticFailureCausedBy` – nested structures that capture node failure information.  
  
### TODOs  
No explicit `TODO:` comments are present in the file.  
  
---  
  
## Summary of Major Code Parts  
  
#### `type Elastic struct`  
Represents a snapshot of an Elasticsearch cluster.    
* **Cluster**: holds general cluster info (`ElasticCluster`).    
* **Nodes**: list of nodes/peers (`ElasticNode` – defined elsewhere).    
* **Indices**: map of index names to their details (`map[string]ElasticIndex`).  
  
#### `type ElasticCluster struct`  
Describes the core metadata for a cluster.    
* `ClusterName`, `ClusterUUID`: identifiers.    
* `Indices`: collection of indices (`ElasticIndices`).    
* `Nodes`: list of nodes (`ElasticNodes`).    
* `NodesMore`: additional node statistics (`ElasticNodesMore`).    
* `Status` and `Timestamp`: status flag and epoch time.  
  
#### `type ElasticNodesMore struct`  
Captures extended node metrics.    
* `Failed`, `Successful`, `Total`: counters for node operations.    
* `Failures`: slice of `ElasticFailure` entries detailing individual failures.  
  
#### `type ElasticFailure struct`  
Describes a single failure event.    
* `CausedBy`: nested cause information (`ElasticFailureCausedBy`).    
* `NodeID`, `Reason`, `Type`: identifiers and description of the failure.  
  
#### `type ElasticFailureCausedBy struct`  
Nested structure for recursive failure causes.    
* `CausedBy`: optional pointer to another `ElasticFailureCausedBy` (recursive).    
* `Reason`, `Type`: textual details of the cause.  
  
---  
  
These definitions together provide a comprehensive data model that can be used to parse, store, and manipulate Elasticsearch cluster information within the `services` package.  
  
# models/services/elastic_commons.go  
**Package/Component name**    
`services`  
  
---  
  
### Imports  
No external imports are required in this file.  
  
### External Data / Input Sources  
The structs defined here are intended to hold JSON‑serializable data that can be read from or written to an Elastic‑search backend (or similar monitoring service). No additional input sources are referenced directly in the code.  
  
### TODOs  
None found in the current source.  
  
---  
  
## Summary of major code parts  
  
| Section | Description |  
|---------|-------------|  
| **ElasticJvmMem** | Holds JVM memory metrics. Each field is annotated with a JSON tag so that it can be marshalled/unmarshalled automatically. The struct captures direct, heap and non‑heap memory usage in bytes as well as the maximum values for each category. |  
| **ElasticOsMem** | Stores operating‑system level memory statistics. Fields include free/used bytes and percentages, plus total bytes. All tags are optional except `total_in_bytes`, ensuring that missing fields will be omitted from JSON output when empty. |  
| **ElasticProcess** | Represents a process snapshot with an ID, file descriptor limits, lock status, refresh interval, CPU load (via nested `ElasticCPULoad` struct) and open‑file‑descriptor statistics (`ElasticOpenFileDescriptors`). All tags are optional except the mandatory ones. |  
  
These structs together provide a lightweight data model for capturing JVM, OS and process metrics that can be persisted or transmitted as JSON. The file is ready to be used by other components in the `services` package that handle serialization, persistence or API communication.  
  
# models/services/elastic_index.go  
## Package and Imports  
- **Package name**: `services`    
- **Imports**: none declared in this file – all types are defined locally for JSON serialization of ElasticSearch cluster statistics.  
  
---  
  
## External Data Sources  
The structs represent the shape of data that will be marshalled/unmarshalled from/to JSON. They map directly to the fields returned by an ElasticSearch node’s stats API, enabling easy decoding into Go values and subsequent use in monitoring or reporting components.  
  
---  
  
## TODOs  
No `TODO` comments are present in this file; all types are fully defined.  
  
---  
  
## Summary of Major Code Parts  
  
### 1. Index‑level metrics    
| Type | Purpose | Key fields |  
|------|---------|------------|  
| **ElasticIndex** | Top‑level container for index statistics. | `Primaries`, `Total`, `UUID` |  
| **ElasticIndexStats** | Holds the indexing sub‑metrics. | `Indexing` |  
| **ElasticIndexing** | Detailed counters for delete/index operations, throttling and performance. | `DeleteCurrent`, `DeleteTimeInMillis`, `IndexCurrent`, etc. |  
  
These three types form a nested hierarchy that captures all primary/total statistics for an index, including counts of deletes, indexing time, and throughput.  
  
---  
  
### 2. Completion & Docs metrics    
| Type | Purpose | Key fields |  
|------|---------|------------|  
| **ElasticIndices** | Aggregates high‑level stats for an entire node (or cluster). | `Completion`, `Count`, `Docs`, `Fielddata`, `FilterCache`, `IDCache`, `Percolate`, `QueryCache`, `Segments`, `Shards`, `Store` |  
| **ElasticCompletion** | Size of the completion cache. | `SizeInBytes` |  
| **ElasticIndexDocs** | Document counts and deletions. | `Count`, `Deleted` |  
| **ElasticFielddata** | Memory usage for field data. | `Evictions`, `MemorySizeInBytes` |  
| **ElasticFilterCache** | Cache evictions & memory. | `Evictions`, `MemorySizeInBytes` |  
  
These types provide a comprehensive view of the node’s index state, from completion cache to segment statistics.  
  
---  
  
### 3. Node‑level and OS metrics    
| Type | Purpose | Key fields |  
|------|---------|------------|  
| **ElasticNodes** | Main container for node‑specific stats. | `Count`, `FS`, `JVM`, `NetworkTypes`, `OS`, `Plugins`, `Process`, `Versions`, `Ingest`, `PackagingTypes`, `DiscoveryTypes` |  
| **ElasticNodeCount** | Counts of various node roles. | `Client`, `CoordinatingOnly`, `Data`, etc. |  
| **ElasticFS** | File system and disk I/O metrics. | `AvailableInBytes`, `DiskIoOp`, `DiskReadSizeInBytes`, etc. |  
| **ElasticJVMdata** | JVM memory & thread stats. | `MaxUptimeInMillis`, `Mem`, `Threads`, `Versions` |  
| **ElasticOS** | OS‑level counters and CPU metrics. | `AllocatedProcessors`, `AvailableProcessors`, `CPU`, `Mem`, `Names`, `PrettyNames` |  
  
These structs capture the full breadth of node performance, from file system usage to JVM uptime.  
  
---  
  
### 4. Miscellaneous & helper types    
| Type | Purpose | Key fields |  
|------|---------|------------|  
| **ElasticShardIndex** | Per‑shard metrics for primaries/replication/shards. | `Primaries`, `Replication`, `Shards` |  
| **ElasticIndexMetric** | Simple avg/max/min container used by several nested structs. | `Avg`, `Max`, `Min` |  
| **ElasticIndicesShards** | Summary of shard counts and replication factor. | `Index`, `Primaries`, `Replication`, `Total` |  
| **ElasticJVMVersion**, **ElasticOSname**, **ElasticNetworkTypes**, **ElasticPercolate**, **ElasticPrettyName**, **ElasticQueryCache**, **ElasticSegments**, **ElasticStore**, **PackagingType** | Supporting types for JVM versions, OS names, network type maps, percolation stats, pretty‑names list, query cache counters, segment memory, store size, and packaging metadata. |  
  
These helper structs are used throughout the hierarchy to keep the JSON structure flat yet fully typed.  
  
---  
  
All types together form a complete Go representation of an ElasticSearch node’s statistics payload, ready for unmarshalling from JSON and further processing in the `services` package.  
  
# models/services/elastic_node.go  
**Package / Component**    
`services`  
  
---  
  
### Imports  
No explicit imports are declared in this file; all types rely on the Go standard library and JSON tags for marshaling/unmarshaling data.  
  
### External Data & Input Sources  
The structs represent the shape of JSON payloads exchanged with an ElasticSearch cluster.    
- `ElasticNode` – top‑level node representation, containing a map of individual node info (`Nodes`) and aggregated statistics (`NodesStat`).    
- Nested types (`ElasticNodeInfo`, `ElasticOsInfo`, etc.) describe detailed attributes such as CPU, network, modules, plugins, and JVM metrics.  
  
### TODOs  
No `TODO` comments are present in the current code.  
  
---  
  
## Summary of Major Code Parts  
  
| Struct | Purpose & Key Fields |  
|--------|---------------------|  
| **ElasticNode** | Holds cluster name, a map of node identifiers to detailed info (`Nodes`), and overall node statistics (`NodesStat`). |  
| **ElasticNodeStat** | Aggregates failure/success counts and total nodes for the cluster. |  
| **ElasticAttributes** | Stores miscellaneous node attributes (AWS zone, box type, rack, etc.) that are optional in JSON. |  
| **ElasticCPUdata** | Captures CPU characteristics: cache size, cores per socket, MHz, vendor, etc. |  
| **ElasticNodeHTTP** | Holds HTTP binding information for a node. |  
| **ElasticIngest** | Describes ingest processors and pipeline counts. |  
| **ElasticJVM** | Contains JVM‑level metrics (GC collectors, memory pools, start time). |  
| **ElasticModule** | Represents an ElasticSearch module with its name, version, description, and optional flags. |  
| **ElasticNetwork** | Holds network interface details and refresh interval. |  
| **ElasticNodeInfo** | The core node descriptor: thread pool, settings, attributes, HTTP, ingest, modules, OS info, plugins, process, roles, transport, etc. |  
| **ElasticOsInfo** | Provides OS‑level metrics (processors, CPU data, memory, swap). |  
| **ElasticPlugin** | Describes a plugin with its class name, version, URL, and optional flags. |  
| **ElasticPrimaryInterface** | Simple representation of the primary network interface. |  
| **ElasticProcessor** | Minimal processor descriptor used in ingest. |  
| **ElasticSwap** | Swap memory statistics for a node. |  
| **ElasticTransport** | Transport binding information for a node. |  
  
These types collectively model an ElasticSearch node’s state and configuration, enabling the `services` package to marshal/unmarshal JSON data when communicating with the cluster.  
  
# models/services/etcd.go  
**Package / Component**    
`services`  
  
---  
  
### Imports    
No explicit imports are present in this file; the structs rely only on Go’s built‑in types.  
  
### External data & input sources    
The struct field tags (`json:"..."`) indicate that these structures are intended for JSON serialization/deserialization.    
- `Etcd.ClientUrls`, `PeerUrls` – lists of URLs used by the etcd node.    
- `Etcd.ID`, `Name`, `StartTime`, `State`, `Version` – basic metadata about the node and cluster.    
- `Etcd.LeaderInfo` – nested struct holding leader‑specific data.    
- Counters (`RecvAppendRequestCnt`, `SendAppendRequestCnt`) and rates (`RecvBandwidthRate`, `SendBandwidthRate`, etc.) provide performance metrics.  
  
### TODOs    
No TODO comments are present in the current snippet.  
  
---  
  
## Summary of major code parts  
  
#### `Etcd` struct  
* Holds all runtime information for an etcd node.    
  * **ClientUrls** – slice of client endpoint URLs (JSON key: `"clientURLs"`).    
  * **ID** – unique identifier string for the node.    
  * **LeaderInfo** – nested `EtcdLeaderInfo` providing leader‑specific data.    
  * **Name** – cluster name.    
  * **PeerUrls** – slice of peer endpoint URLs (JSON key: `"peerURLs"`).    
  * **RecvAppendRequestCnt**, **SendAppendRequestCnt** – counters for append requests received/sent.    
  * **RecvBandwidthRate**, **SendBandwidthRate** – optional bandwidth rates for receive/send paths.    
  * **RecvPkgRate**, **SendPkgRate** – optional package rates for receive/send paths.    
  * **StartTime** – timestamp when the service started.    
  * **State** – current state of the node (e.g., `"running"`).    
  * **Version** – etcd version string.  
  
#### `EtcdLeaderInfo` struct  
* Nested within `Etcd`.    
  * **Leader** – identifier of the leader node.    
  * **StartTime** – timestamp when the leader started.    
  * **Uptime** – uptime duration for the leader.  
  
These structs together form a lightweight representation of an etcd cluster’s status, ready to be marshaled into JSON and persisted or transmitted over the network.  
  
# models/services/ethernetip.go  
## Package / Component    
**services**  
  
The file defines a single Go struct that represents an Ethernet/IP service message. No external imports are required for this definition.  
  
---  
  
### Imports    
None – the struct relies only on the standard library types (`int`, `string`, `interface{}`).  
  
---  
  
### External Data / Input Sources    
All fields carry JSON tags, indicating that instances of this struct will be marshalled/unmarshalled to/from JSON. The data is expected to come from an Ethernet/IP service response and contains:  
  
* command information  
* device & product metadata  
* encapsulation header details  
* raw payload (hex‑encoded)  
* revision numbers  
* session context  
  
---  
  
### TODOs    
No `TODO` comments are present in the file.  
  
---  
  
## Summary of Major Code Parts    
  
### 1. Command & Status Section    
| Field | Purpose |  
|-------|---------|  
| `Command`, `CommandLength` | Encodes the command code and its length. |  
| `CommandStatus` | Return status flag for the command. |  
  
These fields form the header that identifies what operation is being performed.  
  
### 2. Device & Product Metadata    
| Field | Purpose |  
|-------|---------|  
| `DeviceType`, `ProductCode`, `ProductName`, `ProductNameLength` | Identify the device type and product details. |  
| `VendorID` (interface{}) | Holds a numeric vendor identifier; can be an int or other type. |  
  
### 3. Encapsulation Header    
| Field | Purpose |  
|-------|---------|  
| `EncapsulationLength`, `IP`, `Options`, `SenderContext`, `Session`, `SocketAddr` | Describe the encapsulation header, IP address, options flags, sender context, session ID and socket address. |  
  
### 4. Payload & Revision Information    
| Field | Purpose |  
|-------|---------|  
| `Raw` | Hex‑encoded string containing the original response from the Ethernet/IP service. |  
| `RevisionMajor`, `RevisionMinor` | Major/minor revision numbers of the encapsulation format. |  
  
### 5. Serial, State & Version    
| Field | Purpose |  
|-------|---------|  
| `Serial`, `State`, `Status`, `TypeID`, `Version` | Track serial number, state flag, status code, type ID and protocol version. |  
  
---  
  
All fields are annotated with JSON tags so that the struct can be directly used for serialization/deserialization of Ethernet/IP service messages.  
  
# models/services/ftp.go  
**Package/Component name**    
`services`  
  
---  
  
### Imports    
No external imports are declared in this file.  
  
### External data / input sources    
The types defined here use JSON struct tags, indicating that instances of these structs will be marshalled/unmarshalled to/from JSON (e.g., when communicating with an FTP server or persisting configuration).  
  
### TODOs    
None found in the current code snippet.  
  
---  
  
## Summary  
  
#### `FTP` struct  
- **Purpose**: Represents a collection of capabilities for an FTP server.    
- **Fields**:  
  - `Anonymous bool`: Flag indicating whether anonymous access is allowed.  
  - `Features map[string]FtpFeature`: A mapping from feature names to their corresponding parameters; the key is a string identifier, and the value is an `FtpFeature` struct that lists supported parameters.  
  - `FeaturesHash *int`: Pointer to an integer holding a numeric hash of the features list (useful for change detection or caching).  
  
#### `FtpFeature` struct  
- **Purpose**: Encapsulates the parameter set for a single FTP feature.    
- **Fields**:  
  - `Parameters []string`: Slice of strings that enumerate the valid parameters associated with this feature.  
  
These two structs together provide a lightweight, JSON‑serialisable representation of an FTP server’s capabilities and their configuration options.  
  
# models/services/hive.go  
**Package / Component name**  
  
`services`  
  
---  
  
### Imports    
No external imports are declared in this file.  
  
### External data, input sources    
The code defines three Go structs that model a Hive service hierarchy:  
* `Hive` – top‑level container for multiple databases.  
* `HiveDatabase` – represents an individual database with its tables.  
* `HiveTable` – represents a table within a database, including name and arbitrary properties.  
  
No external data sources are referenced directly in this file; the structs are intended to be marshalled/unmarshalled from JSON (as indicated by the struct tags).  
  
### TODOs    
There are no explicit `TODO:` comments in the provided code.  
  
---  
  
## Summary of major code parts  
  
#### 1. `Hive` struct  
* Holds a slice of `HiveDatabase` objects.  
* The field tag `json:"databases"` indicates that when this struct is serialized to JSON, the key will be `"databases"`.  
* Acts as the root container for all database information in the service.  
  
#### 2. `HiveDatabase` struct  
* Contains a single string field `Name`, tagged with `json:"name"`.  
* Holds a slice of `HiveTable` objects under the tag `json:"tables"`.  
* Represents one logical database within the Hive service, exposing its name and tables.  
  
#### 3. `HiveTable` struct  
* Stores the table’s name (`Name`) and an arbitrary list of key/value pairs in `Properties`.    
* The properties field is a slice of maps from string to string, allowing flexible metadata for each table.  
* Tagging ensures JSON keys `"name"` and `"properties"` are used during marshaling.  
  
Overall, this file defines the data model for a Hive service hierarchy that can be serialized to/from JSON. It provides clear struct tags for easy integration with other components of the package.  
  
# models/services/http.go  
**Package / Component**    
- **services**  
  
---  
  
### Imports  
No explicit imports are declared in this file; all types rely on the Go standard library and any external packages that may be imported elsewhere in the project.  
  
### External Data & Input Sources  
| Field | Description |  
|-------|-------------|  
| `Components` | Map of component names to their categories (used for site‑wide feature tracking). |  
| `Favicon` | Base64‑encoded favicon image data and its location. |  
| `HTML` | Raw HTML content fetched from the host. |  
| `HTMLHash` | Numeric hash of the HTML string, useful for change detection. |  
| `Host` | Hostname used to request the page. |  
| `Location` | Final URL after any redirects. |  
| `Redirects` | Slice of redirect records that were followed during fetching. |  
| `Robots` | Content of the robots.txt file. |  
| `RobotsHash` | Hash of the robots.txt content. |  
| `Securitytxt` | Content of security.txt (note: typo – should be “securitytxt”). |  
| `SecuritytxtHash` | Hash of the security.txt content. |  
| `Server` | Server header from the HTTP response. |  
| `Sitemap` | sitemap.xml content. |  
| `SitemapHash` | Hash of the sitemap content. |  
| `Title` | Page title. |  
| `WAF` | Web Application Firewall identifier (optional). |  
  
### TODOs  
No explicit `TODO:` comments are present in this file, but future work could include:  
- Adding validation logic for hash calculations.  
- Implementing methods to serialize/deserialize the struct to/from JSON.  
  
---  
  
## Summary of Major Code Parts  
  
#### 1. `HTTP` Struct    
The central data holder for a website’s HTTP snapshot. It aggregates all fetched resources (HTML, favicon, robots.txt, sitemap.xml) and metadata such as host, title, and server header. The struct is annotated with JSON tags to enable easy marshaling/unmarshaling.  
  
#### 2. `HttpComponent` Struct    
Represents individual components of the website, currently only storing a list of categories. This could be expanded later to include more detailed component data (e.g., type, version).  
  
#### 3. `HttpRedirect` Struct    
Captures information about each redirect that was followed during fetching: raw data, resulting HTML snippet, host and final location.  
  
#### 4. `HttpFavicon` Struct    
Encapsulates the favicon image in base64 form along with its hash and URL. This allows the service to track changes to the favicon over time.  
  
---  
  
All structs are tightly coupled through JSON tags, making them ready for persistence or API communication. The file serves as a foundational data model within the `services` package, likely used by higher‑level components that orchestrate HTTP requests and storage.  
  
# models/services/influxdb.go  
## Package / Component    
**services.InfluxDb**  
  
### Imports  
No external imports are required for this file.  
  
### External data / input sources    
The struct `InfluxDb` is a plain Go type that holds runtime information about an InfluxDB instance. It can be populated from JSON or other configuration files, but the code itself does not reference any external data sources directly.  
  
### TODOs  
No TODO comments are present in this file.  
  
### Summary of major code parts    
- **Struct definition** – The `InfluxDb` struct contains fields that describe the state and configuration of an InfluxDB service.    
  - `Uptime`, `GoMaxProcs`, `GoVersion`, `GoOS`, `GoArch`: runtime metrics about the Go process.    
  - `NetworkHostname`, `BindAddress`, `Build`, `Version`: metadata for network and build information.    
  - `Databases`: a slice of database names that are managed by this service.  
  
This file provides a single data structure used throughout the `services` package to pass InfluxDB configuration around.  
  
# models/services/isakmp.go  
## Package / Component    
**services**  
  
### Imports    
No explicit imports are present in this file; the code relies solely on Go’s built‑in types and tags.  
  
---  
  
## External Data & Input Sources    
  
| Field | Type | JSON tag | Notes |  
|-------|------|----------|-------|  
| `Aggressive` | *ISAKMP | `aggressive,omitempty` | Pointer to another ISAKMP struct (recursive). |  
| `ExchangeType` | int | `exchange_type` | Numeric code for the IKE exchange type. |  
| `Flags` | IsakmpFlags | `flags` | Nested struct holding boolean flags. |  
| `InitiatorSPI` | string | `initiator_spi` | Hex‑encoded initiator security parameter index. |  
| `Length` | int | `length` | Size of the ISAKMP packet in bytes. |  
| `MsgID` | string | `msg_id` | Hex‑encoded message identifier. |  
| `NextPayload` | int | `next_payload` | Type code for the next payload after initiation. |  
| `ResponderSPI` | string | `responder_spi` | Hex‑encoded responder security parameter index. |  
| `VendorIds` | []string | `vendor_ids` | List of vendor identifiers used to fingerprint the service. |  
| `Version` | string | `version` | Protocol version string. |  
  
Nested struct:  
  
| Field | Type | JSON tag | Notes |  
|-------|------|----------|-------|  
| `Authentication` | bool | `authentication` | Flag indicating authentication is enabled. |  
| `Commit` | bool | `commit` | Flag indicating commit is enabled. |  
| `Encryption` | bool | `encryption` | Flag indicating encryption is enabled. |  
  
---  
  
## TODOs    
No explicit TODO comments are present in the provided code.  
  
---  
  
## Summary of Major Code Parts    
  
### 1. ISAKMP struct definition    
The top‑level `ISAKMP` struct represents an IKE (Internet Key Exchange) message used in IPsec/IKEv2 negotiations. It contains all fields required to describe a complete ISAKMP packet, including identifiers, flags, payload types, and vendor information. The struct is annotated with JSON tags for easy marshaling/unmarshaling.  
  
### 2. IsakmpFlags nested struct    
`IsakmpFlags` holds three boolean flags that indicate whether authentication, commit, and encryption are enabled in the ISAKMP packet. These flags are embedded within the main struct via the `Flags` field.  
  
---  
  
# models/services/lantronix.go  
## Package / Component    
**services**  
  
---  
  
### Imports    
No external imports are required for this file.  
  
### External Data / Input Sources    
The `Lantronix` struct is intended to hold configuration data for a Lantronix device, typically populated from JSON payloads or configuration files.  
  
### TODOs    
None found in the current code.  
  
---  
  
## Summary of Major Code Parts    
  
#### Struct Definition: `Lantronix`  
```go  
type Lantronix struct {  
    Gateway *string `json:"gateway"`  
    IP      *string `json:"ip"`  
    Mac     string  `json:"mac"`  
    Password *string `json:"password"`  
    Type    *string `json:"type"`  
    Version string  `json:"version"`  
}  
```  
* **Purpose** – Represents a Lantronix device’s configuration and status.    
* **Fields**    
  - `Gateway`: pointer to the gateway address (JSON key `"gateway"`).    
  - `IP`: pointer to the main IP address that the service listens on (JSON key `"ip"`).    
  - `Mac`: MAC address of the device (JSON key `"mac"`).    
  - `Password`: pointer to the password used for authentication (JSON key `"password"`).    
  - `Type`: pointer to the device type identifier (JSON key `"type"`).    
  - `Version`: firmware version string (JSON key `"version"`).  
  
* **Usage** – This struct can be marshalled/unmarshalled with Go’s `encoding/json` package, allowing easy persistence or network communication of Lantronix configuration data.  
  
---  
  
#### Notes for Package Integration    
- The file resides in the `services` package; it may be imported by other modules that need to manage Lantronix devices.    
- No additional logic is present here; future extensions could include methods for validation, serialization, or device control.  
  
# models/services/minecraft.go  
**Package / Component**    
`services`  
  
**Imports**    
  
```go  
import "encoding/json"  
```  
  
---  
  
## External data & input sources    
The file defines a set of Go structs that map directly to JSON payloads typically received from a Minecraft server status API (or similar service). Each struct field is annotated with `json:"..."`, so the package can marshal/unmarshal JSON into these types. The root type, `Minecraft`, aggregates all information about a running server: its version, player statistics, Forge data, mod list and an optional description block.  
  
---  
  
## TODOs    
No explicit `TODO:` comments are present in this file; if future work is required it can be added to the section below.  
  
---  
  
# Summary of major code parts  
  
## 1. Root struct – **`Minecraft`**    
| Field | Type | JSON key | Notes |  
|-------|------|----------|-------|  
| `Version` | `MinecraftServerVersion` | `"version"` | Server protocol & name |  
| `Players` | `MinecraftPlayersInfo` | `"players"` | Max/online player counts |  
| `ForgeData` | `MinecraftForgeInfo` | `"forgeData"` | Forge channels, mods and network version |  
| `ModInfo` | `MinecraftModInfo` | `"modinfo,omitempty"` | List of mod items (type + list) |  
| `Description` | `json.RawMessage` | `"description"` | Raw JSON block that can be further decoded into a `MinecraftDescription` struct if needed |  
| `Favicon` | `string` | `"favicon,omitempty"` | Optional favicon URL or base64 data |  
| `Whitelisted` | `bool` | `"whitelisted,omitempty"` | Flag indicating whether the server is whitelisted |  
  
The root type serves as the primary data container for a Minecraft service status response.  
  
## 2. Server version – **`MinecraftServerVersion`**    
- Holds the numeric protocol and human‑readable name of the running server.    
  
## 3. Player statistics – **`MinecraftPlayersInfo`**    
- Simple counters: maximum capacity (`Max`) and current online count (`Online`).    
  
## 4. Forge data – **`MinecraftForgeInfo`**    
| Field | Type | JSON key | Notes |  
|-------|------|----------|-------|  
| `Channels` | `[]MinecraftForgeChannel` | `"channels"` | List of active Forge channels (resource, version, required flag) |  
| `Mods` | `[]MinecraftMod` | `"mods"` | Mod identifiers and markers |  
| `FmlNetworkVersion` | `int` | `"fmlNetworkVersion"` | Forge Mod Loader network version |  
  
## 5. Forge channel – **`MinecraftForgeChannel`**    
- Represents a single channel entry with resource string, version string, and whether it is required.  
  
## 6. Mod entry – **`MinecraftMod`**    
- Simple key/value pair for a mod: marker and ID.  
  
## 7. Mod list info – **`MinecraftModInfo`**    
| Field | Type | JSON key | Notes |  
|-------|------|----------|-------|  
| `Type` | `string` | `"type"` | Category or type of the mod list |  
| `ModList` | `[]ModInfoItem` | `"modList"` | Array of individual mod items |  
  
## 8. Mod info item – **`ModInfoItem`**    
- Contains a version string and an ID for each mod in the list.  
  
## 9. Player struct – **`MinecraftPlayer`**    
- Simple representation of a player with an ID and name; not directly used by `Minecraft`, but may be useful elsewhere in the package.  
  
## 10. Description block – **`MinecraftDescription`**    
| Field | Type | JSON key | Notes |  
|-------|------|----------|-------|  
| `Text` | `string` | `"text"` | Main description text |  
| `Extra` | `[]map[string]interface{}` | `"extra,omitempty"` | Optional array of arbitrary extra data (e.g., formatting, colors) |  
  
The `Description` field in the root struct uses `json.RawMessage`, allowing lazy decoding into this nested structure when needed.  
  
---  
  
**End of output**  
  
# models/services/monero.go  
**Package / Component**    
`services`  
  
---  
  
### Imports  
No explicit imports are declared in this file; the structs rely only on Go’s standard library for JSON marshaling/unmarshaling.  
  
### External Data / Input Sources  
The two structs (`Monero` and `MoneroConnection`) represent data that is expected to be received from or sent to a Monero daemon via its JSON‑API. Each field carries a `json:"..."` tag, indicating the key name used when marshaling/unmarshaling.  
  
### TODOs  
No TODO comments are present in this file.  
  
---  
  
## Summary of Major Code Parts  
  
### 1️⃣ `Monero` struct    
Represents the overall status of a Monero node.    
| Field | Type | JSON Key | Notes |  
|-------|------|----------|-------|  
| `Credits` | `uint64` | `"credits"` | Total credits earned by the node. |  
| `TopHash` | `string` | `"top_hash"` | Hash of the most recent block. |  
| `AltBlocksCount` | `int` | `"alt_blocks_count"` | Number of alternative blocks to main chain. |  
| `BlockSizeLimit` | `int` | `"block_size_limit"` | Maximum allowed block size. |  
| `BlockSizeMedian` | `int` | `"block_size_median,omitempty"` | Median block size (optional). |  
| `BlockWeightLimit` | `int` | `"block_weight_limit,omitempty"` | Max weight of a block. |  
| `BlockWeightMedian` | `int` | `"block_weight_median,omitempty"` | Median block weight (optional). |  
| `BootstrapDaemonAddress` | `string` | `"bootstrap_daemon_address,omitempty"` | Address used for bootstrap. |  
| `Connections` | `[]MoneroConnection` | `"connections"` | Slice of peer connections. |  
| `CumulativeDifficulty` | `int` | `"cumulative_difficulty"` | Cumulative network difficulty. |  
| `CumulativeDifficultyTop64` | `int` | `"cumulative_difficulty_top64,omitempty"` | Top‑64 bits of cumulative difficulty (optional). |  
| `DatabaseSize` | `int` | `"database_size,omitempty"` | Size of the node’s database. |  
| `Difficulty` | `int` | `"difficulty"` | Current network difficulty. |  
| `DifficultyTop64` | `int` | `"difficulty_top64,omitempty"` | Top‑64 bits of difficulty (optional). |  
| `FreeSpace` | `int` | `"free_space,omitempty"` | Free disk space available. |  
| `GreyPeerlistSize` | `int` | `"grey_peerlist_size"` | Size of the grey peer list. |  
| `Height` | `int` | `"height"` | Current blockchain height. |  
| `HeightWithoutBootstrap` | `int` | `"height_without_bootstrap,omitempty"` | Height excluding bootstrap blocks (optional). |  
| `IncomingConnectionsCount` | `int` | `"incoming_connections_count"` | Number of incoming connections. |  
| `Mainnet` | `bool` | `"mainnet,omitempty"` | Flag indicating main network usage. |  
| `Nettype` | `string` | `"nettype,omitempty"` | Network type (e.g., main, test). |  
| `Offline` | `bool` | `"offline,omitempty"` | Node offline status flag. |  
| `OutgoingConnectionsCount` | `int` | `"outgoing_connections_count"` | Number of outgoing connections. |  
| `RPCConnectionsCount` | `int` | `"rpc_connections_count,omitempty"` | RPC connection count (optional). |  
| `Stagenet` | `bool` | `"stagenet,omitempty"` | Flag for stage network usage. |  
| `StartTime` | `int` | `"start_time"` | Node start time in epoch seconds. |  
| `Status` | `string` | `"status"` | Current status string. |  
| `Target` | `int` | `"target"` | Target proof‑of‑work value. |  
| `TargetHeight` | `int` | `"target_height"` | Height of the next block to be mined. |  
| `Testnet` | `bool` | `"testnet"` | Flag indicating test network usage. |  
| `TopBlockHash` | `string` | `"top_block_hash"` | Hash of the highest known block. |  
| `TxCount` | `int` | `"tx_count"` | Total number of non‑coinbase transactions. |  
| `TxPoolSize` | `int` | `"tx_pool_size"` | Size of the transaction pool. |  
| `Untrusted` | `bool` | `"untrusted,omitempty"` | Flag for untrusted status (optional). |  
| `UpdateAvailable` | `bool` | `"update_available,omitempty"` | Update availability flag (optional). |  
| `Version` | `string` | `"version,omitempty"` | Node version string (optional). |  
| `WasBootstrapEverUsed` | `bool` | `"was_bootstrap_ever_used,omitempty"` | Flag for bootstrap usage (optional). |  
| `WhitePeerlistSize` | `int` | `"white_peerlist_size"` | Size of the white peer list. |  
| `WideCumulativeDifficulty` | `string` | `"wide_cumulative_difficulty,omitempty"` | Wide cumulative difficulty string (optional). |  
| `WideDifficulty` | `string` | `"wide_difficulty,omitempty"` | Wide difficulty string (optional). |  
  
### 2️⃣ `MoneroConnection` struct    
Represents a single peer connection to the Monero daemon.    
| Field | Type | JSON Key | Notes |  
|-------|------|----------|-------|  
| `Address` | `string` | `"address"` | Peer address. |  
| `AvgDownload` | `int` | `"avg_download"` | Average download speed. |  
| `AvgUpload` | `int` | `"avg_upload"` | Average upload speed. |  
| `ConnectionID` | `string` | `"connection_id"` | Unique connection identifier. |  
| `CurrentDownload` | `int` | `"current_download"` | Current download rate. |  
| `CurrentUpload` | `int` | `"current_upload"` | Current upload rate. |  
| `Height` | `int` | `"height"` | Height of the peer’s chain. |  
| `Host` | `string` | `"host"` | Host name or IP. |  
| `IP` | `string` | `"ip"` | Peer IP address. |  
| `Incoming` | `bool` | `"incoming"` | Flag if connection is incoming. |  
| `LiveTime` | `int` | `"live_time"` | Live time of the connection. |  
| `LocalIP` | `bool` | `"local_ip"` | Flag for local IP usage. |  
| `Localhost` | `bool` | `"localhost"` | Flag for localhost usage. |  
| `PeerID` | `string` | `"peer_id"` | Peer identifier. |  
| `Port` | `string` | `"port"` | Connection port. |  
| `PruningSeed` | `int` | `"pruning_seed,omitempty"` | Pruning seed value (optional). |  
| `RPCPort` | `int` | `"rpc_port,omitempty"` | RPC port number (optional). |  
| `RecvCount` | `int` | `"recv_count"` | Number of received packets. |  
| `RecvIdleTime` | `int` | `"recv_idle_time"` | Idle time for receiving. |  
| `SendCount` | `int` | `"send_count"` | Number of sent packets. |  
| `SendIdleTime` | `int` | `"send_idle_time"` | Idle time for sending. |  
| `State` | `string` | `"state"` | Current state string. |  
| `SupportFlags` | `int` | `"support_flags"` | Support flags bitmask. |  
  
---  
  
These two structs together provide a comprehensive representation of the Monero node’s status and its peer connections, ready for JSON serialization/deserialization.  
  
# models/services/mongodb.go  
**Package / Component**    
- **Name:** `services`    
- **Imports:** None declared in this file (only the standard package declaration is present).  
  
---  
  
### External Data & Input Sources  
The code defines a set of Go structs that map to JSON data returned by MongoDB server commands.    
* `Mongo` – top‑level representation of a MongoDB instance, including authentication status, build info, database list and server status.    
* `MongoListDatabases` – holds the result of the `listDatabases` command (array of databases, OK flag, size metrics).    
* `MongoBuildInfo` – detailed information from the `buildInfo` command (allocator, compiler details, versioning, etc.).    
* `MongoBuildEnvironment` – nested build environment data used by `MongoBuildInfo`.    
* `MongoDatabase` – individual database metadata (collections, name, size).    
* `MongoOpenSSl` – sub‑structure for OpenSSL information inside the build info.  
  
These structs are intended to be marshalled/unmarshalled with JSON tags that match MongoDB’s command output.  
  
---  
  
### TODO Comments  
No explicit `TODO:` comments were found in this file.    
  
---    
  
## Summary of Major Code Parts  
  
#### 1️⃣ `Mongo` struct    
Represents the overall state of a MongoDB server.    
- **Fields**:    
  - `Authentication` (bool) – whether authentication is enabled.    
  - `BuildInfo` (`MongoBuildInfo`) – nested build information.    
  - `ListDatabases` (`MongoListDatabases`) – list of databases with optional JSON tag.    
  - `ServerStatus` (map[string]interface{}) – generic server status snapshot.  
  
#### 2️⃣ `MongoListDatabases` struct    
Captures the output of MongoDB’s `listDatabases` command.    
- **Fields**:    
  - `Databases` ([]MongoDatabase) – slice of database entries.    
  - `Ok`, `TotalSize`, `TotalUncompressedSize` – numeric metrics.  
  
#### 3️⃣ `MongoBuildInfo` struct    
Holds detailed build information from the `buildInfo` command.    
- **Fields**: allocator, bits, compiler flags, version strings, timestamps, and nested `MongoBuildEnvironment`.    
- Includes a slice of modules and an embedded `MongoOpenSSl`.  
  
#### 4️⃣ `MongoBuildEnvironment` struct    
Nested environment data used by `MongoBuildInfo`.    
- Contains compiler/linker flags, architecture/OS targets, and distribution details.  
  
#### 5️⃣ `MongoDatabase` struct    
Describes a single database entry.    
- **Fields**: collections list, name, size metrics, and an empty flag.  
  
#### 6️⃣ `MongoOpenSSl` struct    
Sub‑structure for OpenSSL information inside the build info.    
- Holds compiled and running version strings.  
  
---  
  
These structs together provide a comprehensive Go model of MongoDB server status that can be serialized to/from JSON, enabling higher‑level services or monitoring components to consume this data.  
  
# models/services/mqtt.go  
**Package / Component Name**    
`services`  
  
---  
  
### Imports  
No external imports are declared in this file.  
  
### External Data / Input Sources  
The code defines two data structures that will be used to marshal/unmarshal JSON payloads:  
- `MQTT` – a container for response status and an array of messages.  
- `MqttMessage` – individual message entries with a payload pointer and topic string.  
  
---  
  
### TODO Comments  
No TODO comments are present in the current file.  
  
---  
  
## Summary of Major Code Parts  
  
### 1. `type MQTT struct`  
* **Purpose**: Represents a response from an MQTT broker or similar service.    
* **Fields**:  
  * `Code int` – JSON field `"code"`; holds a status code (0 indicates success).    
  * `Messages []MqttMessage` – JSON field `"messages"`; stores a slice of individual messages received during a 5‑second window.  
  
### 2. `type MqttMessage struct`  
* **Purpose**: Encapsulates the payload and topic for each message captured by the service.    
* **Fields**:  
  * `Payload *string` – JSON field `"payload"`; pointer to a string containing the raw payload data (available only on system properties).    
  * `Topic string` – JSON field `"topic"`; identifies the MQTT topic associated with this message.  
  
These two structs together form the core data model for the `services` package, enabling serialization of MQTT responses and their constituent messages.  
  
# models/services/netbios.go  
**Package / Component Name**    
`services`  
  
---  
  
### Imports  
No external packages are imported in this file; all types and fields are defined locally within the `services` package.  
  
### External Data & Input Sources  
The struct definitions rely on JSON tags for serialization/deserialization, implying that data will be read from or written to JSON sources (e.g., configuration files, network packets, or API responses). No other external data sources are referenced directly in this file.  
  
### TODOs  
No `TODO` comments were found in the provided code snippet.  
  
---  
  
## Summary of Major Code Parts  
  
#### 1. `Netbios` struct    
- **Purpose**: Represents a NetBIOS service instance with all relevant metadata and state.  
- **Fields**:  
  - `MAC`: MAC address string for the primary network interface.  
  - `Names`: Slice of `NetbiosName`, holding individual NetBIOS names discovered or configured.  
  - `Networks`: Slice of strings listing additional networks/interfaces that this service monitors.  
  - `Raw`: Slice of hex‑encoded response packets, likely used for debugging or raw data capture.  
  - `Servername`: Name of the server running the NetBIOS service.  
  - `Username`: Pointer to a string containing the user name; optional field allowing nil values.  
  
#### 2. `NetbiosName` struct    
- **Purpose**: Encapsulates individual NetBIOS names with associated flags and suffix information.  
- **Fields**:  
  - `Flags`: Integer flag set for each name (e.g., status or type bits).  
  - `Name`: The actual NetBIOS name string.  
  - `Suffix`: Integer suffix used to differentiate multiple names on the same interface.  
  
---  
  
These two structs together provide a lightweight, JSON‑serializable model of a NetBIOS service and its constituent names. They can be instantiated from configuration files or network captures, manipulated by other components in the package, and persisted back to storage or transmitted over the network.  
  
# models/services/ntp.go  
**Component:** `services`    
**File:** *ntp.go* (provides NTP data structure and JSON handling)  
  
---  
  
## Imports  
```go  
import (  
	"encoding/json"  
)  
```  
The file uses only the standard library’s `encoding/json` package for marshaling/unmarshaling.  
  
---  
  
## External Data / Input Sources  
| Source | Description |  
|--------|-------------|  
| `documentedKeys` | Slice of JSON field names that are explicitly mapped to struct fields. These keys are used in custom unmarshalling to separate known fields from “extra” data. |  
| JSON payload | The method `UnmarshalJSON` expects a byte slice containing an NTP‑formatted JSON object, which is decoded into the struct and any remaining key/value pairs are stored in the `Extra` map. |  
  
---  
  
## TODOs  
- **MarshalJSON**: A stub exists for future implementation of custom marshaling logic.  
  
```go  
// todo: loss of Extra data  
//func (n *NTP) MarshalJSON() ([]byte, error) {  
//	panic("Not implemented")  
//}  
```  
  
---  
  
## Summary of Major Code Parts  
  
### 1. `documentedKeys`  
A slice of strings that lists all JSON keys expected to be mapped directly into the `NTP` struct. It is used in `UnmarshalJSON` to filter out known fields from the raw map.  
  
### 2. `type NTP struct`  
Defines the data model for an NTP (Network Time Protocol) response:  
- Fields cover system information, version, clock details, timing metrics, and a flexible `Extra` map.  
- JSON tags specify how each field is encoded/decoded; many fields are optional (`omitempty`) to allow partial payloads.  
  
### 3. `type ntpOverhead NTP`  
An alias used as an intermediate type in the custom unmarshal routine. It allows unmarshalling into a temporary copy before copying back into the receiver.  
  
### 4. `(n *NTP) UnmarshalJSON(bytes []byte)`  
Custom JSON unmarshal logic:  
1. Decodes bytes into `overhead` (temporary struct).  
2. Copies that value into the receiver.  
3. Creates an auxiliary map to capture any remaining keys not listed in `documentedKeys`.  
4. Removes known keys from this map and assigns the rest to `n.Extra`.  
  
### 5. TODO: MarshalJSON  
A placeholder for future implementation of custom JSON marshaling, which will likely mirror the logic of `UnmarshalJSON` but in reverse.  
  
---  
  
This file supplies a complete NTP data structure with custom unmarshalling support, ready to be integrated into the larger `services` package.  
  
# models/services/redis.go  
**Package / Component**    
- **Name:** `services`    
- **Imports:** none (the file only declares types)  
  
---  
  
## External Data & Input Sources  
The code defines several Go structs that map to JSON payloads used by a Redis monitoring service. Each field carries a `json:"..."` tag, indicating the key name expected in the incoming data.  
  
| Struct | Key(s) | Description |  
|--------|--------|-------------|  
| `Redis` | `cpu`, `clients`, `cluster`, `keys`, `keyspace`, `memory`, `pacluster`, `persistence`, `replication`, `server`, `ssl`, `oom-prevention`, `stats` | Top‑level container for all service metrics. |  
| `RedisCpuData` | `used_cpu_sys`, `used_cpu_sys_children`, `used_cpu_user`, `used_cpu_user_children` | CPU usage statistics. |  
| `RedisKeys` | `data`, `more` | List of keys and a flag indicating if more keys exist beyond the initial request. |  
| `RedisServer` | `arch_bits`, `atomicvar_api`, …, `uptime_in_seconds` | Server configuration & runtime information. |  
| `RedisSSL` | `ssl_connections_to_current_certificate`, …, `ssl_enabled` | SSL connection statistics. |  
| `RedisOomPrevention` | `oom_prevention_on`, …, `peak_used_memory_total_human` | OOM prevention metrics and memory usage details. |  
  
---  
  
## TODOs  
No explicit `TODO:` comments are present in the file.  
  
---  
  
# Summary of Major Code Parts  
  
### 1. `Redis` struct    
The central data structure that aggregates all monitoring information for a Redis instance. It contains nested structs (`CPU`, `Keys`, `Server`, etc.) and several maps to hold dynamic key/value pairs (e.g., `memory`, `pacluster`). The JSON tags provide the exact field names expected in external payloads.  
  
### 2. `RedisCpuData` struct    
Captures CPU usage statistics for both system and user processes, including child process metrics. These values are later used to compute overall CPU load.  
  
### 3. `RedisKeys` struct    
Holds a slice of key identifiers (`data`) and a boolean flag (`more`) that signals whether additional keys exist beyond the current batch.  
  
### 4. `RedisServer` struct    
Describes server‑level configuration such as architecture bits, process ID, uptime, and various build/version fields. It also contains network parameters like TCP port and run identifier.  
  
### 5. `RedisSSL` struct    
Tracks SSL connection counts to both current and previous certificates, along with timestamps for certificate validity and a flag indicating whether SSL is enabled.  
  
### 6. `RedisOomPrevention` struct    
Provides metrics related to out‑of‑memory prevention: peak memory usage totals, thresholds, and human‑readable representations of memory statistics.  
  
---  
  
These structs together form the data model used by the `services` package for monitoring a Redis instance. The JSON tags ensure seamless marshaling/unmarshaling between Go structures and external JSON payloads.  
  
# models/services/rip.go  
# Package / Component    
**services**  
  
## Imports    
No external imports are declared in this file; all types are defined locally within the `services` package.  
  
## External Data / Input Sources    
The structs are intended for serializing/deserializing JSON data that represents a RIP (Routing Information Protocol) service. The fields carry JSON tags, indicating how they map to/from JSON objects:  
- `Addresses []RipAddress` → array of host/route entries  
- `Command int` → numeric command type  
- `Version int` → protocol version  
  
## TODOs    
No explicit TODO comments are present in the code.  
  
## Summary of Major Code Parts    
  
### 1. `RIP` struct    
Defines a high‑level representation of a RIP service instance:  
- **Addresses**: slice of `RipAddress`, each entry describing an individual host or route.  
- **Command**: integer flag indicating whether this is a request (1) or response (2).  
- **Version**: protocol version number.  
  
### 2. `RipAddress` struct    
Describes the details for a single address/route:  
- **Addr** (`string`) – textual representation of the host address.  
- **Family** (`interface{}`) – placeholder for an optional integer value; can be expanded later.  
- **Metric** (`int`) – numeric metric associated with this route.  
- **NextHop** (`interface{}`) – placeholder for next hop information.  
- **Subnet** (`*string`) – pointer to a subnet string, allowing nil values.  
- **Tag** (`*int`) – optional tag identifier.  
  
Both structs are annotated with JSON tags so they can be marshalled/unmarshalled automatically by Go’s `encoding/json` package. The file serves as the core data model for the services component of the application.  
  
# models/services/rsync.go  
## Package / Component    
**services**  
  
### Imports    
No external imports are declared in this file.  
  
### External data / input sources    
* `Authentication` – a boolean flag indicating whether the server requires authentication.    
* `Modules` – a map where each key is the name of a module (folder) and the value is an optional description; both fields are serialized to/from JSON using the tags `json:"authentication"` and `json:"modules"`.  
  
### TODOs    
No TODO comments are present in this file.  
  
### Summary of major code parts    
  
| Section | Description |  
|---------|-------------|  
| **Struct definition** | Declares a Go struct named `Rsync` that holds configuration data for an rsync‑like service. The struct contains two fields: `Authentication`, a boolean flag, and `Modules`, a map from module names to descriptions. Both fields are annotated with JSON tags so they can be marshalled/unmarshalled automatically. |  
  
The file defines the core data structure used by the *services* package; it will likely be instantiated elsewhere in the package to configure rsync operations.  
  
# models/services/smb.go  
**Package / Component**    
`services`  
  
---  
  
### Imports    
No explicit imports are declared in this file; the structs rely only on Go’s built‑in types and JSON tags.  
  
### External Data / Input Sources    
The struct fields use `json:"..."` tags, indicating that instances of these types will be marshalled/unmarshalled to/from JSON. The data is expected to come from SMB service discovery (e.g., banner grabbing) and subsequent share/file enumeration.  
  
### TODOs    
No TODO comments are present in the current code.  
  
---  
  
## Summary of Major Code Parts  
  
### `SMB` struct  
Represents a discovered SMB service instance.  
| Field | Type | JSON tag | Description |  
|-------|------|----------|-------------|  
| `Anonymous` | `bool` | `json:"anonymous"` | Indicates whether anonymous connections are allowed. |  
| `Capabilities` | `[]string` | `json:"capabilities"` | List of SMB features supported by the service. |  
| `OS` | `string` | `json:"os,omitempty"` | Operating system name (optional). |  
| `Raw` | `[]string` | `json:"raw"` | Hex‑encoded response packets captured during banner grabbing. |  
| `Shares` | `[]SmbShare` | `json:"shares,omitempty"` | Collection of shared directories/files discovered on the service. |  
| `SmbVersion` | `int` | `json:"smb_version"` | Highest SMB protocol version negotiated with the server. |  
| `Software` | `string` | `json:"software,omitempty"` | Name of the software powering the SMB service (optional). |  
  
### `SmbShare` struct  
Describes a single share exposed by an SMB service.  
| Field | Type | JSON tag | Description |  
|-------|------|----------|-------------|  
| `Comments` | `string` | `json:"comments"` | Share description/comments. |  
| `Files` | `[]SmbFile` | `json:"files,omitempty"` | List of files contained in the share (optional). |  
| `Name` | `string` | `json:"name"` | Name of the share. |  
| `Special` | `bool` | `json:"special"` | Flag indicating if the share is special (e.g., administrative). |  
| `Temporary` | `bool` | `json:"temporary"` | Flag indicating if the share is temporary. |  
| `Type` | `string` | `json:"type"` | Type of the share (e.g., disk, printer). |  
  
### `SmbFile` struct  
Represents a file or directory within an SMB share.  
| Field | Type | JSON tag | Description |  
|-------|------|----------|-------------|  
| `Directory` | `bool` | `json:"directory"` | Indicates if the entry is a directory. |  
| `Name` | `string` | `json:"name"` | File or directory name. |  
| `ReadOnly` | `bool` | `json:"read-only"` | Flag indicating read‑only status. |  
| `Size` | `int` | `json:"size"` | Size of the file in bytes. |  
  
These structs provide a lightweight, JSON‑serialisable model for SMB service discovery and share enumeration, suitable for use within a larger network‑analysis or monitoring package.  
  
# models/services/snmp.go  
# Package / Component    
**services**  
  
## Imports    
No external imports are declared in this file; the struct relies solely on Go’s built‑in types.  
  
## External data, input sources    
The `SNMP` struct is intended to model SNMP (Simple Network Management Protocol) information for a device. All fields are annotated with JSON tags so that instances of this struct can be marshalled/unmarshalled directly from/to JSON payloads.  
  
## TODO comments    
No explicit TODO markers appear in the source, but future work could include:  
- Adding validation logic or default values.  
- Implementing methods to marshal/unmarshal the struct.  
  
## Summary of major code parts    
  
### 1. Struct definition (`SNMP`)    
The file defines a single Go struct named `SNMP`. It contains eleven fields that represent SNMP data for a network device:  
  
| Field | JSON tag | Description |  
|-------|----------|-------------|  
| `Contact` | `"contact"` | Contact information of the device. |  
| `Description` | `"description"` | A textual description of the device. |  
| `Location` | `"location"` | Physical location (pointer to string). |  
| `Name` | `"name"` | Device name given by the owner (pointer to string). |  
| `Uptime` | `"uptime"` | Uptime value. |  
| `ObjectId` | `"objectid"` | Identifier for the SNMP object. |  
| `Services` | `"services"` | List of services exposed by the device. |  
| `OrLastChange` | `"orlastchange"` | Timestamp of last change. |  
| `OrDescr` | `"ordescr"` | Description of the OR (Object Repository). |  
| `OrUptime` | `"oruptime"` | Uptime for the OR. |  
| `OrId` | `"orid"` | Identifier for the OR. |  
  
All fields are exported, allowing external packages to read/write them directly. The JSON tags ensure that when this struct is serialized (e.g., via `encoding/json`) the field names match expected keys in a JSON payload.  
  
### 2. Field types and pointers    
`Location` and `Name` are defined as pointers (`*string`). This indicates that these fields may be optional or nullable; callers can check for nil before accessing them. The remaining fields use plain strings, implying they are required values.  
  
### 3. Potential extensions    
The struct could later be expanded with methods such as:  
- `func (s *SNMP) MarshalJSON() ([]byte, error)` – to customize JSON output.  
- Validation helpers or default value setters for optional fields.  
  
Overall, this file provides a clean data model for SNMP device information that can be used throughout the `services` package and beyond.  
  
# models/services/ssh.go  
**Package / Component**    
`services`  
  
---  
  
### Imports    
No explicit imports are declared in this file; the structs rely only on Go’s built‑in types.  
  
---  
  
### External Data & Input Sources    
The two structs expose JSON field names via struct tags, indicating that they are intended to be marshalled/unmarshalled from/to JSON payloads.    
* `SSH` fields: `cipher`, `fingerprint`, `hassh`, `kex`, `key`, `mac`, `type`.    
* `SshKex` fields: `compression_algorithms`, `encryption_algorithms`, `kex_algorithms`, `kex_follows`, `languages`, `mac_algorithms`, `server_host_key_algorithms`, `unused`.  
  
---  
  
### TODOs    
No TODO comments are present in the current code.  
  
---  
  
## Summary of Major Code Parts    
  
#### 1. `SSH` struct    
*Purpose*: Represents an SSH service configuration, capturing all key parameters needed for establishing a connection.    
*Key fields*    
- **Cipher** – encryption algorithm used during negotiation.    
- **Fingerprint** – unique identifier for the service.    
- **Hassh** – (likely a typo or custom field) holds additional SSH data.    
- **Kex** – nested `SshKex` struct containing key‑exchange details.    
- **Key** – public key string.    
- **Mac** – message authentication code algorithm.    
- **Type** – type of the service (e.g., server, client).    
  
#### 2. `SshKex` struct    
*Purpose*: Encapsulates all parameters related to SSH key‑exchange and session configuration.    
*Key fields*    
- **CompressionAlgorithms** – list of supported compression methods.    
- **EncryptionAlgorithms** – list of supported encryption algorithms.    
- **KexAlgorithms** – list of supported key‑exchange algorithms.    
- **KexFollows** – boolean flag indicating whether the key exchange follows a particular protocol sequence.    
- **Languages** – array of language identifiers for SSH negotiation.    
- **MacAlgorithms** – list of MAC algorithms.    
- **ServerHostKeyAlgorithms** – host‑key algorithm options.    
- **Unused** – placeholder integer (currently unused).    
  
These structs together provide a comprehensive representation of an SSH service’s capabilities and configuration, ready to be serialized into JSON for storage or transmission.  
  
# models/services/vertx.go  
## Package / Component    
**services**  
  
### Imports  
No external imports are declared in this file.  
  
### External Data / Input Sources    
The `Vertx` struct represents a device’s firmware and network configuration data that can be serialized to/from JSON. It contains:  
  
- Firmware release date (`FirmwareData`)  
- Firmware version (`FirmwareVersion`)  
- Local IP address (`InternalIP`)  
- MAC address (`MAC`)  
- Door controller name (`Name`)  
- Product type (`Type`)  
  
### TODOs    
No `TODO:` comments are present in the current code.  
  
## Summary of Major Code Parts    
  
### Struct Definition  
The file defines a single Go struct named **Vertx**. It is intended to hold all relevant information about a device’s firmware and network state, with JSON tags for each field so that it can be marshalled/unmarshalled easily.  
  
### Field Descriptions    
| Field | Purpose | JSON Tag |  
|-------|---------|----------|  
| `FirmwareData` | Date the firmware was released | `"firmware_data"` |  
| `FirmwareVersion` | Firmware version identifier | `"firmware_version"` |  
| `InternalIP` | Local IP address of the device | `"internal_ip"` |  
| `MAC` | MAC address of the device | `"mac"` |  
| `Name` | Name of the door controller | `"name"` |  
| `Type` | Product type of the device | `"type"` |  
  
The struct is ready for use in other parts of the package, such as configuration handling or API communication.  
  
