# ASA_IAC — Demo Ansible project for Cisco ASA & IOS

This repository is a small demo to show how to deploy simple configurations to Cisco ASA and IOS devices using Ansible collections (no Jinja templates).

Quick start

1. Install collections referenced in `requirements.yml`:

```bash
ansible-galaxy collection install -r requirements.yml
```

2. Ensure your `inventory.yml` contains the devices and that `host_vars/<hostname>/` overrides `asa_config_lines` / `ios_config_lines` as needed.

3. Run the ASA playbook (dry-run):

```bash
ansible-playbook playbooks/deploy_asa.yml --check
```

4. Run the IOS playbook (apply):

```bash
ansible-playbook playbooks/deploy_ios.yml
```

Connectivity and credentials

- This demo expects `connection: network_cli` and that credentials are provided via `ansible_user`/`ansible_password`/`ansible_ssh_common_args` in your inventory or via `--extra-vars` or Ansible Vault.
- For lab usage, prefer `host_vars/<device>` for per-device config lines.

Notes for instructors

- The roles use `cisco.asa.asa_config` and `cisco.ios.ios_config` modules. They accept a `lines:` list which avoids Jinja templating and teaches line-based config.
- Replace example lines in `roles/*/vars/main.yml` or better, put per-host lines under `host_vars/DEVICE.yml`.
Ansible project to deploy Cisco ASA and IOS configurations

Quick start

1. Install required collections:

   ansible-galaxy collection install -r requirements.yml

2. Edit `inventory.yml`, `host_vars/` or `group_vars/` to provide device IPs and credentials.

3. Run a playbook in check mode:

   ansible-playbook playbooks/deploy_ios.yml -C

4. Run for real:

   ansible-playbook playbooks/deploy_ios.yml

Notes

- Uses `network_cli` connection. Ensure SSH connectivity and that username/password/privilege are configured in host_vars.
- Templates are in `roles/*/templates` and can be adapted.
# ASA_IAC
Demo for ASA Infrastructure as Code Framework using AWX 
