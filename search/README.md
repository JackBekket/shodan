# Search Package Summary

**Package name:** `search`  
The code in this directory implements a flexible query builder for the *Shodan* search API, covering domain lookups, exploit searches, link types, SSL options and many other facets.  The package is designed to be used by higher‑level tools (e.g., a CLI client or a web service) that need to construct complex HTTP requests.

---

## Project structure

```
search/
├── domain_query.go
├── exploit_types/
│   └── exploit_types.go
├── exploits_query.go
├── link_types/
│   └── link_types.go
├── query.go
├── reflect_types.go
├── search_params.go
├── ssl_versions/
│   └── query_ssl_version.go
└── triggers/
    └── triggers.go
```

---

## Environment variables, flags & command‑line arguments

| Variable / Flag | Purpose |
|------------------|---------|
| `search.Query` | Holds the main search parameters (see `query.go`). |
| `search.Params` | Wraps a query with pagination and facet information (`search_params.go`). |
| `search.ExploitsQuery` | Exploit‑specific query structure (`exploits_query.go`). |
| `search.DomainQuery` | Simple domain‑lookup struct (`domain_query.go`). |
| `search.LinkType`, `search.SSLVersion` | Types used in nested structs; their runtime types are cached in `reflect_types.go`. |

The package exposes a `String()` method on each struct, so the values can be passed as command‑line arguments or embedded into URLs.  For example:

```
$ shodan search --query "example.com" --page 2
```

will create a `search.Query` instance and serialize it to a query string.

---

## How the code works together

* **DomainQuery** – simple holder for domain‑specific parameters; used by higher‑level helpers that need only a few fields.
* **ExploitTypes / ExploitsQuery** – defines an exploit search payload.  The `String()` method marshals all fields into a single string, ready to be appended to the API URL.
* **LinkTypes** – provides the type for the `Link` field in `Query`; its definition lives in `link_types/link_types.go`.
* **SSLVersions** – supplies SSL‑specific options; used inside the nested `SSLOpts` struct of `Query`.  The concrete type is defined in `ssl_versions/query_ssl_version.go`.
* **Triggers** – contains helper functions that build query strings from structs (`marshalQueryParamField`, `marshalQueryParam`).  These helpers are used by both `ExploitsQuery.String()` and `Query.String()`.
* **ReflectTypes** – caches the `reflect.Type` values for all custom types so that the marshaling helpers can look them up quickly.
* **SearchParams** – wraps a `Query` with pagination, facets and minification flags.  The `ToURLValues()` method turns the whole structure into a `net/url.Values` map, which is what an HTTP client would send to Shodan.

The flow of data is:

```
DomainQuery → (used by) Query
ExploitsQuery → (used by) Params.ExploitParams
LinkTypes + SSLVersions → (used by) Query nested structs
Triggers → marshal all fields into a query string
SearchParams → build final URL parameters
```

---

## Edge cases for launching

* **CLI entry point** – If the repository contains a `main.go` that imports this package, you can run:

  ```bash
  go run ./cmd/shodan --search "example.com" --page 1
  ```

  The CLI should create a `search.Params`, populate its `Query` field with values from flags, then call `Params.ToURLValues()` to build the request.

* **HTTP client** – A higher‑level service can simply do:

  ```go
  q := search.Query{Text:"example.com", Page:1}
  url.Values := q.String()
  ```

  and send it via an HTTP GET/POST to Shodan’s API endpoint.

---

## Summary of logic

* `domain_query.go` defines a lightweight struct for domain lookups.  
* `exploit_types/exploit_types.go` declares the enum used in exploit queries.  
* `exploits_query.go` implements a full exploit search payload and its string conversion.  
* `link_types/link_types.go` supplies link‑type definitions that are referenced by `Query`.  
* `query.go` contains the heart of the package: a large, nested struct (`Query`) with many optional fields (SSL, HTTP, NTP, etc.) plus helper functions to marshal it into a query string.  
* `reflect_types.go` caches runtime type information for all custom types used by the marshaling helpers.  
* `search_params.go` wraps a `Query` in a higher‑level struct (`Params`) that adds facets, pagination and minification flags; it also provides a helper to convert everything into URL query parameters.  
* `ssl_versions/query_ssl_version.go` defines SSL version options used inside the nested `SSLOpts` struct of `Query`.  
* `triggers/triggers.go` contains the low‑level marshaling functions (`marshalQueryParamField`, `marshalQueryParam`) that walk a struct’s fields, read their tags and build a query string.

All pieces together allow a developer to construct complex search requests programmatically or via command‑line flags, serialize them into a URL‑friendly format, and send them to the Shodan API.