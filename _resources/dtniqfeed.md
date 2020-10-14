---
layout: collection
title:  "Setting Up DTN and IQFeed"
---



## Requesting Historical Data via TCP/IP Connection

### Steps to Initiate the Feed
1. Log into IQFeed by following the instructions for Initializing the Feed.
2. Connect to IQConnect by following the instructions for Level 1 via TCP/IP.
3. Once a connection has been established, connect to the Lookup port for IQFeed's IQConnect. It's TCP/IP address is:
```
Address: 127.0.0.1 Port: [configurable via registry setting]
```
NOTE: The port is a configurable registry setting that can be found in the registry at
[HKEY_CURRENT_USER\SOFTWARE\DTN\IQFEED\Startup\LookupPort]. It is a string value that defaults to 9100.
Once connected, send one of the request commands.

### Request commands

#### Data Returned
- It's important to note that data is returned as *High, Low, Open, Close* format, rather than the typical *Open, High, Low, Close* format.
