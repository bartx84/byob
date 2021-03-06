# BYOB (Build Your Own Botnet)
[![license](https://img.shields.io/badge/license-GPL-brightgreen.svg)](https://github.com/colental/byob/blob/master/LICENSE)

BYOB is an open-source project that provides a framework for security researchers 
and developers to build and operate a basic botnet to deepen their understanding
of the sophisticated malware that infects millions of devices every year and spawns
modern botnets, in order to improve their ability to develop counter-measures against 
these threats

The library contains 4 main parts:

## Server
[![server](https://img.shields.io/badge/byob-server-blue.svg)](https://github.com/colental/byob/blob/master/byob/server.py)

`usage: server.py [-h] [-v] [--host HOST] [--port PORT] [--database DATABASE]`

*Command & control server with persistent database and console*

- __Console-Based User-Interface__: streamlined console interface for controlling client host machines remotely via
reverse TCP shells which provide direct terminal access to the client host machines

- __Persistent SQLite Database__: lightweight database that stores identifying information about client host machines,
allowing reverse TCP shell sessions to persist through disconnections of arbitrary
duration and enabling long-term reconnaissance

- __Client-Server Architecture__: all python packages/modules installed locally are automatically made available for clients 
to remotely import without writing them to the disk of the target machines, allowing clients to use modules which require
packages not installed on the target machines

## Client
[![client](https://img.shields.io/badge/byob-client-blue.svg)](https://github.com/colental/byob/blob/master/byob/client.py)

`usage: client.py [-h] [-v] [--name NAME] [--icon ICON] [--pastebin API]
                     [--encrypt] [--obfuscate] [--compress] [--compile]
                     host port [module [module ...]]`

*Generate fully-undetectable clients with staged payloads, remote imports, and unlimited modules*

- __Remote Imports__: remotely import third-party packages from the server without writing them 
to the disk or downloading/installing them

- __Nothing Written To The Disk__: clients never write anything to the disk - not even temporary files (zero IO
system calls are made) because remote imports allow arbitrary code to be 
dynamically loaded into memory and directly imported into the currently running 
process

- __Zero Dependencies (Not Even Python Itself)__: client runs with just the python standard library, remotely imports any non-standard
packages/modules from the server, and can be compiled with a standalone python 
interpreter into a portable binary executable formatted for any platform/architecture,
allowing it to run on anything, even when Python itself is missing on the target host

- __Add New Features With Just 1 Click__: any python script, module, or package you to copy to the `./byob/modules/` directory
automatically becomes remotely importable & directly usable by every client while 
your command & control server is running

- __Write Your Own Modules__: a basic module template is provided in `./byob/modules/` directory to make writing
your own modules a straight-forward, hassle-free process

- __Run Unlimited Modules Without Bloating File Size__: use remote imports to add unlimited features without adding a single byte to the
client's file size 

- __Fully Updatable__: each client will periodically check the server for new content available for
remote import, and will dynamically update its in-memory resources
if anything has been added/removed

- __Platform Independent__: everything is written in Python (a platform-agnostic language) and the clients
generated can optionally be compiled into portable executable (*Windows*) or
bundled into an standalone application (*macOS*)

- __Bypass Firewalls__: clients connect to the command & control server via reverse TCP connections, which
will bypass most firewalls because the default filter configurations primarily
block incoming connections

- __Counter-Measure Against Antivirus__: avoids being analyzed by antivirus by blocking processes with names of known antivirus
products from spawning

- __Encrypt Payloads To Prevent Analysis__: the main client payload is encrypted with a random 256-bit key which exists solely
in the payload stager which is generated along with it

- __Prevent Reverse-Engineering__: by default, clients will abort execution if a virtual machine or sandbox is detected

## Modules
[![modules](https://img.shields.io/badge/byob-modules-blue.svg)](https://github.com/colental/byob/blob/master/byob/modules)

*11 post-exploitation modules that are remotely importable by clients*

1) `byob.modules.keylogger`: logs the user’s keystrokes & the window name entered
2) `byob.modules.screenshot`: take a screenshot of current user’s desktop
3) `byob.modules.webcam`: view a live stream or capture image/video from the webcam
4) `byob.modules.ransom`: encrypt files & generate random BTC wallet for ransom payment
5) `byob.modules.outlook`: read/search/upload emails from the local Outlook client
6) `byob.modules.packetsniffer`: run a packet sniffer on the host network & upload .pcap file
7) `byob.modules.persistence`: establish persistence on the host machine using 5 different methods
8) `byob.modules.phone`: read/search/upload text messages from the client smartphone
9) `byob.modules.escalate`: attempt UAC bypass to gain unauthorized administrator privileges
10) `byob.modules.portscanner`: scan the local network for other online devices & open ports
11) `byob.modules.process`: list/search/kill/monitor currently running processes on the host

## Core
[![core](https://img.shields.io/badge/byob-core-blue.svg)](https://github.com/colental/byob/blob/master/byob/core)

*6 core framework modules used by the generator and the server*

1) `byob.core.util`: miscellaneous utility functions that are used by many modules
2) `byob.core.handlers`: base server class and various request handler classes
3) `byob.core.security`: Diffie-Hellman Internet Key Exchange (RFC 2741) and 3 different types of encryption
4) `byob.core.loader`: enables clients to remotely import any package/module/script from the server
5) `byob.core.payload`: reverse TCP shell designed to remotely import dependencies, packages & modules
6) `byob.core.generators`: functions which all dynamically generate code for the client generator

