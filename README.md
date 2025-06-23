
# HONEYPOT

A modular, terminal-based and web-based honeypot system to capture **IP addresses**, **usernames**, **passwords**, and **commands** via multiple protocols. Currently supports:
- **SSH (custom shell emulator)**
- **HTTP (WordPress-style login trap)**

Written in **Python** using Flask, Paramiko, and logging with file rotation.

---

## 🚀 Installation

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/honeypy.git
cd honeypy
```

### 2. Set Permissions
Ensure the launcher script is executable:
```bash
chmod +x honeypy.py
```

### 3. Generate SSH Key
```bash
mkdir static
cd static
ssh-keygen -t rsa -b 2048 -f server.key
```
Make sure `server.key` is in the `static/` directory and accessible to the script.

---

## 🧪 Usage

Run the honeypot using:

```bash
python3 honeypy.py -a <bind_ip> -p <port> --ssh
```

### Arguments

| Argument       | Description                            |
|----------------|----------------------------------------|
| `-a` `--address` | IP address to bind (e.g., `0.0.0.0`) |
| `-p` `--port`   | Port number to listen on              |
| `--ssh`         | Start SSH honeypot                    |
| `--http`        | Start HTTP honeypot                   |
| `-u` `--username` | (Optional) Set expected username    |
| `-pw` `--password` | (Optional) Set expected password   |

### Examples

```bash
# Start SSH honeypot
python3 honeypy.py -a 0.0.0.0 -p 2223 --ssh -u admin -pw secret

# Start HTTP honeypot
python3 honeypy.py -a 0.0.0.0 -p 5000 --http -u admin -pw deeboodah
```

---

## 📁 Logging

| Log File         | Description                                 |
|------------------|---------------------------------------------|
| `cmd_audits.log` | Captures commands typed in SSH shell        |
| `creds_audits.log` | Captures SSH login attempts (CSV format)  |
| `http_audits.log` | Captures HTTP login attempts               |

Log files rotate at 2000 bytes with 5 backups.

---

## 💻 Honeypot Types

### ✅ SSH
- Simulates a shell with commands like `pwd`, `ls`, `cat`, etc.
- Logs all typed commands and login attempts
- Easily configurable username/password

### ✅ HTTP
- Mimics a WordPress login page using Flask
- Logs all submitted credentials
- Customizable HTML login form (`wp-admin.html` in `/templates/`)

---

## 📊 Dashboard (Optional)

A future `web_app.py` dashboard will display:
- Top usernames, IPs, and commands
- Country-code lookups (via CleanTalk API)
- Uses Dash + Pandas

---

## ☁️ VPS Hosting Tips

To run on a public server:
```bash
sudo ufw enable
sudo ufw allow 2223/tcp
sudo ufw allow 5000/tcp
```

---

## 🛠 systemd (Background Execution)

Example unit file (`honeypy.service`):
```ini
[Unit]
Description=HONEYPY SSH Honeypot

[Service]
ExecStart=/usr/bin/python3 /path/to/honeypy.py -a 0.0.0.0 -p 2223 --ssh -u admin -pw secret
Restart=always

[Install]
WantedBy=multi-user.target
```

Then run:
```bash
sudo cp honeypy.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable honeypy.service
sudo systemctl start honeypy.service
```

---

## 🧩 Future Features

- Telnet, HTTPS, SMTP support
- Docker containerization
- Real-time dashboard
- IP reputation tagging

---

## 📚 References

- https://securehoney.net/blog/how-to-build-an-ssh-honeypot-in-python-and-docker-part-1.html  
- https://medium.com/@abdulsamie488/deceptive-defense-building-a-python-honeypot-to-thwart-cyber-attackers-2a9d2ced2760  
- https://gist.github.com/cschwede/3e2c025408ab4af531651098331cce45  

---

> ⚠️ For educational and research use only. Do not deploy in production networks without authorization.
