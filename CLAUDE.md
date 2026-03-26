# CLAUDE.md — Logs Over Reflector

Indigo plugin that exposes the event log as a JSON REST API over the Indigo Reflector.

## Versioning & Release

### Version bump is required for every PR

The `PluginVersion` in `LogsOverReflector.indigoPlugin/Contents/Info.plist` must be bumped in every PR. CI runs a version-check that fails if the version already exists as a git tag. **Do not merge with failing checks.**

Version format: `YYYY.R.patch` (e.g. `2026.0.4`). Bump the patch for fixes/docs, minor for features.

On merge to main, the `create-release` workflow automatically creates a GitHub release with a `.zip` bundle of the plugin.

### PR checklist

1. Bump `PluginVersion` in `Info.plist`
2. Push and create PR
3. Wait for version-check CI to pass
4. Merge only after all checks are green

## Plugin Overview

- **Bundle ID**: `com.simons-plugins.logs-over-reflector`
- **Plugin ID**: `com.simons-plugins.logs-over-reflector`
- **Python**: 3.10+ (Indigo 2023+)

### Endpoints

All endpoints served via Indigo Reflector message API:

| Endpoint | Description |
|----------|-------------|
| `log` | Current event log with pagination and filtering |
| `sources` | Distinct log source names |
| `history` | Historical entries from dated log files |
| `dates` | List of available log dates |

### Query Parameters

- `lines` — number of entries (default 500, max 5000)
- `offset` — skip N most-recent entries (pagination)
- `source` — filter by source name
- `search` — text search in message field
- `date` — (history only) date in `YYYY-MM-DD` format

## Plugin Structure

```
LogsOverReflector.indigoPlugin/
└── Contents/
    ├── Info.plist
    └── Server Plugin/
        ├── plugin.py          # HTTP handlers (log, sources, history, dates)
        ├── Actions.xml        # Action definitions for HTTP endpoints
        └── PluginConfig.xml   # Plugin preferences
```

## Testing

```bash
# Copy to Indigo server
cp -r "LogsOverReflector.indigoPlugin" "/Volumes/Macintosh HD-1/Library/Application Support/Perceptive Automation/Indigo 2025.1/Plugins/"
```

Then reload via MCP: `mcp__indigo__restart_plugin(plugin_id="com.simons-plugins.logs-over-reflector")`
