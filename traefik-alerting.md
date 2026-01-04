# Traefik / Blackbox alert runbook

## SERVICE DOWN (critical)
Detta betyder att Blackbox får svar men tjänsten svarar fel (`probe_success == 0`).

Checklista:
1. Kolla Traefik-container: `docker ps | grep traefik`
2. Kolla Traefik-loggar: `docker logs traefik --since 30m`
3. Kolla att /healthz svarar internt och externt:
   - `curl -sk https://info.bergsland.se/healthz`
   - `curl -sk https://info.majk.one/healthz`
4. Om Traefik nyligen restartats: verifiera cert-mounts + nät (`proxy_net`).

## MONITORING DEGRADED (warning)
Detta betyder att Grafana/Prometheus inte får data (`absent(...)`), inte nödvändigtvis att Traefik är nere.

Checklista:
1. Kolla Prometheus-container: `docker ps | grep prometheus`
2. Kolla Blackbox-container: `docker ps | grep blackbox`
3. Prometheus Targets: Status → Targets (UI)
4. Kolla disk/I/O/backuper runt tiden för alerten.