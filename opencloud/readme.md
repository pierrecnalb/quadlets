# OpenCloud quadlets

1. Copy the caddy and opencloud folders into `/etc/containers/systemd/`
2. Remove or comment unused services in `Caddyfile`
3. Replace all occurrences of `example.org` with your opencloud domain name: `sed -i 's/example.org/yourdomain.org/' opencloud/*`
4. Create the following secrets: 
   ```sh
    printf <secret> | podman secret create opencloud_admin -
    printf <secret> | podman secret create opencloud_smtp -
    printf <secret> | podman secret create collabora_pwd -
   ```
5. Create the following directories:
   ```sh
    sudo mkdir -p /home/containers/opencloud/{config,data}
    sudo chown -R 1000:1000 /home/containers/opencloud
   ```
6. Run `systemctl daemon-reload`
7. Run `systemctl start opencloud`

