## Homelab

k3s on Raspberry Pi 5: nodes set up by Ansible, apps deployed by Flux.

- `ansible/`: node prep, k3s, Flux bootstrap (`ansible-playbook site.yml`, first run with `-K`)
- `flux/`: what Flux syncs (FluxInstance, `apps` Kustomization)
- `apps/`: one folder per app; secrets encrypted with SOPS (age)

Updates: OS by unattended-upgrades, k3s by system-upgrade-controller, reboots by kured.
