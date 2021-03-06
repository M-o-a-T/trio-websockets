.. image:: https://img.shields.io/badge/docs-read%20now-blue.svg
   :target: https://trio-websockets.readthedocs.io/en/latest/?badge=latest
   :alt: Documentation Status

.. image:: https://travis-ci.org/M-o-a-T/trio-websockets.svg?branch=master
   :target: https://travis-ci.org/M-o-a-T/trio-websockets
   :alt: Automated test status

.. image:: https://codecov.io/gh/M-o-a-T/trio-websockets/branch/master/graph/badge.svg
   :target: https://codecov.io/gh/M-o-a-T/trio-websockets
   :alt: Test coverage

trio-websockets
===============

Trio-websockets is a `trio`_ compatible library for connecting and serving websockets.

It was originally written by Laura Dickinson <l@veriny.tf> as `asyncwebsockets`_
This fork dispenses with curio / multio compatibility and renames it to be
compatible with the rest of the Trio landscape.


Installation
------------

To install the latest stable version::

    $ pip install trio-websockets

To install the latest development version::

    $ pip install git+https://github.com/M-o-a-T/trio-websockets.git


Basic Usage
-----------

.. code-block:: python3

    import trio

    from trio_websockets.client import connect_websocket
    from trio_websockets.ws import WebsocketConnectionEstablished, WebsocketBytesMessage

    async def test():
        sock = await connect_websocket("wss://echo.websocket.org", reconnecting=False)
        async for message in sock:
            print("Event received", message)
            if isinstance(message, WebsocketConnectionEstablished):
                await sock.send_message(b"test")

            elif isinstance(message, WebsocketBytesMessage):
                print("Got response:", message.data)
                await sock.close(code=1000, reason="Thank you!")


    trio.run(main)

.. _trio: https://trio.readthedocs.io/en/latest/

.. _asyncwebsockets: https://github.com/SunDwarf/asyncwebsockets

