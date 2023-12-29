---
title: The Go Implant
layout: default
parent: Implant
nav_order: 1
---

# Go implant
{: .fw-700}

We have a Go implant that is currently in development. It's not officially supported in the client/server yet, but you can easily build it and use it.
Many functions are from [Coldfire](https://github.com/redcode-labs/Coldfire), so full credit to them for that.

{: .warning}
The Go implant does not have as many features as the C++ implant. It is still in development and is very prone to bugs. Use at your own risk.

## Building the implant

Tools needed:
* Go (I like to use [`gimme`](https://github.com/travis-ci/gimme))
* Make

Then, change the following variables in `go_implant/pkg/config/config.go`:

```go
// !!!!!!!!!! CHANGE THESE !!!!!!!!! //
const C2_DOMAIN = "localhost"
const C2_PORT = 80
const C2_USE_TLS = false
const C2_REGISTER_PASSWORD = "SWh5bHhGOENYQWF1TW9KR3VTb0YwVkVWbDRud1RFaHc="
// !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! //
```

Then, go into the `go_implant` directory:

```bash
$ cd go_implant
```

To build the development implant, simply run

```bash
$ make
```

This builds the debug version of the implant for your native OS and architecture.

{: .warning}
Any other `make` command will respect the `DEBUG` environment variable when deciding to make a release or debug build.

To build for Mac, Linux, and Windows, run:

```bash
$ make most
```

This will cover most of the use cases.

To build for *all* supported OSes and architectures, run

```bash
$ make all
```

To build a stripped version of the implant, simply set `DEBUG=0` when running make:

```bash
$ DEBUG=0 make most
```

To remove all print statements and debug strings, and strip the binary run:

```bash
$ DEBUG=0 make release

# Compile all supported architectures
$ DEBUG=0 make release-all
```

You can also set the usual `GOOS` and `GOARCH` environment variables to build for a specific OS and architecture using

```bash
$ GOOS=linux GOARCH=arm make build
```

## What's supported?

We've implemented all _basic_ functionality, including:
* All commands
* Anti-debugging
* Anti-VM (this is actually better than the C++ implant thanks to Coldfire)
* Better AV killing on Windows
* Multiple operating systems (Windows, MacOS, Linux)!!

## What's NOT supported?

We've implemented all _basic_ functionality, but there are a few things that are not supported yet:
* Compiletime string obfuscation
* Persistence
* Server side building
* Sandbox detection behavior
* (Mac) AV Kill
* (Mac) Shellcode execution

Also, since it's Golang, there are some downsides:
* Memory footprint is larger
* Binary size is larger