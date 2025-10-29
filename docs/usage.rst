Usage
=====

Bedrock Server Status
--------------------

.. code-block:: python

    from mcstatusio import BedrockServer

    server = BedrockServer("play.example.com", 19132)
    status = server.status()

    if not status.online:
        print("The server is offline.")
    else:
        print(f"The server has {status.players.online} players online.")


Java Server Status
-----------------

.. code-block:: python

    from mcstatusio import JavaServer

    server = JavaServer("play.example.com", 25565)
    status = server.status()

    if not status.online:
        print("The server is offline.")
    else:
        print(f"The server has {status.players.online} players online.")


Async Usage
===========

Bedrock Server Status (Async)
-----------------------------

.. code-block:: python

    import asyncio
    from mcstatusio import BedrockServer

    server = BedrockServer("play.example.com", 19132)
    status = asyncio.run(server.async_status())

    if not status.online:
        print("The server is offline.")
    else:
        print(f"The server has {status.players.online} players online.")


Java Server Status (Async)
-------------------------

.. code-block:: python

    import asyncio
    from mcstatusio import JavaServer

    server = JavaServer("play.example.com", 25565)
    status = asyncio.run(server.async_status())

    if not status.online:
        print("The server is offline.")
    else:
        print(f"The server has {status.players.online} players online.")
