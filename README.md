# diskmind-agent

Push agent for [diskmind.net](https://diskmind.net) - SMART disk health monitoring for Linux and macOS.

Collects SMART data from your drives via `smartctl` and pushes it to your diskmind dashboard.

## Quick start

```bash
sudo curl -sSL -o /usr/local/bin/diskmind_scan https://raw.githubusercontent.com/efnats/diskmind-agent/main/diskmind_scan
sudo chmod +x /usr/local/bin/diskmind_scan
sudo diskmind_scan --push
```

On first run, you'll be prompted for your API token. Sign up at [diskmind.net](https://diskmind.net) and find it under **Settings > API Tokens**.

## Automate with cron

```bash
echo "*/15 * * * * root /usr/local/bin/diskmind_scan --push --log /var/log/diskmind.log" | sudo tee /etc/cron.d/diskmind
```

## Requirements

- Linux or macOS
- `smartmontools` (smartctl)
- `curl`
- Root access (for smartctl)

## Options

```
diskmind_scan [OPTIONS]

  --push [URL]       Push data to server (default: https://diskmind.net)
  --host HOSTNAME    Override hostname (default: system hostname)
  --token TOKEN      API token (also reads from /etc/diskmind/token)
  --log FILE         Log to file with timestamps (for cron)
  --buffer-dir DIR   Buffer directory for offline scans
                     (default: /var/lib/diskmind/buffer)
```

## Self-hosted

If you run your own diskmind server, point `--push` at it:

```bash
sudo diskmind_scan --push http://your-server:8085
```

## How it works

The agent runs `smartctl --scan-open` to discover all drives, reads their SMART attributes, and sends the data as CSV to the diskmind server. If the server is unreachable, scans are buffered locally and synced on the next successful push.

## License

MIT
