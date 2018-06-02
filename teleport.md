Configure auth and proxy:
=========
https://gravitational.com/teleport/docs/admin-guide/

Installation at:
https://gravitational.com/teleport/download
```
curl https://get.gravitational.com/teleport-v2.6.1-linux-amd64-bin.tar.gz -O 
tar xzf teleport-v2.6.1-linux-amd64-bin.tar.gz
cd teleport
sh install
```
Next, create initial config
```
teleport configure > /etc/teleport.conf`
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
Add below content to `/etc/systemd/system/teleport.service` on each node, remember to replace `node-token` with your secret token:
```
[Unit]
Description=Teleport SSH Service
After=network.target 

[Service]
Type=simple
Restart=on-failure
ExecStart=/usr/local/bin/teleport start --roles=node --token=node-token --auth-server=10.39.124.223 --labels uptime=[1m:"uptime -p"],kernel=[1h:"uname -r"] --pid-file=/var/run/teleport.pid
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
