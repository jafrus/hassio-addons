# hassio-addons

https://home-assistant.io/hassio

## Autossh [![](https://images.microbadger.com/badges/version/odinuge/hassio-addon-autossh-armhf.svg)](https://microbadger.com/images/odinuge/hassio-addon-autossh-armhf "Get your own version badge on microbadger.com")
Simple autossh addon. The addon creates a ssh keypair, and uses it
to connect to to the given host. The public key can be found in the
log after the first startup.

Remember to set `GatewayPorts clientspecified` in sshd-config if you
would like to open ports on other interfaces than localhost.

**IMPORTANT**: If you set `GatewayPorts yes`, all forwarded ports will
listen on all interfaces, `0.0.0.0`. `GatewayPorts clientspecified`
is preferable.

## Apache config
### Modules
```
  a2enmod proxy_wstunnel
  a2enmod proxy
  a2enmod proxy_http
  a2enmod proxy_balancer
  a2enmod lbmethod_byrequests
```
### *.conf
```
  ServerAdmin webmaster@localhost
  ServerName hassio.example.com

  ProxyPreserveHost On
  ProxyRequests Off
  ProxyPass / http://127.0.01:8123
  ProxyPassReverse / http://127.0.0.1:44400
  ProxyPass /api/websocket ws://127.0.0.1:44400/api/websocket
  ProxyPassReverse /api/websocket ws://127.0.0.1:44400/api/websocket

  RewriteEngine on
  RewriteCond %{HTTP:Upgrade} =websocket [NC]
  RewriteRule /(.*)  ws://127.0.0.1:44400/$1 [P,l]
  RewriteCond %{HTTP:Upgrade} !=websocket [NC]
  RewriteRule /(.*)  http://127.0.0.1:44400/$1 [P,l]
```

### Licence
MIT (c) Odin Ugedal
