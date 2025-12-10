# Immich quadlets

## Usage

_(Note: this describes the step to follow for a conventional secured rootful setup. A rootless setup would need some modifications to be made, specifically for caddy)_

1. Copy the caddy and immich folders into `/etc/containers/systemd/`
2. Remove or comment unused services in `Caddyfile`
3. Create the following secrets: 
   ```sh
    printf <secret> | podman secret create immich_db_pwd -
   ```
4. Create the following directories:
   ```sh
    sudo mkdir -p /home/containers/immich
    sudo chown -R 1000:1000 /home/containers/immich
   ```
5. Run `systemctl daemon-reload`
6. Run `systemctl start immich`

