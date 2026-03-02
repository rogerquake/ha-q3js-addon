# Q3JS - Quake III Arena Add-on

Runs a Quake III Arena dedicated server (`ioq3ded`) inside Home Assistant,
accessible from any browser via the companion Lovelace card.

## Game assets

Copy your `pak*.pk3` files to `/share/q3js/baseq3/` on your HA host before starting.
The add-on copies them automatically on start. Shareware demo assets are sufficient
for basic play (Episode 1 only).

## Configuration

| Option | Default | Description |
|--------|---------|-------------|
| `map_name` | `q3dm17` | Map to load on startup |
| `frag_limit` | `30` | Frags needed to win |
| `time_limit` | `20` | Time limit in minutes (0 = unlimited) |
| `rcon_password` | `homeassistant` | Remote console password |
| `spectator_name` | `HA_Spectator` | Name used when the card auto-spectates |
| `bots` | Keel + Sarge | List of bots: `name` and `skill` (1–5) |

## Ports

| Port | Protocol | Purpose |
|------|----------|---------|
| `27960` | UDP | Quake 3 game traffic |
| `27961` | TCP | WebSocket proxy (browser clients) |
| `8090` | TCP | Metrics HTTP API (used by the integration) |

## Lovelace card & integration

Install the companion HACS integration from:
`https://github.com/rogerquake/ha-q3js`
