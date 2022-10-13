# V2Ray Docker Compose

This repository contains sample Docker Compose files to run V2Ray upstream and bridge servers.

## Documentation

### Terminology

* Upstream Server: A server that has free access to the Internet.
* Bridge Server: A server that is available to clients and has access to the upstream server.
* Client: A user-side application with access to the bridge server.

```
(Client) <-> [ Bridge Server ] <-> [ Upstream Server ] <-> (Internet)
```

### Setup

#### UUIDs

V2Ray uses the VMESS protocol as the primary protocol.
The VMESS protocol requires UUIDs for security reasons (instead of passwords).
We need two UUIDs for the two V2Ray servers (upstream and bridge servers).

You can generate UUIDs using:

* This Linux command:

```bash
cat /proc/sys/kernel/random/uuid
```

* This online tool:

[https://www.uuidgenerator.net](https://www.uuidgenerator.net)

Sample generated UUIDs:
* `cfc3ac34-a70d-424e-b43c-33049cf4bf31`
* `143d98d8-ac89-465a-acb5-d8d51e1f851f`

#### Upstream Server

To setup the upstream server:
1. Copy the "v2ray-upstream-server" directory into the upstream server.
2. Replace `<UPSTREAM-UUID>` in the `config.json` file with one of the generated UUIDs.
3. Run `docker-compose up -d`.

#### Bridge Server

To setup the bridge server:
1. Copy the "v2ray-bridge-server" directory into the bridge server.
2. Replace the following variables in the `config.json` file with appropriate values.
    * `<SHADOWSOCKS-PASSWORD>`: A password for ShadowSocks users like `!FR33DoM!`.
    * `<BRIDGE-UUID>`: The generated UUID for the bridge server.
    * `<UPSTREAM-IP>`: The upstream server IP address like `13.13.13.13`.
    * `<UPSTREAM-UUID>`: The used UUID for the upstream server.
3. Run `docker-compose up -d`. 

#### Clients

The bridge server exposes these proxy protocols:
* HTTP
* ShadowSocks
* VMESS

##### ShadowSocks Protocol

ShadowSocks is a popular proxy protocol.
You can find many client apps to use the ShadowSocks proxy on your devices.
These are recommended client apps:
* [ShadowSocks for macOS](https://github.com/shadowsocks/ShadowsocksX-NG/releases)
* [ShadowSocks for Linux](https://github.com/shadowsocks/shadowsocks-libev)
* [ShadowSocks for Windows](https://github.com/shadowsocks/shadowsocks-windows/releases)
* [ShadowSocks for Android](https://github.com/shadowsocks/shadowsocks-android/releases)
* [ShadowLink for iOS](https://apps.apple.com/us/app/shadowlink-shadowsocks-vpn/id1439686518)
* [Potatso Lite for iOS](https://apps.apple.com/us/app/potatso-lite/id1239860606)

Client configuration:
```
IP Address: <BRIDGE-IP>
Port: 1210
Encryption/Method/Algorithm: aes-128-gcm
Password: <SHADOWSOCKS-PASSWORD>
```

##### VMESS Protocol

The VMESS proxy protocol is the recommended one.
It's the primary protocol that V2Ray provides.
These are recommended client apps:
* [V2RayX for macOS](https://github.com/Cenmrev/V2RayX/releases)
* [v2ray-core for Linux](https://github.com/v2ray/v2ray-core)
* [v2rayN for Windows](https://github.com/2dust/v2rayN/releases)
* [ShadowLink for iOS](https://apps.apple.com/us/app/shadowlink-shadowsocks-vpn/id1439686518)
* [v2rayNG for Android](https://github.com/2dust/v2rayNG)

Client configuration:
```
IP Address: <BRIDGE-IP>
Port: 1310
ID/UUID/UserID: <BRIDGE-UUID>
Alter ID: 10
Level: 0
Security/Method/Encryption: aes-128-gcm
Network: TCP
```

##### HTTP Protocol

The HTTP proxy is appropriate for internal usage on the bridge server and would be exposed only to the 127.0.0.1 IP address without a password.
For example, the command below shows how to use it on the bridge server terminal.

```shell
export {http,https}_proxy="http://127.0.0.1:1110"
export {HTTP,HTTPS}_PROXY="http://127.0.0.1:1110"

# This "curl" should return the upstream server IP address
curl ifconfig.io

# The "sudo" command needs the -E parameter to use HTTP proxy and other envs
sudo -E apt install docker

unset {http,https}_proxy
unset {HTTP,HTTPS}_PROXY
```

You can use the HTTP proxy on your local devices using port forwarding, as well.
The following SSH command makes the HTTP proxy available to the local device and the private network it uses.

```shell
ssh -vNL 1110:0.0.0.0:1110 root@<BRIDGE-IP>

export {http,https}_proxy="http://127.0.0.1:1110"
export {HTTP,HTTPS}_PROXY="http://127.0.0.1:1110"

# ...
```



##### To use ssh over http proxy you can add this line at $HOME/.ssh/ssh_config
```
ProxyCommand /usr/bin/nc -X connect -x 127.0.0.1:YOUR_HTTP_CLIENT_PROXY_PORT %h %p

Then you can use ssh:

ssh root@1.2.3.4 -p 22
```

##### To use ssh over socks5 proxy you can add this line at $HOME/.ssh/ssh_config
```
ProxyCommand /usr/bin/nc -X 5 -x 127.0.0.1:YOUR_SOCKS_CLIENT_PROXY_PORT %h %p

Then you can use ssh:

ssh root@1.2.3.4 -p 22
```

## P.S.

This repository is kind of forked from [v2ray-config-examples](https://github.com/xesina/v2ray-config-examples).
Thanks to [@xesina](https://github.com/xesina) and other contributors.
