# webtunnel-server

webtunnel exposes your localhost to the world for easy testing and sharing! No need to mess with DNS or deploy just to have others test out your changes.

This repo is the server component. If you are just looking for the CLI webtunnel app, see (https://github.com/yousufkalim/webtunnel).

## overview ##

The default webtunnel client connects to the `webtunnel.me` server. You can, however, easily set up and run your own server. In order to run your own webtunnel server you must ensure that your server can meet the following requirements:

* You can set up DNS entries for your `domain.tld` and `*.domain.tld` (or `sub.domain.tld` and `*.sub.domain.tld`).
* The server can accept incoming TCP connections for any non-root TCP port (i.e. ports over 1000).

The above are important as the client will ask the server for a subdomain under a particular domain. The server will listen on any OS-assigned TCP port for client connections.

#### setup

```shell
# pick a place where the files will live
git clone git://github.com/yousufkalim/webtunnel-server.git
cd webtunnel-server
npm install

# server set to run on port 1234
bin/server --port 1234
```

The webtunnel server is now running and waiting for client requests on port 1234. You will most likely want to set up a reverse proxy to listen on port 80 (or start webtunnel on port 80 directly).

**NOTE** By default, webtunnel will use subdomains for clients, if you plan to host your webtunnel server itself on a subdomain you will need to use the _--domain_ option and specify the domain name behind which you are hosting webtunnel. (i.e. my-webtunnel-server.example.com)

#### use your server

You can now use your domain with the `--host` flag for the `wt` client.

```shell
wt --host http://sub.example.tld:1234 --port 9000
```

You will be assigned a URL similar to `heavy-puma-9.sub.example.com:1234`.

If your server is acting as a reverse proxy (i.e. nginx) and is able to listen on port 80, then you do not need the `:1234` part of the hostname for the `wt` client.

## REST API

### POST /api/tunnels

Create a new tunnel. A webtunnel client posts to this enpoint to request a new tunnel with a specific name or a randomly assigned name.

### GET /api/status

General server information.
