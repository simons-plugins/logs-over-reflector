# LogsOverReflector

Exposes the Indigo event log as a JSON API over Reflector, enabling remote log viewing with pagination, filtering, and historical date access.

Provides a REST API over the Indigo Reflector for accessing event log data remotely. Supports the current live log and historical dated log files with pagination, source filtering, and text search. Designed as a companion to the [Home Remote for Indigo](https://github.com/simons-plugins/Home-Remote-for-Indigo) iOS app, but usable by any HTTP client.

## Endpoints

| Endpoint | Description |
|----------|-------------|
| `log` | Current event log with pagination and filtering |
| `sources` | Distinct log source names |
| `history` | Historical entries from dated log files |
| `dates` | List of available log dates |

## Usage

All endpoints are accessed via the Indigo Reflector message API:

```
GET /message/com.simons-plugins.logs-over-reflector/<endpoint>
```

### Query Parameters

**`log`** and **`history`**:
- `lines` — number of entries to return (default 500, max 5000)
- `offset` — number of most-recent entries to skip (for pagination)
- `source` — filter by source name
- `search` — text search in message field

**`history`** additionally requires:
- `date` — date in `YYYY-MM-DD` format

### Examples

```
# Fetch latest 500 log entries
GET /message/com.simons-plugins.logs-over-reflector/log

# Fetch 100 entries, skipping the most recent 200
GET /message/com.simons-plugins.logs-over-reflector/log?lines=100&offset=200

# Search for entries from a specific source
GET /message/com.simons-plugins.logs-over-reflector/log?source=Z-Wave&search=error

# Fetch historical log for a specific date
GET /message/com.simons-plugins.logs-over-reflector/history?date=2026-02-15

# List available log dates
GET /message/com.simons-plugins.logs-over-reflector/dates

# List distinct log sources
GET /message/com.simons-plugins.logs-over-reflector/sources
```

## Installation

Install from the Indigo Plugin Store, or copy `LogsOverReflector.indigoPlugin` to your Indigo plugins folder.

## Requirements

- Indigo 2023.2 or later
- Reflector access enabled
