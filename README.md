# MCStatusio

[![Tests](https://github.com/rzn1r/mcstatusio/actions/workflows/test.yml/badge.svg)](https://github.com/rzn1r/mcstatusio/actions/workflows/test.yml)
[![Lint](https://github.com/rzn1r/mcstatusio/actions/workflows/lint.yml/badge.svg)](https://github.com/rzn1r/mcstatusio/actions/workflows/lint.yml)
![PyPI - Version](https://img.shields.io/pypi/v/mcstatusio)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/mcstatusio)

A Python wrapper for the [mcstatus.io](https://mcstatus.io/) API that provides an easy way to query Minecraft server status for both Java Edition and Bedrock Edition servers. 

## Features

- 🎮 Support for both Java Edition and Bedrock Edition servers
- 🔄 Synchronous and asynchronous API support
- 📊 Comprehensive server information (players, version, MOTD, etc.)
- 🚀 Simple and intuitive interface
- 🔧 Type hints for better IDE support
- ⚡ Lightweight with minimal dependencies

## Installation

**Requirements:** Python 3.11 or higher

You can install the package via pip:

```bash
pip install mcstatusio
```

## Quick Start

### Java Edition Server

```python
from mcstatusio import JavaServer

# Create a server instance (default port is 25565)
server = JavaServer("mc.hypixel.net")
status = server.status()

if status.online:
    print(f"Server is online!")
    print(f"Players: {status.players.online}/{status.players.max}")
    print(f"Version: {status.version.name.clean}")
    print(f"MOTD: {status.motd.clean}")
else:
    print("Server is offline")
```

### Bedrock Edition Server

```python
from mcstatusio import BedrockServer

# Create a server instance (default port is 19132)
server = BedrockServer("play.cubecraft.net")
status = server.status()

if status.online:
    print(f"Server is online!")
    print(f"Players: {status.players.online}/{status.players.max}")
    print(f"Version: {status.version.name}")
    print(f"Edition: {status.edition}")
else:
    print("Server is offline")
```

## Usage Examples

### Custom Port

You can specify a custom port either in the hostname or as a parameter:

```python
from mcstatusio import JavaServer

# Method 1: Include port in hostname
server = JavaServer("play.example.com:25566")

# Method 2: Pass port as parameter
server = JavaServer("play.example.com", port=25566)

status = server.status()
```

### Accessing Server Information

```python
from mcstatusio import JavaServer

server = JavaServer("mc.hypixel.net")
status = server.status()

if status.online:
    # Player information
    print(f"Players online: {status.players.online}")
    print(f"Max players: {status.players.max}")
    
    # Version information
    print(f"Version: {status.version.name.clean}")
    print(f"Protocol: {status.version.protocol}")
    
    # Message of the Day
    print(f"MOTD (clean): {status.motd.clean}")
    print(f"MOTD (raw): {status.motd.raw}")
    print(f"MOTD (HTML): {status.motd.html}")
    
    # Server details
    print(f"Hostname: {status.hostname}")
    print(f"IP Address: {status.ip_address}")
    print(f"Port: {status.port}")
    
    # Optional fields (may be None)
    if status.software:
        print(f"Software: {status.software}")
    if status.plugins:
        print(f"Plugins: {len(status.plugins)}")
```

### Error Handling

```python
from mcstatusio import JavaServer
import requests

server = JavaServer("invalid.server.address")

try:
    status = server.status()
    if status.online:
        print("Server is online")
    else:
        print("Server is offline")
except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.RequestException as e:
    print(f"Error querying server: {e}")
```

### Async Usage

For asynchronous applications, use the `async_status()` method:

```python
import asyncio
from mcstatusio import JavaServer

async def check_server():
    server = JavaServer("mc.hypixel.net")
    status = await server.async_status()
    
    if status.online:
        print(f"Players online: {status.players.online}")
    return status

# Run the async function
status = asyncio.run(check_server())
```

### Query Multiple Servers

```python
import asyncio
from mcstatusio import JavaServer

async def check_multiple_servers():
    servers = [
        JavaServer("mc.hypixel.net"),
        JavaServer("play.cubecraft.net"),
        JavaServer("mineplex.com"),
    ]
    
    # Query all servers concurrently
    statuses = await asyncio.gather(*[s.async_status() for s in servers])
    
    for server, status in zip(servers, statuses):
        if status.online:
            print(f"{server.hostname}: {status.players.online} players online")
        else:
            print(f"{server.hostname}: Offline")

asyncio.run(check_multiple_servers())
```

### Custom Timeout

```python
from mcstatusio import JavaServer

# Set a custom timeout (default is 5 seconds)
server = JavaServer("mc.hypixel.net", timeout=10)
status = server.status()
```

## API Reference

### JavaServer

**Constructor:**
- `JavaServer(hostname: str, port: int = 25565, timeout: int = 5)`

**Methods:**
- `status()`: Returns server status synchronously
- `async_status()`: Returns server status asynchronously

**Response Attributes (when online):**
- `online: bool` - Whether the server is online
- `players.online: int` - Number of players currently online
- `players.max: int` - Maximum number of players
- `players.sample: list | None` - Sample of online players (if available)
- `version.name.clean: str` - Clean version string
- `version.protocol: int` - Protocol version number
- `motd.clean: str` - Message of the Day (plain text)
- `motd.raw: str` - MOTD with formatting codes
- `motd.html: str` - MOTD in HTML format
- `hostname: str` - Server hostname
- `ip_address: str` - Server IP address
- `port: int` - Server port
- `icon: str | None` - Base64-encoded server icon
- `software: str | None` - Server software (e.g., "Paper", "Spigot")
- `plugins: list | None` - List of plugins (if available)
- `mods: list | None` - List of mods (if available)

### BedrockServer

**Constructor:**
- `BedrockServer(hostname: str, port: int = 19132, timeout: int = 5)`

**Methods:**
- `status()`: Returns server status synchronously
- `async_status()`: Returns server status asynchronously

**Response Attributes (when online):**
- `online: bool` - Whether the server is online
- `players.online: int` - Number of players currently online
- `players.max: int` - Maximum number of players
- `version.name: str` - Version string
- `version.protocol: int` - Protocol version number
- `motd.clean: str` - Message of the Day (plain text)
- `gamemode: str | None` - Server gamemode (e.g., "Survival", "Creative")
- `server_id: str | None` - Unique server identifier
- `edition: str | None` - Server edition ("MCPE" or "MCEE")
- `ip_address: str` - Server IP address
- `port: int` - Server port

## Documentation

Full documentation is available at [Read the Docs](https://mcstatusio.readthedocs.io/).

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request on GitHub.

## Support

- 📖 [Documentation](https://mcstatusio.readthedocs.io/)
- 🐛 [Issue Tracker](https://github.com/rzn1r/mcstatusio/issues)
- 💬 [Discussions](https://github.com/rzn1r/mcstatusio/discussions)
