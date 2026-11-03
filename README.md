<p align="center">
  <a href="https://raw.githubusercontent.com/BasaltProject/.github/main/assets/basalt-logo-white.svg#gh-dark-mode-only">
    <img src="https://raw.githubusercontent.com/BasaltProject/.github/main/assets/basalt-logo-white.svg" alt="Basalt" width="360">
  </a>
  <a href="https://raw.githubusercontent.com/BasaltProject/.github/main/assets/basalt-logo-black.svg#gh-light-mode-only">
    <img src="https://raw.githubusercontent.com/BasaltProject/.github/main/assets/basalt-logo-black.svg" alt="Basalt" width="360">
  </a>
</p>

<h2 align="center">Basalt Bridge</h2>

<p align="center">
  <strong>Run Basalt plugins on JVM servers and proxies. One Lua project, both distributions.</strong>
</p>

<p align="center">
  <img alt="Kotlin" src="https://img.shields.io/badge/Kotlin-JVM-7F52FF?logo=kotlin&logoColor=white">
  <img alt="Lua" src="https://img.shields.io/badge/plugins-Lua-000080?logo=lua&logoColor=white">
  <img alt="Gradle" src="https://img.shields.io/badge/build-Gradle-02303A?logo=gradle&logoColor=white">
  <img alt="License" src="https://img.shields.io/badge/license-GPL--3.0--only-green">
</p>

## Overview

Basalt Bridge is a compatibility layer written in Kotlin that runs Basalt's Lua plugins on
Bukkit, Spigot, Paper, Velocity and BungeeCord through [Luak](https://github.com/BluevaDevelopment/Luak).

It provides two ways to distribute a plugin: install Bridge on a server or proxy and load Basalt
packages, or use its Gradle integration to build a platform jar and a native Basalt package from
the same Lua project.

| | |
|---|---|
| Language | Kotlin, running on the JVM |
| Lua runtime | Luak |
| Server platforms | Bukkit, Spigot, Paper |
| Proxy platforms | Velocity, BungeeCord |
| Distribution | Platform `.jar` and native `.bspkg` or `.bppkg` packages |
| Build integration | Gradle |
| License | GPL-3.0-only |

## Status

**Design stage.** This repository currently contains the initial documentation and license.
The platform adapters, package loader and Gradle integration are not implemented yet.
The workflows below describe the intended behavior. Build commands, Maven coordinates and
supported platform versions will be documented once they are available.

## Running packages with Bridge

Bridge is intended to install as a plugin on the host server or proxy:

1. Place the Bridge jar for your platform in `plugins/`.
2. Start the server or proxy to initialize Bridge.
3. Drop your Basalt packages into Bridge's package directory.
4. Restart to validate and load the plugins through Luak.

Server plugins use `.bspkg`; proxy plugins use `.bppkg`. Bridge maps their Basalt API calls to the
host platform. The package directory and Java requirements will be defined with the first implementation.

| Host | Package |
|---|---|
| Bukkit, Spigot, Paper | `.bspkg` |
| Velocity, BungeeCord | `.bppkg` |
| Basalt Server, without Bridge | `.bspkg` |
| Basalt Proxy, without Bridge | `.bppkg` |

Server and proxy plugins target different APIs. Supporting both requires code for each target;
renaming a package does not convert it. API coverage will be documented per platform and feature.

## Building with Gradle

The planned Gradle integration keeps Lua sources, resources and plugin metadata in one project
and produces two artifacts for the selected target:

- A `.jar` with the adapter and metadata required by the JVM platform.
- A `.bspkg` for Basalt Server or a `.bppkg` for Basalt Proxy.

Whether the jar bundles the runtime or requires a shared Bridge installation is still to be decided.
The Gradle configuration and tasks will be documented when implemented.
Native packages must preserve Basalt's package format and API contracts, so they run directly on
Basalt Server or Basalt Proxy without adding a JVM dependency to either product.

## Related projects

[Basalt Server](https://github.com/BasaltProject/Server) and
[Basalt Proxy](https://github.com/BasaltProject/Proxy) define the native APIs Bridge will adapt.
[Kiln](https://github.com/BasaltProject/Kiln) verifies and packages their native plugins.

[Luak](https://github.com/BluevaDevelopment/Luak) provides the Lua interpreter and compiler in Kotlin.
[Mawu](https://github.com/BluevaDevelopment/Mawu) integrates Lua sources into Gradle projects and
packages them for Luak. It is a reference for the build integration; adopting it as a Bridge
dependency is still under evaluation.

Bridge runs Basalt plugins on JVM platforms. Running Java plugins inside Basalt's native products
is handled by their integrations with [Shim](https://github.com/BasaltProject/Shim).

## License

GPL-3.0-only. See [LICENSE](LICENSE).
