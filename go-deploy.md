### Create a systemd service file to manage your Go application:


```sudo nano /etc/systemd/system/myapp.service```

### Add the following content to the file:

```
[Unit]
Description=My Go Application
After=network.target

[Service]
ExecStart=/usr/local/bin/myapp
Restart=always
User=nobody
Group=nogroup
Environment=PORT=8080
StandardOutput=file:/var/log/myapp.log
StandardError=file:/var/log/myapp.log

[Install]
WantedBy=multi-user.target
```

Adjust the ExecStart and Environment variables as needed.

### Enable and start the service:
```
sudo systemctl enable myapp
sudo systemctl start myapp
```
### Verify the service status:

```sudo systemctl status myapp```

### Create Log File and Set Permissions

```
sudo touch /var/log/myapp.log
sudo chown nobody:nogroup /var/log/myapp.log
```

### use after modifying service file

```sudo systemctl daemon-reload```

### log rotation configuration for indefinitely growing file

Create a logrotate configuration file for your Go application logs, e.g., /etc/logrotate.d/myapp

```
/var/log/myapp.log {
    daily
    missingok
    rotate 14
    compress
    delaycompress
    notifempty
    create 0640 nobody nogroup
    postrotate
        systemctl restart myapp > /dev/null 2>&1 || true
    endscript
}
```