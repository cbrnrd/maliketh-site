---
title: Malleable Profiles
layout: default
---

# Malleable Profiles
{: .fw-700}

In order to make the implant more suitable to different situations, we have implemented a maleable profile system. This allows the implant to be configured to behave differently from build to build. These options can also be changed on the fly, allowing the implant to be reconfigured without having to be rebuilt.

The default profile is located in [`server/config/profiles/default.yaml`](https://github.com/cbrnrd/maliketh/tree/master/server/config/profiles/default.yaml). This is the profile that is used if no profile is specified. The default profile is also used if the specified profile is not found.

## Format

Profiles are YAML files with three main top level directives: `client`, `server`, and `routes`. The `client` directive contains the configuration options that are sent to the implant. The `server` directive contains the configuration options that are used by the server. The `routes` directive contains all HTTP endpoints to be used by both the client and server. Any options outside of these directoves are considered "global" and are used by both the client and server.

| Section | Purpose |
|:--------|:--------|
| `client` | Configures the implant |
| `server` | Configures the server |
| `routes` | Configures the implant routes |

The following top-level keys are **required**: `name`, `implant_id_cookie`, `client`, `server`, `routes`.


### Global options

| Option | Description | Required | Type |
|--------|-------------|----------|------|
| `name` | The name of this profile | Yes | String |
| `description` | A description of this profile | No | String |
| `implant_id_cookie` | The name of the cookie implants should use to identify themselves | Yes | String |

### Client options

| Option | Description | Required | Type |
|--------|-------------|----------|------|
| `user_agent` | The user agent to use when making HTTP requests | Yes | String |
| `sleep_time` | The number of seconds to sleep between each HTTP request | Yes | Integer |
| `jitter` | % jitter. The implant will sleep for a random amount of time between `sleep` and `sleep * (1 + jitter)` | Yes | Float, `[0, 0.99]` |
| `max_retries` | The maximum number of times to retry a request before giving up | Yes | Integer |
| `auto_self_destruct` | Whether or not to self destruct on failed checkins. If set to true, the implant will delete itself after `max_retries` failed checkins. | Yes | Boolean |
| `retry_wait` | The number of seconds to wait before retrying a request. | Yes | Integer |
| `retry_jitter` | % jitter. Between checkins, the implant will wait for a random amount of time between `retry_wait` and `retry_wait * (1 + retry_jitter)` | Yes | Float, `[0, 0.99]` |
| `tailoring_hashes` | (Unused) A list of hashes to use for payload tailoring. | Yes | List of strings |
| `tailoring_hash_function` | (Unused) The hash function to use for payload tailoring. | Yes | One of: `sha256`, `md5` |
| `tailoring_hash_rounds` | (Unused) The number of hash rounds to use for payload tailoring. | Yes | Integer |

#### Example JSON config

```json
{
    "cookie": "SESSID",
    "kill_date": "",
    "user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:68.0) Gecko/20100101 Firefox/68.0",
    "auto_self_destruct": true,
    "sleep_time": 60,
    "jitter": 0.1,
    "max_retries": 3,
    "retry_wait": 5,
    "retry_jitter": 0.1,
    "enc_key": "kX7tvu+8/ChkNuP2ScZRfz26OHde9DSfshaqSyIoEXY=",
    "tailoring_hash_function": "sha256",
    "tailoring_hash_rounds": 1,
    "tailoring_hashes": []
  }
```

### Server options

| Option | Description | Required | Type |
|--------|-------------|----------|------|
| `headers` | A String to String map of headers to send with each HTTP request | No | Map of strings |
| `redirect_url` | The URL to redirect to on the root path | Yes | String |

### Routes

See [here](../api-specifications/implant-c2.md) for a list of routes and their purposes.

## Default Profile

```yaml
######################################
# This is the default maleable profile for implants.
# This profile will be used if no profile is specified in the implant config
# The values in `routes` will be used to generate the routes for the C2 listener
# These will all be passed directly to @app.route() in Flask, so any valid Flask
# route arguments can be used here. 
######################################

# The name of the profile
name: default

######################################
# REQUIRED
# The name of the cookie that will be used by implants to identify themselves
# This cookie will be used to identify implants in the database
######################################
implant_id_cookie: SESSID

######################################
# REQUIRED
# The base64 encoded password to encrypt the initial registration request
# (SecretBox, 32 bytes)
######################################
registration_password: SWh5bHhGOENYQWF1TW9KR3VTb0YwVkVWbDRud1RFaHc=

client:

  ######################################
  # The user agent string that will be used by
  # implants when making HTTP requests to the C2.
  # This can be any string, but it is recommended
  # to use a common user agent string to avoid
  # detection by network monitoring tools.
  # Requests without this user agent will be ignored.
  ######################################
  user_agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:68.0) Gecko/20100101 Firefox/68.0

  ######################################
  # The type of encoding that will be used
  # by implants when sending encrypted data 
  # to the C2.
  ######################################
  encoding: base64

  ######################################
  # The amount of time to sleep between
  # checkins with the C2. (in seconds)
  ######################################
  sleep: 60

  ######################################
  # % jitter. The implant will sleep for
  # a random amount of time between `sleep`
  # and `sleep * (1 + jitter)`. From  0-0.99
  ######################################
  jitter: 0.1

  ######################################
  # The maximum number of times to retry
  # a request to the C2 before giving up.
  ######################################
  max_retries: 3

  ######################################
  # Whether or not to self destruct on
  # failed checkins. If set to true, the
  # implant will delete itself after
  # `max_retries` failed checkins.
  ######################################
  auto_self_destruct: true

  ######################################
  # The amount of time to wait before
  # retrying a failed request to the C2.
  # (in seconds)
  ######################################
  retry_wait: 5

  ######################################
  # % jitter. The implant will wait for 
  # a random amount of time between 
  # `retry_wait` and `retry_wait * (1 + retry_jitter)`
  # From  0-0.99
  ######################################
  retry_jitter: 0.1

  ######################################
  # The list of hashes to use for payload
  # tailoring.
  ######################################
  tailoring_hashes: []

  ######################################
  # The hash function to use for payload
  # tailoring.
  ######################################
  tailoring_hash_function: sha256

  ######################################
  # The number of rounds to use for payload
  # tailoring.
  ######################################
  tailoring_rounds: 1
    

server:
  
  ######################################
  # OPTIONAL
  # The `Server` header that will be returned by the C2 listener
  # This can be any string, but it is recommended
  # to use a common server header to avoid
  # detection by network monitoring tools.
  ######################################
  headers:
    Server: nginx/1.14.2


  ######################################
  # REQUIRED
  # The URL to redirect to for the base path (/)
  # This can be any valid URL
  ######################################
  redirect_url: https://www.google.com

######################################
# REQUIRED
# Routes for the C2 listener. All routes will be prepended with the base_path
# The routes will be passed directly to @app.route()
# Any valid Flask route arguments can be used here.
######################################
routes:
    
  ######################################
  # The base path for the C2 listener routes
  # This will be prepended to all paths
  ######################################
  base_path: /c2

  # The implant will use this route to register itself with the C2
  register:
    path: /register
    methods:
      - POST

  # The implant will use this route to check in with the C2
  checkin:
    path: /checkin
    methods:
      - GET

  # The implant will use this route to send results from a task
  task_results:
    path: /task
    methods:
      - POST
```