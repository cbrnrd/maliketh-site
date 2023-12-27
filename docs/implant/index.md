---
title: Implant
layout: default
nav_order: 5
has_children: true
---

# Implant
{: .fw-700}

The implant is the agent that is installed on the target machine. It is responsible for executing the commands sent by the server and sending the results back to the server.

## Building the implant

The primary implant is written in MSVC++ and is compiled to an EXE. It should be built using mingw-w64. The following command will build the implant:

Thankfully, you don't have to worry about configuring any compiler flags or anything like that. The `Makefile` will take care of everything for you.

To build the debug version of the implant, run the following command:

```bash
$ cd implant
$ make implant
```

To build a stripped version of the implant in release mode, run the following command:

```bash
$ cd implant
$ make release
```

### Building a full "release" implant

When you build with `make release`, the binary is stripped, but debug print statements are still included in the binary.

In order to remove these, run the following command:

Linux: 
```bash
$ cd implant
$ RELEASE_OUTFILE=release.exe ./build_release.sh  # implant saved in ./release.exe 
```

Windows (powershell):
```powershell
PS> cd implant
PS> $env:RELEASE_OUTFILE="release.exe"; .\build_release.ps1  # implant saved in ./release.exe 
```

## Evasion

The implant has some basic evasion techniques included, but it is NOT meant to be FUD by anti-virus as there are some clear signatures. For example, it uses implicit linking of all DLLs instead of being sneakier and usng explicit linking. This is because I don't want to spend too much time on evasion techniques. If you want to make it FUD, you can use a tool like [Veil](https://github.com/Veil-Framework/Veil) to generate a payload that is relatively FUD.

However, we'll go into the basic evasion techniques that are used in the implant.

### Compile-time string obfuscation

The implant uses a compile-time string obfuscation technique that I found [here](https://github.com/andrivet/ADVobfuscator). Full credit to andrivet for this technique. It's a very simple technique that is very effective at preventing static analysis of strings.

### Anti-Debugging

The implant sets the `ThreadHideFromDebugger` flag using `NtSetInformationThread` to hide from debuggers. The implant will then crash if it detects that it is being debugged.

### Sleep skip detection

The implant will check for sleep skipping detection by using `QueryPerformanceCounter` and `QueryPerformanceCounter` to check if the sleep time is actually the time that has passed. If the implant detects it is being sleep skipped, it will crash.

This topic interests me for some reason, so this is a possible area of improvement (maybe using NTP?).

### Anti-Sanbox

This is maybe the simplest of the evasion techniques. The implant will execute the `cpuid` instruction and check the value of `ebx` after executing it. The implant shows a pop up message box if this is detected. This is a very simple technique that is easily bypassed, but it's good enough for now.