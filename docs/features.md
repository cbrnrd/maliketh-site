---
title: Features
layout: default
nav_order: 2
---

# Features
{: .fw-700}

Maliketh comes with a variety of features in all components of the framework.

## Client
* Easy to use, console-based interface (similar to Metasploit, Empire, etc.)
* Implant aliasing
* Live updates/server announcements
* Public-key backed authentication. No passwords!

## Server
* Easily deployable via Docker Compose
* (Ideally) Clients connect through WireGuard, providing a secure, authenticated, and encrypted tunnel
* Supports multiple operators
* Persistent storage of implants and operator data
* Per-operator implant builder

## Implant
The primary C++ implant supports all of the following features.

{: .note }
The Go implant is still in development and does not support all of the features listed below, but it has most of them.

* File upload and download
* Command execution
* Shellcode injection
* Live configuration updates
* Sysinfo
* Self-destruct (and auto self-destruct)
* `cmd`less basic operating system commands (ls, cd, pwd, etc.)
* Sleep Skip detection
* VM detection
* Sandbox detection
* (Golang only) Anti-AV


## TODO
* Operator roles (admin, operator, etc.)
* Operator chat
* Keylogger
* Clipjacker/clipboard monitor
* Looter
* More transport protocols
* New Anti-VM, Anti-Sandbox, Anti-AV, sleep skip detection improvements and methods