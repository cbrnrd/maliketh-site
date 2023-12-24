---
title: Operator Client
layout: default
nav_order: 4
---

# The Maliketh Client
{: .fw-700}

The Maliketh Operator Client is a console-based application that enables you to easily interact with implants. It is written in Python and is cross-platform.

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>


## Starting the client

After installing the client, starting the client is simple:

```bash
$ poetry run maliketh_client -c your_config.json
```
{: .fs-5}


There are a few other optional command line arguments that you can use:

```bash
$ poetry run maliketh_client -h
Usage: usage: maliketh_client --config OPERATOR_CONFIG_FILE [options]

Maliketh Operator Client

Options:
  -h, --help            show this help message and exit
  -c CONFIG, --config=CONFIG
                        The operator configuration file to use
  -d, --debug           Enable debug mode
  -t, --with-timestamps
                        Enable timestamps in log messages
```
{: .fs-5}

## Interacting with implants

When an implant connects to the server, it will be displayed in the client. You can interact with the implant with the `interact` command. This will open a new shell in "interact" mode that will be used to interact with the implant. In this case, our implant's ID is `dd35c485`.

![Interacting with an implant](/data/client/interact.png)


## Executing tasks

Once you are interacting with an implant, can execute tasks (or commands) on the implant. For example, you can execute `whoami`:

![Executing commands on an implant](/data/client/whoami.png)


## Retrieving output
Notice above how the output of the command is not displayed immediately. This is because the output (could) be a large amount of data. The output is stored in the database and can be retrieved with the `results` command, followed by the ID of the task:

![Retrieving output](/data/client/results.png)

### More complex output

Sometimes the output of a task is more complex than just a string. For example, the output of the `ps` command is a map of process IDs to process names. The client will automatically detect this and display the output in a more readable tabular format:

![Retrieving complex output](/data/client/ps.png)

## Broadcasting commands

You can broadcast commands to all connected implants with the `broadcast` command. This can be useful for tasks like downloading a file from all connected implants:

![Broadcasting commands](/data/client/broadcast.png)

## Building implants

Each operator has the ability to compile new implants with some custom options. This is done with the `build` command. The `build` command will create a new implant with the given options and write it to the given output file. The output file can then be distributed to targets.

### Setting build options

The following options can be set when building an implant (using the `builder set` command):

| Option | Arg(s) | Description |
| --- | --- | --- |
| `initial_sleep_seconds` | `<seconds>` | Set the initial sleep time |
| `kill_parent` | `<true|false>` | Set the kill_parent option |
| `register_max_retries` | `<number>` | Set the max number of retries for registering |
| `scheduled_task_name` | `<name>` | Set the scheduled task name |
| `schtask_persist` | `<true|false>` | Set the schtask_persist option |
| `use_antidebug` | `<true|false>` | Set the use_antidebug option |
| `use_antivm` | `<true|false>` | Set the use_antivm option |

To show the current value of a given option, use the `builder show` command:

![Showing build options](/data/client/builder_show.png)

### Actually building the thing

Once you have set the options you want, you can build the implant with the `build` command, along with a path to write the file to:

![Building an implant](/data/client/build.png)

## Command reference

### `help`

| Command | Subcommands | Args | Description |
| --- | --- | --- | --- |
| alias | delete, list, set |`<implant_id> [alias]` | Set an alias for a given implant |
| broadcast | | `<command>` | Send an interact command to every connected implant (this can get very noisy, USE WITH CAUTION!) |
| build | | `<output_file>` | Build an implant with the given options and write it to <output_file> |
| builder | set, show | `<option> [value]` | Set or show builder options |
| exit | | | Exit the client |
| help | | | Show this help message and exit |
| interact | | `<implant_id>` | Interact with a given implant id |
| results | | `<task_id> [local_path]` | Show the results of a given task id. Optionally write the results to a file |
| show | implants, stats, tasks | | Show implants, stats, or tasks |


### `interact`

| Command | Subcommands | Args | Description |
| --- | --- | --- | --- |
| back | | | Exit the interact menu |
| chdir | | `<path>` | Change the implant's working directory |
| cmd | | `<args>` | Execute a shell command on the implant |
| config | set, show | `<option> [value]` | Set or show implant configuration options |
| disable_defender | | | Try to disable Windows Defender (will not work if user is not admin) |
| download | | `<remote_path>` | Download a file from the implant |
| exit | | | Exit the interact menu |
| getenv | | | Get all environment variables |
| inject | | `<shellcode_path> <process_name>` | Spawn a process then inject shellcode into it |
| ls | | | List files in the implant's working directory |
| ps | | | List running processes |
| pwd | | | Get the implant's working directory |
| results | | `<task_id> [local_path]` | Show the results of a given task id. Optionally write the results to a file |
| selfdestruct | | | Remove and kill the implant |
| sleep | | `<seconds>` | Sleep for a given number of seconds |
| sysinfo | | | Get system information from the implant |
| upload | | `<local_path> <remote_path>` | Upload a file to the implant and store it in <remote_path> |
| whoami | | | Get the current user |
