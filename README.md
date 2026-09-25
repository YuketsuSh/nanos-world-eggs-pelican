# nanos world — Pelican Egg

An open-source **Pelican egg** for hosting a **nanos world Dedicated Server** on Linux.

Maintained by **Yuketsu** — GitHub: **YuketsuSh**.

## Features

- SteamCMD installation and validation
- Official nanos world Dedicated Server App ID `1936830`
- `public` and `bleeding-edge` branches
- Linux startup through `NanosWorldServer.sh`
- Separate game/HTTP and query ports
- Dedicated or Steam Datagram Relay/P2P networking
- Server-list name, description, language flag and logo
- Password and private Vault token support
- Map, game mode, packages, assets and loading-screen configuration
- Automatic Vault downloads
- Tick, sync, bandwidth, compression and distance-optimization controls
- Async logging, profiling and thread-pool controls
- Optional custom settings
- Unsafe Lua libraries disabled by default
- Startup validation for conflicting ports and transfer-rate settings

## Requirements

nanos world currently supports Linux on:

- Ubuntu 24.04
- Ubuntu 22.04
- Debian 13

The server is x86-64 oriented. Older Debian/Ubuntu releases and ARM are not the target of this egg.

## Network allocations

The default nanos world ports are:

| Purpose | Port | Protocol |
| --- | ---: | --- |
| Game traffic | `7777` | UDP |
| Built-in HTTP server | `7777` | TCP |
| Server query | `7778` | UDP |

In Pelican, assign the primary server allocation to the game port and expose it over **TCP and UDP**. Add a second allocation for the query port over **UDP**.

`QUERY_PORT` must not equal `SERVER_PORT`.

## Installation

1. Download `egg-nanos-world-latest.json`.
2. Open your Pelican administration panel.
3. Import the egg into the desired nest/category.
4. Create a server from the imported egg.
5. Configure the primary allocation for the game/HTTP port.
6. Add the UDP query allocation and set `QUERY_PORT` to that port.
7. Start the server.

The installer downloads SteamCMD and installs/validates nanos world through Steam App ID `1936830`.

## Release branches

`SRCDS_BETAID` supports:

- `public` — stable/default branch
- `bleeding-edge` — preview branch

`public` is recommended unless you explicitly need preview builds.

## Important variables

| Variable | Default | Description |
| --- | --- | --- |
| `QUERY_PORT` | `7778` | UDP server-query port |
| `SERVER_NAME` | `nanos world - Pelican Server` | Public server name |
| `SERVER_LANGUAGE` | `global` | Server-list language/country flag |
| `STEAM_APP` | `playtest` | nanos world Steam App |
| `ANNOUNCE` | `1` | Announce in server list |
| `DEDICATED_SERVER` | `1` | Dedicated networking |
| `SERVER_MAP` | `default-blank-map` | Startup map |
| `MAX_PLAYERS` | `64` | Player limit |
| `MAX_TICK_RATE` | `30` | Server tick rate |
| `MAX_SYNC_RATE` | `30` | Actor sync rate |
| `MAX_SEND_RATE` | `1024` | Per-client send rate in KB/s |
| `MAX_FILE_TRANSFER_RATE` | `1024` | Per-client file transfer rate in KB/s |
| `COMPRESSION` | `1` | Network compression |
| `DISTANCE_OPTIMIZATION` | `4` | Distance-based network optimization |

The egg exposes additional advanced settings directly through Pelican.

## Security

`SERVER_TOKEN` is configured as a non-viewable Pelican variable because it can authorize private Vault downloads.

`ENABLE_UNSAFE_LIBS` defaults to `0`. Enabling it exposes Lua APIs capable of OS command execution and filesystem operations. Only enable it for packages you fully trust.

## Updating

Restart/reinstall according to your Pelican deployment workflow to run SteamCMD validation against the selected release branch.

Before changing from `public` to `bleeding-edge`, back up your server data.

## Troubleshooting

### Server does not start

Verify that `NanosWorldServer.sh` exists and is executable. The official Linux server must be launched through this shell script.

### Server is not visible in the list

Check that:

- `ANNOUNCE=1`
- the game port is reachable over UDP
- the same game port is reachable over TCP for the built-in HTTP server
- `QUERY_PORT` is reachable over UDP
- the query port differs from the game port

### Packages or assets do not download

Enable `AUTO_DOWNLOAD`. Private/owned Vault content may additionally require a valid `SERVER_TOKEN`.

### Performance

The upstream defaults are intentionally conservative. Start with `30` tick/s, `30` sync/s, `1024` KB/s send rate, `1024` KB/s file-transfer rate, compression `1`, and distance optimization `4`; tune only after measuring your workload.

## Upstream documentation

- nanos world Server Installation: https://docs.nanos-world.com/docs/core-concepts/server-manual/server-installation
- nanos world Server Configuration: https://docs.nanos-world.com/docs/core-concepts/server-manual/server-configuration
- Pelican Panel: https://pelican.dev/

This project is a community integration and is not an official nanos world or Pelican project.

## License

Released under the **MIT License**.

Copyright (c) 2026 Yuketsu.

See [`LICENSE`](LICENSE).
