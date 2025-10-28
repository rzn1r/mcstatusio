# MCStatusio
[![Tests](https://github.com/rzn1r/mcstatusio/actions/workflows/test.yml/badge.svg)](https://github.com/rzn1r/mcstatusio/actions/workflows/test.yml)
[![Lint](https://github.com/rzn1r/mcstatusio/actions/workflows/lint.yml/badge.svg)](https://github.com/rzn1r/mcstatusio/actions/workflows/lint.yml)
![PyPI - Version](https://img.shields.io/pypi/v/mcstatusio)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/mcstatusio)

A Python wrapper for [mcstatus](httos://mcstatus.io/) API. 

## Installation
You can install the package via pip:
```bash
pip install mcstatusio
```

## Usage

### Bedrock Server Status

```python 
from mcstatusio import BedrockServer

server = BedrockServer("play.example.com", 19132)
status = server.status()

if not status.online:
    print("The server is offline.")
else:
    print(f"The server has {status.players.online} players online.")

```

### Java Server Status

```python
from mcstatusio import JavaServer

server = JavaServer("play.example.com", 25565)
status = server.status()

if not status.online:
    print("The server is offline.")
else:
    print(f"The server has {status.players.online} players online.")
```

## Async Usage

### Bedrock Server Status

```python 
import asyncio
from mcstatusio import BedrockServer

server = BedrockServer("play.example.com", 19132)
status = asyncio.run(server.async_status())

if not status.online:
    print("The server is offline.")
else:
    print(f"The server has {status.players.online} players online.")

```

### Java Server Status

```python
import asyncio
from mcstatusio import JavaServer

server = JavaServer("play.example.com", 25565)
status = asyncio.run(server.async_status())

if not status.online:
    print("The server is offline.")
else:
    print(f"The server has {status.players.online} players online.")
```

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details

## Contributing
Contributions are welcome! Please open an issue or submit a pull request on GitHub.
