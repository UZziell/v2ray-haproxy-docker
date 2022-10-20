# V2Ray installer for a two VPS setup

**This project is a bash script that aims to setup a [V2Ray](https://www.v2fly.org/en_US/) Proxy on a Linux servers, as easily as possible!**

The readme might look a liitle verbose to make it easy to build on top of it. You can simply skip to terminology and usage. <br>
V2fly (a community-driven edition of V2Ray) is not a proxy itself, it's a modular platform for proxies that make it possible to setup different combinations of **Proxy Protocls (like ShadowSocks, Vmess, Vless, Trojan,...)** and **Transports (like TCP, Websocket(ws), TLS, HTTP2 ...)**. Currently this combinations are the default ones: **vmess-ws, shadowsocks-ws and vless-ws**.

Note: Since these transports overlap, you might be using more than one of them at a time. For example vmess-ws-TLS uses vmess as proxy protocol, websocket as transport and wraps the whole thing in TLS for security. 

## Requirements
* Two VPS servers. A domestic VPS hosted in your country and a non-domestic one hosted elsewhere.

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

Run the script again to uninstall the service.


### Client
Scan the generated QRCodes or use the links with app that support the default used protocols(VMess, Vless, Shadowsocks)


##### VMess and Vless Protocol
In theory any v2ray client should work.

Tested clients apps:
* **Android**: [v2rayNG](https://play.google.com/store/apps/details?id=com.v2ray.ang&hl=en&gl=US)

* **IOS**: [Fair](https://apps.apple.com/us/app/fair-vpn/id1533873488)  - [ShadowLink](https://apps.apple.com/us/app/shadowlink-shadowsocks-vpn/id1439686518)

* **Linux**: v2ray package or [v2fly](https://github.com/v2fly/fhs-install-v2ray)

* **MacOs**: [V2rayU](https://github.com/yanue/V2rayU/tree/master)

* **Windows**: [nekoray](https://github.com/MatsuriDayo/nekoray/releases)



#### Shadowsocks Protocol

These are recommended client apps:
* **Android**: [Shadowsocks](https://play.google.com/store/apps/details?id=com.github.shadowsocks&hl=en&gl=US) + [V2ray Plugin](https://play.google.com/store/apps/details?id=com.github.shadowsocks.plugin.v2ray&hl=en&gl=US)



## P.S.

This repository is kind of forked from [v2ray-docker-compose
](https://github.com/miladrahimi/v2ray-docker-compose).
Regards to [@miladrahimi](https://github.com/miladrahimi) and other contributors.

<p align="right">(<a href="#top">back to top</a>)</p>

