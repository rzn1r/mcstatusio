Usage
=====

This guide shows you how to use mcstatusio to query Minecraft server status.

Basic Usage
-----------

Java Server Status
~~~~~~~~~~~~~~~~~~

.. code-block:: python

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


Bedrock Server Status
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

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


Advanced Usage
--------------

Custom Port
~~~~~~~~~~~

You can specify a custom port either in the hostname or as a parameter:

.. code-block:: python

    from mcstatusio import JavaServer

    # Method 1: Include port in hostname
    server = JavaServer("play.example.com:25566")

    # Method 2: Pass port as parameter
    server = JavaServer("play.example.com", port=25566)

    status = server.status()


Accessing Response Data
~~~~~~~~~~~~~~~~~~~~~~~~

The response object contains comprehensive server information:

.. code-block:: python

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


Error Handling
~~~~~~~~~~~~~~

Handle potential errors when querying servers:

.. code-block:: python

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


Custom Timeout
~~~~~~~~~~~~~~

Set a custom timeout for API requests (default is 5 seconds):

.. code-block:: python

    from mcstatusio import JavaServer

    # Set a custom timeout of 10 seconds
    server = JavaServer("mc.hypixel.net", timeout=10)
    status = server.status()


Async Usage
===========

Asynchronous Single Server
~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use the ``async_status()`` method for asynchronous requests:

.. code-block:: python

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


Bedrock Server (Async)
~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: python

    import asyncio
    from mcstatusio import BedrockServer

    async def check_bedrock_server():
        server = BedrockServer("play.cubecraft.net")
        status = await server.async_status()

        if status.online:
            print(f"Players: {status.players.online}/{status.players.max}")
            print(f"Edition: {status.edition}")
        
        return status

    asyncio.run(check_bedrock_server())


Query Multiple Servers
~~~~~~~~~~~~~~~~~~~~~~~

Efficiently query multiple servers concurrently:

.. code-block:: python

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
                print(f"{server.hostname}: {status.players.online} players")
            else:
                print(f"{server.hostname}: Offline")

    asyncio.run(check_multiple_servers())


Response Objects
================

JavaServerStatusResponse
~~~~~~~~~~~~~~~~~~~~~~~~

When a Java Edition server is online, the response contains:

- ``online`` (bool): Whether the server is online
- ``players.online`` (int): Number of players currently online
- ``players.max`` (int): Maximum number of players
- ``players.sample`` (list | None): Sample of online players
- ``version.name.clean`` (str): Clean version string
- ``version.name.raw`` (str): Raw version string with formatting
- ``version.name.html`` (str): HTML-formatted version string
- ``version.protocol`` (int): Protocol version number
- ``motd.clean`` (str): Message of the Day (plain text)
- ``motd.raw`` (str): MOTD with formatting codes
- ``motd.html`` (str): MOTD in HTML format
- ``hostname`` (str): Server hostname
- ``ip_address`` (str): Server IP address
- ``port`` (int): Server port
- ``icon`` (str | None): Base64-encoded server icon
- ``software`` (str | None): Server software (e.g., "Paper")
- ``plugins`` (list | None): List of plugins
- ``mods`` (list | None): List of mods
- ``srv`` (dict | None): SRV record information


BedrockServerStatusResponse
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When a Bedrock Edition server is online, the response contains:

- ``online`` (bool): Whether the server is online
- ``players.online`` (int): Number of players currently online
- ``players.max`` (int): Maximum number of players
- ``version.name`` (str): Version string
- ``version.protocol`` (int): Protocol version number
- ``motd.clean`` (str): Message of the Day (plain text)
- ``motd.raw`` (str): MOTD with formatting codes
- ``motd.html`` (str): MOTD in HTML format
- ``gamemode`` (str | None): Server gamemode
- ``server_id`` (str | None): Unique server identifier
- ``edition`` (str | None): Server edition ("MCPE" or "MCEE")
- ``ip_address`` (str): Server IP address
- ``port`` (int): Server port
