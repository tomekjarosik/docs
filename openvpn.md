
https://www.digitalocean.com/community/tutorials/how-to-set-up-an-openvpn-server-on-ubuntu-18-04

Disable proc limit in: `/lib/systemd/system/openvpn@.service` Note the exact name!!!


When choosing TCP protocol, disable option in /etc/openvpn/server.conf, lke that:
```
; explicit-exit-notify 1
```
