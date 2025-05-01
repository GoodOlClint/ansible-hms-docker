# Netdata

## Claiming Instance

To claim your Netdata node when prompted in the webpage, run this command:

```bash
docker exec -it netdata cat /var/lib/netdata/netdata_random_session_id
```

## Accessing via Traefik

Since Netdata uses the `host` network mode, it cannot be connected to Traefik via the Docker internal network which results in it not being "auto-discovered" by Traefik.

Instead, you will have to treat it as an "external service" to Traefik. More documentation is available in the [External Services](./traefik/other-svcs.md) Traefik documentation for configuring external services, but the below should help.

In `inventory/group_vars/all/traefik.yml`, set `traefik_ext_hosts_enabled` to `yes` and uncomment the lines in `traefik_ext_hosts_list` that are related to Netdata (like below):

```yml
...
{
  friendly_name: netdata,
  subdomain_name: netdata,
  backend_url: "http://netdata.{{ hms_docker_domain }}:19999",
  enabled: yes,
  authentik: no,
  authentik_provider_type: proxy
}
```
