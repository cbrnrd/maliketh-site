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

Then, build the debug implant using the following command:

```bash
$ cd go_implant
$ make
```

If you'd like to build an implant for all supported operating systems, run the following command:

```bash
$ cd go_implant
$ make all
```

If you'd like to build a stripped version of the implant in release mode, run the following command:

```bash
$ cd go_implant
$ DEBUG=0 make [all]
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
* 

Also, since it's Golang, there are some downsides:
* Memory footprint is larger
* Binary size is larger