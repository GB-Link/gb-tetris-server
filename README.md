# GB Tetris Server

Backend WebSocket server for [gb-tetris-web](https://github.com/starlarkus/gb-tetris-web).

## Requirements

- **Python 3.11+**
- **SSL/TLS**: Server must either be configured with an SSL certificate or run behind a reverse proxy with SSL termination (recommended)
  > Browsers require `wss://` for WebSocket connections from `https://` pages — plain `ws://` will be blocked.

## Installation

From a clean Debian environment:

```bash
# Install dependencies
apt install python3 python3-pip git

# Install Python packages
pip install websockets

# Clone and run
cd ~
git clone --depth 1 https://github.com/starlarkus/gb-tetris-server
cd gb-tetris-server
python3 server.py
```

## Usage

The server runs on port `5678` by default. Configure your reverse proxy (nginx, caddy, etc.) to forward WebSocket connections to this port.

Copy `template.env` to `.env`. `REPORT_TOKEN` is the shared secret for status reports on WebSocket path `/report`. If it is missing or blank, `/report` does not exist (HTTP 404 before the handshake). A request is `{"type":"get_report","token":"..."}`.

## Auto-start

```bash
apt install screen
chmod +x startdetached.sh
crontab -e
@reboot screen -S tetrisserver -d -m ~/gb-tetris-server/startdetached.sh
```

Access the running server window with `screen -r tetrisserver`