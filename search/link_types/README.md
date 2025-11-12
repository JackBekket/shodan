# Package **link_types**

## Overview  
`search/link_types/link_types.go` defines a simple type `LinkType` and a set of exported constants that represent common network link types. The package is intended to be imported by other parts of the application (e.g., configuration generators, CLI tools) when they need to refer to or compare link types in a type‑safe way.

## Environment / Configuration  
No external environment variables, flags, or command‑line arguments are declared directly in this file; however, any package that imports `link_types` can use the exported constants as part of its own configuration logic.

## File structure  

```
search/
└── link_types/
    └── link_types.go
```

### `link_types.go`
* **Type declaration** – `type LinkType string`.  
* **Constants** – eleven values (`EthernetOrModem`, `TunnelOrVPN`, …, `AX25`) that are all of type `LinkType`.  

These constants can be referenced elsewhere as `link_types.EthernetOrModem`, etc.

## Relations & Usage  
- The constants provide a single source of truth for link‑type strings; other packages can import this file and use the values without hard‑coding literal strings.  
- Because all constants are exported, they can be used in configuration files or command‑line flags that expect a string representation of a link type.

## Edge cases / Launch scenarios  
If the application contains a CLI entry point (e.g., `cmd/main.go`) that accepts a flag such as `--link-type`, it could use one of these constants to validate or default the value. The package itself does not provide any executable logic, so launching is only relevant for packages that import it.

---