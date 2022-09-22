# V2Ray Docker Compose

This repository contains sample Docker Compose files to run upstream and bridge servers using V2Ray.

## Documentation

### Terminology

* Upstream Server: A server that has free access to the Internet.
* Bridge Server: A server with access to an upstream server and is available to clients.
* Client: A user application that only has access to the bridge server.

```
[ Client ] <-> [ Bridge Server ] <-> [ Upstream Server ] <-> [ Internet ]
```

### Setup

#### Generate UUIDs

The VMESS protocol requires UUIDs.
We need two UUIDs (`<UPSTREAM-UUID>` and `<BRIDGE-UUID>`) for the two VMESS servers.
You can generate UUIDs here:

[https://www.uuidgenerator.net](https://www.uuidgenerator.net)

Sample UUIDs:

```
cfc3ac34-a70d-424e-b43c-33049cf4bf31
143d98d8-ac89-465a-acb5-d8d51e1f851f
```

#### Upstream Server

1. Copy the "upstream" directory into the server with free access to the Internet.
2. Replace `<UPSTREAM-UUID>` in the config.json file with the generated UUID for the upstream server.
3. Run `docker-compose up -d`. 

#### Bridge Server

1. Copy the "bridge" directory into the server with access to the upstream server.
2. Replace the following config variables:
    * `<HTTP-USERNAME>` with a desired username for http proxy.
    * `<HTTP-PASSWORD>` with a safe password for http proxy.
    * `<SHADOWSOCKS-PASSWORD>` with a safe password for ShadowSocks proxy.
    * `<BRIDGE-UUID>` with the generated UUID for the bridge server.
    * `<UPSTREAM-SERVER-IP>` with the upstream server IP address.
    * `<UPSTREAM-UUID>` with the generated UUID for the upstream server.
3. Run `docker-compose up -d`. 

#### Setup Client Apps

The bridge server exposes three proxy protocols:
* HTTP Proxy
* ShadowSocks
* VMESS

You can use any VPN client which supports one of the protocols above.

Here are some sample clients:
* [ShadowSocks for macOS](https://github.com/shadowsocks/ShadowsocksX-NG/releases)
* [ShadowSocks for Linux](https://github.com/shadowsocks/shadowsocks-libev)
* [ShadowSocks for Windows](https://github.com/shadowsocks/shadowsocks-windows/releases)
* [ShadowSocks for iOS](https://apps.apple.com/us/app/potatso-lite/id1239860606)
* [ShadowSocks for Android](https://github.com/shadowsocks/shadowsocks-android/releases)
* [ShadowSocks for Android](https://github.com/shadowsocks/shadowsocks-android/releases)
* [VMESS Clients](https://www.v2ray.com/en/awesome/tools.html)
