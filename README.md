# Meshing-Around WebGUI (Standalone Addon)

Web-based configuration and monitoring interface for the [Meshing-Around](https://github.com/spudgunman/meshing-around) Meshtastic BBS bot.

**This is the standalone addon version** - drop it into your existing meshing-around installation.

For the full integrated version with additional features, see: [Meshing-Around-WebGUI-Integrated](https://forge.echo6.co/matt/Meshing-Around-WebGUI-Integrated)

## Features

| Feature | Status | Notes |
|---------|--------|-------|
| Dashboard | ✅ | Service status, quick stats |
| Radio Connections | ✅ | Configure up to 9 interfaces (Serial/TCP/BLE) |
| Scheduler | ✅ | Manage scheduled broadcasts |
| Mesh Nodes | ✅ | View all nodes on the mesh |
| BBS Network | ✅ | Track BBS peer sync status |
| System Logs | ✅ | Filterable log viewer |
| Configuration | ✅ | Edit all config.ini sections |
| Leaderboard | ⚠️ | Shows hex IDs (names require integrated version) |
| Packet Monitor | ❌ | Requires integrated version |

## Installation

### 1. Copy files to your meshing-around installation

```bash
# Clone or download this repo
git clone https://forge.echo6.co/matt/Meshing-Around-WebGUI.git

# Copy webgui folder to your meshing-around directory
cp -r Meshing-Around-WebGUI/* /path/to/meshing-around/webgui/
```

### 2. Add to docker-compose.yml

Add this service to your `compose.yaml` or `docker-compose.yml`:

```yaml
  meshing-webgui:
    build:
      context: ./webgui
      dockerfile: Dockerfile
    container_name: meshing-webgui
    restart: unless-stopped
    ports:
      - "8085:8085"
    volumes:
      - ./config.ini:/app/config.ini:ro
      - ./data:/app/data
      - ./logs:/app/logs:ro
      - ./webgui/schedules.json:/app/schedules.json
    environment:
      - CONFIG_PATH=/app/config.ini
      - DATA_PATH=/app/data
      - LOGS_PATH=/app/logs
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8085/health"]
      interval: 30s
      timeout: 10s
      retries: 3
```

### 3. Start the WebGUI

```bash
docker-compose up -d meshing-webgui
```

Access at: `http://your-server:8085`

## Files

```
webgui/
├── main.py              # FastAPI application
├── config_schema.py     # Configuration field definitions
├── Dockerfile           # Container build instructions
├── requirements.txt     # Python dependencies
├── templates/
│   └── index.html       # Single-page application (Tailwind CSS)
├── static/              # Static assets
└── schedules.json       # Scheduler data (created on first run)
```

## Integrated Version

For full functionality including Packet Monitor and node names on leaderboard, use the integrated version which includes modifications to mesh_bot.py and system.py:

- **Forge:** https://forge.echo6.co/matt/Meshing-Around-WebGUI-Integrated
- **GitHub:** https://github.com/zvx-echo6/Meshing-Around-WebGUI-Integrated

## License

MIT License - Same as meshing-around upstream.
