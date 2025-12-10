# Navidrome quadlets

## Usage

_(Note: this describes the step to follow for a conventional secured rootful setup. A rootless setup would need some modifications to be made, specifically for caddy)_

1. Copy the caddy and navidrome folders into `/etc/containers/systemd/`
2. Remove or comment unused services in `Caddyfile`
3. Replace all occurrences of `example.org` with your navidrome domain name: `sed -i 's/example.org/yourdomain.org/' navidrome/*`
4. Create the following directories:
   ```sh
    sudo mkdir -p /home/containers/navidrome
    sudo chown -R 1000:1000 /home/containers/navidrome
   ```
5. Run `systemctl daemon-reload`
6. Run `systemctl start navidrome


