---
tags:
  - development
  - ssh
  - ssh-keygen
  - ngrok
---

# Expose ssh server with ngrok

This setup exposes an ssh server to the public without opening a router port.

## Configure sshd

### Security tips:
1. Use an ssh key instead of the server account password.
2. Use a passphrase with you ssh key.
3. Use a port other than 22.
4. Use an allow list.
5. Don't allow root login.
6. Enable verbose logging.
7. Periodically update sshd.

### Sshd configuration

```bash
# Install openssh-server

sudo systemctl enable --now sshd

sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

sudo nano /etc/ssh/sshd_config

sudo systemctl restart sshd # run after applying the configuration
```

#### /etc/ssh/sshd_config

```
Port 43689
PasswordAuthentication no
PermitRootLogin no
LogLevel INFO
```


## Setup ngrok

```yml
version: "3"
agent:
  authtoken: <your-auth-token>
tunnels:
  ssh:
    proto: tcp
    addr: 22
```

Generate ngrok configuration as systemd service:

```bash
sudo ngrok service install --config /path/to/.config/ngrok/ngrok.yml
```

Service Unit File:
```toml
[Unit]
Description=ngrok secure tunnel client
ConditionFileIsExecutable=/path/to/ngrok

[Service]  
StartLimitInterval=5
StartLimitBurst=10
ExecStart=/path/to/ngrok "service" "run" "--config" "/path/to/.config/ngrok/ngrok.yml"
Restart=always
RestartSec=15
EnvironmentFile=-/etc/sysconfig/ngrok

[Install]
WantedBy=multi-user.target
```

Enable systemd service to autostart:
```bash
sudo systemctl daemon-reload
sudo systemctl enable ngrok.service
sudo systemctl start ngrok.service
```

### Get ngrok url

```bash
NGROK_URL="$(curl http://localhost:4040/api/tunnels | jq ".tunnels[0].public_url")"
echo "$NGROK_URL" # example: tcp://0.tcp.eu.ngrok.io:18137
```


## Test the connection

Generate a ssh key pair on the client machine:
```
ssh-keygen -t rsa -b 4096
```

Copy the public key it on the target (it will be added to `$HOME/.ssh/authorized_keys` file):
```
ssh-copy-id -i <$PUBLIC_KEY_PATH> <$USERNAME_ON_HOST>@0.tcp.eu.ngrok.io -p 18137
```

Connect to the ssh server:
```bash
ssh <$USERNAME_ON_HOST>@0.tcp.eu.ngrok.io -p 18137 -i "$HOME/.ssh/id_rsa"
```


## References

- https://www.brandonrohrer.com/ssh_at_home
- https://ngrok.com/docs/universal-gateway/tcp
- https://ngrok.com/docs/getting-started/#running-ngrok-persistently
