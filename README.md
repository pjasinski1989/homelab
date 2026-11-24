## Homelab

k3s on Raspberry Pi 5: nodes set up by Ansible, apps deployed by Flux.

- `ansible/`: node prep, k3s, Flux bootstrap (`ansible-playbook site.yml`, first run with `-K`)
- `flux/`: what Flux syncs (FluxInstance, `apps` and `configs` Kustomizations)
- `apps/`: one folder per app; secrets encrypted with SOPS (age)
- `configs/`: resources that need CRDs from charts in `apps/`, applied after it

Updates: OS by unattended-upgrades, k3s by system-upgrade-controller, reboots by kured.

Backups: Longhorn, daily to Backblaze B2, opt-in per PVC.

![Architecture](docs/architecture.svg)
