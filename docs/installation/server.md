---
title: Server
layout: default
parent: Installation
---

# Installing the C2 Server
{: .fw-700}

## tl;dr
You should RTFM, but if you're lazy like me, here's the quickstart:
```bash
$ git clone https://github.com/cbrnrd/maliketh
$ cd maliketh/server
$ cp .env.example .env
$ vim .env
$ docker compose --env-file .env up -d --build
$ ./bootstrap_db.sh
```
{: .fs-5}

The C2 server is the backbone of the entire operation. It is responsible for:

* Receiving and storing data from the implants
* Sending commands to the implants
* Receiving commands from operators

## Configuring the server

Aside from a [profile](../profile/), the server needs some environment variables configured to run properly.

Make a copy of the example environment file:

```bash
$ cp .env.example .env
```
{: .fs-5}

Then edit the `.env` file to your liking. The following environment variables are required:

| Variable | Description |
| --- | --- |
| `POSTGRES_USER` | The username for the Postgres database |
| `POSTGRES_PASSWORD` | The password for the Postgres database |
| `PGDATA` | The path to the Postgres data directory. You can ignore this. |
| `WIREGUARD_IP` | The IP address for the Wireguard interface. This should be in the same subnet as the Wireguard server. If you choose to not use wireguard, then set this to the interface you want to bind on. |

## Starting the server

**Prerequistes:**

The following are required to run the server:
* Docker
* Docker Compose
* Wireguard (optional but ideal)

To start the server, run the following command:

```bash
$ docker compose --env-file .env up -d --build
```
{: .fs-5}

**That's it!** The server should now be running.

Ok I lied, there's one more quick step. You need to create the admin operator. To do this, run the following command:

```bash
$ ./bootstrap_db.sh
```
{: .fs-5}

The output will look like this:

```
This will delete the database and create a new one. Are you sure you want to do this? (y/n) y
[*] Initializing database
[*] Generating server keypair
[*] Done, server keypair written to config/admin/certs/
[*] Generating admin credentials:
{
    "name": "admin",
    "c2": "localhost",
    "c2_port": 5000,
    "login_secret": "cFC9wjVnh!p-Fc&AC$Mkd-9n}5glAYAa",
    "secret": "x/MiOc0OZF2AIottypmrPT36K461LnjHCc91jvVbe/E=",
    "public": "3yCjchWJrMX64i9x+1bZ2+/U/H6/yIM1yGKknL1OxDs=",
    "signing_key": "43x9asm7LRSxEMfnU8qgK584UjB27m0av72kfllIDEU=",
    "verify_key": "EcSQOE4g3cf8czIuP+AckWrOkWieWCXL2ZbIvZJrl2E=",
    "server_pub": "5NQJDH80V1BEKoAoPghV9rjS6hpx/Krv9FEByIxztxs=",
    "rmq_queue": "eW3zVMG]I$bXiFHz(~Q9)b>/X_3mSjaK"
}
```
{: .fs-5}

{: .note}
Change `c2` and `c2_port` to the IP address (or domain) and port of your C2 server.

The output should be a JSON object representing an operator config to be used with the client.

## Adding operators

To add an operator, run the following command:

{: .note}
Replace `<operator name>` with the name of the operator you want to create.

```bash
$ docker-compose --env-file .env run --rm operator python3 create_operator.py -n <operator name>
```
{: .fs-5}

### (Optional) Using Wireguard

Wireguard is a VPN protocol that is used to securely connect implants to the C2 server. It is optional, but highly recommended.

The nice thing about using wireguard is it locks down network connections to the server to only those that are connected to the VPN.

I like to use [`wireguard-install`](https://github.com/hwdsl2/wireguard-install) for easy setup.
To install it, run the following command:

```bash
$ wget -O wireguard.sh https://get.vpnsetup.net/wg
$ sudo bash wireguard.sh --auto
```
{: .fs-5}

Then follow the prompts to configure the server.

To create a new operator config, run the following command:

```bash
$ ./wireguard.sh
```
{: .fs-5}

Then follow the prompts to "Add a new client". This will generate a `.conf` file that you can send to the operator.

{: .warning}
It's best practice to have one Wireguard config per operator. This way, if an operator's config is compromised, you can easily revoke their access without affecting other operators.