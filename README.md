# Ansible for Cisco ASA & IOS Automation (PoC)

This repository serves as a proof of concept (PoC) for using Ansible as the automation engine in a mixed Cisco IOS and ASA environment.

The project demonstrates a variable-driven approach to configuration management. All device-specific settings (like IP addresses and route definitions) are abstracted into `host_vars` files, while group-wide settings (like credentials and connection types) are stored in `group_vars`. This allows the Ansible roles and playbooks to remain generic.

## Core Functionality

This project's main playbook, `network_deploy.yml`, targets two groups of devices: `firewalls` (Cisco ASA) and `routers` (Cisco IOS).

* **For Cisco IOS Routers (`routers` group):**
    * Configures L3 interfaces using `cisco.ios.ios_l3_interfaces`.
    * Configures static routes using `cisco.ios.ios_static_routes`.
    * Variables for these tasks are defined in `host_vars/R1.yml`, `R2.yml`, etc..

* **For Cisco ASA Firewalls (`firewalls` group):**
    * Generates interface configurations from a Jinja2 template (`roles/asa_interfaces/templates/interfaces.j2`).
    * Generates static route configurations from a Jinja2 template (`roles/asa_static_routes/templates/static_routes.j2`).
    * Pushes the generated configurations to the devices using `cisco.asa.asa_config`.
    * Variables are defined in `host_vars/ASA_01.yml` and `host_vars/ASA_02.yml`.

All tasks notify a handler to save the configuration upon a change.

## How to Use

1.  **Install Collections:**
    Install the required Ansible collections using `requirements.yml`.
    ```bash
    ansible-galaxy collection install -r requirements.yml
    ```

2.  **Edit Inventory:**
    Update `inventory.yml` with the management IP addresses of your ASA and IOS devices.

3.  **Update Credentials:**
    Modify `group_vars/firewalls.yml` and `group_vars/routers.yml` with the correct SSH and `enable` credentials for your environment.

4.  **Customize Variables:**
    Review and edit the files in `host_vars/` to match your desired interface IPs, descriptions, and static routes.

5.  **Run (Dry-Run):**
    Perform a dry-run using `--check` to see what changes would be made without applying them.
    ```bash
    ansible-playbook network_deploy.yml --check
    ```

6.  **Deploy:**
    Run the playbook to apply the configuration.
    ```bash
    ansible-playbook network_deploy.yml
    ```

## Future Roadmap

This PoC provides the foundation for network automation. Next steps for this project include:

* Building out security zone specifics for the ASA firewalls (e.g., access-lists, object-groups).
* Deploying site-to-site IPSec tunnels between the ASA devices.