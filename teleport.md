Configure auth and proxy:
=========
https://gravitational.com/teleport/docs/admin-guide/

Installation at:
https://gravitational.com/teleport/download
```
curl https://get.gravitational.com/teleport-v2.7.4-linux-amd64-bin.tar.gz -O
tar xzf teleport-v2.6.1-linux-amd64-bin.tar.gz
cd teleport
sh install
```
Next, create initial config
```
teleport configure > /etc/teleport.yaml
```
and edit it according to your needs.

Run it as daemon, add below content to /etc/systemd/system/teleport.service:
```
[Unit]
Description=Teleport SSH Service
After=network.target 

[Service]
Type=simple
Restart=on-failure
ExecStart=/usr/local/bin/teleport start --config=/etc/teleport.yaml --pid-file=/var/run/teleport.pid
ExecReload=/bin/kill -HUP $MAINPID
PIDFile=/var/run/teleport.pid

[Install]
WantedBy=multi-user.target
```

Adding new users:
```
tctl users add joe joe,root
```


Adding a new node
=======
Create config file in `/etc/teleport.yaml` and remember to replace `node-token` with your secret token:
```
teleport:
  nodename: nextcloud
  data_dir: /var/lib/teleport
  pid_file: /var/run/teleport.pid
  auth_token: node-token
  connection_limits:
    max_connections: 1000
    max_users: 250
  log:
    output: stderr
    severity: INFO
ssh_service:
  enabled: "yes"
  listen_addr: 0.0.0.0:3022
  labels:
    role: cloud
  commands:
  - name: hostname
    command: [/bin/hostname]
    period: 1m0s
  - name: arch
    command: [/bin/uname, -p]
    period: 1h0m0s
  - name: disk
    command: [/bin/df, /, -lhP]
    period: 0h1m0s
  - name: ram
    command: [/usr/bin/free, -mh]
    period: 0h1m0s
  - name: top
    command: [/usr/bin/top, -n, 1, -b]
    period: 0h1m0s
  - name: uptime
    command: [/usr/bin/uptime, -p]
    period: 1m0s
```
Add below content to `/etc/systemd/system/teleport.service`:
```
[Unit]
Description=Teleport SSH Service
After=network.target 

[Service]
Type=simple
Restart=on-failure
ExecStart=/usr/local/bin/teleport start --roles=node  --auth-server=10.39.124.223 --pid-file=/var/run/teleport.pid
ExecReload=/bin/kill -HUP $MAINPID
PIDFile=/var/run/teleport.pid

[Install]
WantedBy=multi-user.target
```
and then run: `service teleport start`, check if it is connected via `cat /var/lib/syslog`. You must see something silimar to:
```
Started Teleport SSH Service.
[NODE]    Service is starting on 0.0.0.0:3022.
```
If everything went well, run: `systemctl enable teleport` to add it persistenly on every boot

Optionally: `disk_usage.sh`
```
#!/bin/bash
/bin/df / -h | /usr/bin/tail -1 | /usr/bin/awk '{print $1" "$3" of "$2}'
```
`ram_usage.sh`:
```
#!/bin/bash
free -mh | tail -2 | head -1 | awk '{ print $3" of "$2 }'
```
