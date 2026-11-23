### Apps

| App | Used for |
|---|---|
| Flux Operator | GitOps: syncs `flux/` from this repo |
| SOPS + age | encrypted secrets in git |
| Traefik | ingress, Let's Encrypt wildcard via OVH DNS |
| Blocky | LAN DNS: `*.zxnak37.ovh`, ad blocking |
| Longhorn | replicated volumes on the NVMe |
| kube-prometheus-stack | Prometheus, Alertmanager, Grafana |
| Loki + Alloy | pod logs in Grafana (Alloy tails `/var/log/pods`, Loki stores 7d) |
| Coroot | eBPF service map |
| Gatus | uptime checks, public status page |
| Tailscale | Funnel: public URL for Gatus, no open ports |
| Reloader | restarts pods on config change |
| system-upgrade-controller | k3s upgrades |
| kured | node reboots after kernel updates |

### Conventions

- **Ingress**: plain `Ingress` with host `<name>.zxnak37.ovh`; no `tls:` block, no cert annotations. Traefik serves the wildcard cert and redirects HTTP to HTTPS. LAN DNS (Blocky) already resolves every subdomain.
- **Secrets**: `*.enc.yaml`, `sops -e -i <file>` (only `data`/`stringData` get encrypted). Flux decrypts.
- **Homepage tile**: `apps/homepage/configmap.yaml`. **Uptime check**: `apps/gatus/configmap.yaml`.
- **2+ replicas**: `topologySpreadConstraints` with `maxSkew: 1`, `nodeTaintsPolicy: Honor`, `matchLabelKeys: [pod-template-hash]`, plus a PDB. No PDB on single-replica apps (blocks kured drains).