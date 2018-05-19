http://blog.skufel.net/2015/09/how-to-publish-multiple-urls-with-single-ip-and-haproxy/
https://blog.cavebeat.org/2017/07/haproxy-multi-domain-ssl-termination/

Przykladowy config:

```
global
        log /dev/log    local0
        log /dev/log    local1 notice
        chroot /var/lib/haproxy
        stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
        stats timeout 30s
        user haproxy
        group haproxy
        daemon
        debug

        # Default SSL material locations
        ca-base /etc/ssl/certs
        crt-base /etc/ssl/private

        # Default ciphers to use on SSL-enabled listening sockets.
        # For more information, see ciphers(1SSL). This list is from:
        #  https://hynek.me/articles/hardening-your-web-servers-ssl-ciphers/
        # An alternative list with additional directives can be obtained from
        #  https://mozilla.github.io/server-side-tls/ssl-config-generator/?server=haproxy
        ssl-default-bind-ciphers ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:RSA+AESGCM:RSA+AES:!aNULL:!MD5:!DSS
        ssl-default-bind-options no-sslv3
        tune.ssl.default-dh-param 4096 #tune DH to 4096

        ssl-dh-param-file /etc/haproxy/certs/dhparams.pem

defaults
        log     global
        mode    http
        option  httplog
        option  dontlognull
        timeout connect 5000
        timeout client  50000
        timeout server  50000
        errorfile 400 /etc/haproxy/errors/400.http
        errorfile 403 /etc/haproxy/errors/403.http
        errorfile 408 /etc/haproxy/errors/408.http
        errorfile 500 /etc/haproxy/errors/500.http
        errorfile 502 /etc/haproxy/errors/502.http
        errorfile 503 /etc/haproxy/errors/503.http
        errorfile 504 /etc/haproxy/errors/504.http


frontend frontent_public
  bind *:80
# Add multiple certificates, one for each domain
  bind *:443 ssl crt /etc/ssl/haproxy/jarosik.online.pem crt /etc/ssl/haproxy/api.jarosik.online.pem crt /etc/ssl/haproxy/cloud.jarosik.online.pem crt /etc/ssl/haproxy/healthcheck.jarosik.online.pem

  mode http
# These two are required to make SNI-field based rules to work
  # tcp-request inspect-delay 5s
  # tcp-request content accept if { req_ssl_hello_type 1 }

# redirect http to https
  redirect scheme https code 301 if !{ ssl_fc }
# :::ACL:::Define ACL for each Subdomain to terminate
  acl webserver1-acl hdr(host) -i jarosik.online
  acl webserver2-acl hdr(host) -i api.jarosik.online
  acl cloud-acl hdr(host) -i cloud.jarosik.online
  acl healthcheck-acl hdr(host) -i healthcheck.jarosik.online

# :::BACKEND:::Use Backend Section
  use_backend webserver1-backend if webserver1-acl
  use_backend webserver2-backend if webserver2-acl
  use_backend nextcloud-backend if cloud-acl
  use_backend healthcheck-backend if healthcheck-acl

backend webserver1-backend
  mode http
  server webserver1 10.39.124.47:80 check
  http-request set-header X-Forwared-Port %[dst_port]
  http-request add-header X-Forwarded-Proto https if { ssl_fc }

backend webserver2-backend
  mode http
  server webserver2 10.39.124.162:80 check
  http-request set-header X-Forwared-Port %[dst_port]
  http-request add-header X-Forwarded-Proto https if { ssl_fc }

backend nextcloud-backend
  mode http
  server nextcloud 10.39.124.142:80 check
  http-request set-header X-Forwared-Port %[dst_port]
  http-request add-header X-Forwarded-Proto https if { ssl_fc }

backend healthcheck-backend
  mode http
  server healthcheck 10.39.124.47:80 check
  http-request set-header X-Forwared-Port %[dst_port]
  http-request add-header X-Forwarded-Proto https if { ssl_fc }
```
