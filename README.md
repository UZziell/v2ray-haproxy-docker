# V2Ray Docker Compose

This repository contains sample Docker Compose files to run upstream and bridge V2Ray servers.

## Documentation

### Terminology

* Upstream Server: A server that has free access to the Internet.
* Bridge Server: A server that is available to clients and has access to an upstream server.
* Client: A user-side application with access to the bridge server.

```
[ Client ] <-> [ Bridge Server ] <-> [ Upstream Server ] <-> [ Internet ]
```

### Setup

#### UUIDs

V2Ray uses the VMESS protocol as the primary protocol.
The VMESS protocol requires UUIDs for security reasons (instead of passwords).
We need two UUIDs for the two V2Ray servers (upstream and bridge servers).

You can generate UUIDs:

1- Online:

[https://www.uuidgenerator.net](https://www.uuidgenerator.net)

2- The Linux command:

```bash
cat /proc/sys/kernel/random/uuid
```

Sample UUIDs:
* `cfc3ac34-a70d-424e-b43c-33049cf4bf31`
* `143d98d8-ac89-465a-acb5-d8d51e1f851f`


#### Upstream Server

To setup the upstream server:
1. Copy the "v2ray_upstream_server" directory into the upstream server.
2. Replace `<UPSTREAM-UUID>` in the `config.json` file with the generated UUID for the upstream server.
3. Run `docker-compose up -d`.

#### Bridge Server

To setup the bridge server:
1. Copy the "v2ray_bridge_server" directory into bridge server.
2. Replace the following variables in the `config.json` file with appropriate values.
    * `<SHADOWSOCKS-PASSWORD>`: A password for ShadowSocks like `!FR33DoM!`.
    * `<BRIDGE-UUID>`: The generated UUID for the bridge server.
    * `<UPSTREAM-SERVER-IP>`: The upstream server IP address like `13.13.13.13`.
    * `<UPSTREAM-UUID>`: The generated UUID for the upstream server.
3. Run `docker-compose up -d`. 

#### Setup Client Apps

The bridge server exposes these proxy protocols:
* ShadowSocks
* VMESS

You can use any VPN client which supports one of the protocols above.
The list below includes some of the recommended client applications.
* [ShadowSocks for macOS](https://github.com/shadowsocks/ShadowsocksX-NG/releases)
* [ShadowSocks for Linux](https://github.com/shadowsocks/shadowsocks-libev)
* [ShadowSocks for Windows](https://github.com/shadowsocks/shadowsocks-windows/releases)
* [ShadowSocks for Android](https://github.com/shadowsocks/shadowsocks-android/releases)
* [v2rayNG](https://github.com/2dust/v2rayNG) (Android) [VMESS]
* [ShadowLink](https://apps.apple.com/us/app/shadowlink-shadowsocks-vpn/id1439686518) (iOS) [VMESS & ShadowSocks]
* [Potatso Lite](https://apps.apple.com/us/app/potatso-lite/id1239860606) (iOS) [ShadowSocks]

ShadowSocks client configuration:
```
IP: <BRIDGE-SERVER-IP>
Port: 1012
Encryption (Algorithm): aes-128-gcm
Password: <SHADOWSOCKS-PASSWORD>
```

VMESS client configuration:
```
IP: <BRIDGE-SERVER-IP>
Port: 1013
User ID: <BRIDGE-UUID>
AlterID: 10
Level: 0
Security (Encryption): aes-128-gcm
Network: TCP
```

## P.S.

This repository is kind of forked from [v2ray-config-examples](https://github.com/xesina/v2ray-config-examples).
Thanks to [@xesina](https://github.com/xesina) and other contributors.
