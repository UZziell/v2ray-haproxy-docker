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

You can add one of the following lines to the `$HOME/.ssh/ssh_config` file to use the HTTP or SOCKS proxies in your SSH connections.

* HTTP: ```ProxyCommand /usr/bin/nc -X connect -x 127.0.0.1:HTTP_PROXY_PORT %h %p```
* SOCKS: ```ProxyCommand /usr/bin/nc -X 5 -x 127.0.0.1:SOCKS_PROXY_PORT %h %p```
