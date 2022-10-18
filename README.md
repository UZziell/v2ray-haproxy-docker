# V2Ray installer for a two VPS setup

**This project is a bash script that aims to setup a [V2Ray](https://www.v2fly.org/en_US/) Proxy on a Linux servers, as easily as possible!**

The readme might look a liitle verbose to make it easy to build on top of it. You can simply skip to terminology and usage. <br>
V2fly (a community-driven edition of V2Ray) is not a proxy itself, it's a modular platform for proxies that make it possible to setup different combinations of **Proxy Protocls (like ShadowSocks, Vmess, Vless, Trojan,...)** and **Transports (like TCP, Websocket(ws), TLS, HTTP2 ...)** proxies. Currently this combinations are the default ones: **vmess-ws, shadowsocks-ws and vless-ws**.

Note: Since these transports overlap, you might be using more than one of them at a time. For example vmess-ws-TLS uses vmess as proxy protocol, websocket as transport and wraps the whole thing in TLS for security. 

## Requirements
* Two VPS servers. A domestic VPS hosted in your country and a none-domestic one hosted elsewhere.

* docker and docker-compose should be installed and running on those servers. Instruction to install:
    * **docker**
        * If your OS has snap installed, probably installing docker with the following command is the easiest way.
            ```bash
            sudo snap install docker
            ```
        * If not, follow [Install Docker Engine
](https://docs.docker.com/engine/install/#server). You may take a look at [Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/) to add your user to docker group as well.

    * **docker-compose** Download the binary (compatible with the installed docker engine in previous step) [Install Docker Compose](https://docs.docker.com/compose/install/)
    
*NOTE: If installed docker via snap, docker-compose is also installed with it.

## Terminology

* Upstream Server: A server that has free access to the Internet.
* Bridge Server: A server that is available to clients and has access to the upstream server. HAProxy runs here. This server acts only as a layer4 TCP or layer7 HTTP relay(i.e recieves client request => forwards request to Upstream server => recieve response from upstream => forwards response to client )
* Client: A user-side application with access to the bridge server.

```
(Client) <-> [ Bridge Server (HAProxy) ] <-> [ Upstream Server (V2Ray) ] <-> (Internet)
```

## Usage
### Server
Clone the repo and execute the insall script on both bridge and upstream server. Answer the questions asked by the script and it will take care of the rest.

```bash
git clone https://github.com/UZziell/v2ray-haproxy-docker;
cd v2ray-haproxy-docker;
chmod +x v2ray-install.sh;
sudo ./v2ray-install.sh
```

Run the script again to add or remove clients!


### Client

The bridge server exposes these proxy protocols:
* Shadowsocks
* VMESS
* HTTP
* SOCKS

##### Shadowsocks Protocol

Shadowsocks is a popular proxy protocol.
You can find many client apps to use the Shadowsocks proxy on your devices.
These are recommended client apps:
* [Shadowsocks for macOS](https://github.com/shadowsocks/ShadowsocksX-NG/releases)
* [Shadowsocks for Linux](https://github.com/shadowsocks/shadowsocks-libev)
* [Shadowsocks for Windows](https://github.com/shadowsocks/shadowsocks-windows/releases)
* [Shadowsocks for Android](https://github.com/shadowsocks/shadowsocks-android/releases)
* [ShadowLink for iOS](https://apps.apple.com/us/app/shadowlink-shadowsocks-vpn/id1439686518)

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
<p align="right">(<a href="#top">back to top</a>)</p>
