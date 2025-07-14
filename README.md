# Linux and Windows System Patching and Validation Playbook

## Overview
This repository contains Ansible playbooks and inventory structure for automated OS patching, reporting, and notification for both Linux and Windows hosts. The solution is designed for maintainability, security, and ease of use, leveraging group variables, Ansible Vault, and modular playbooks.

## Directory Structure
```
  linux.yml                # Playbook for Linux patching and reporting
  windows.yml              # Playbook for Windows patching and reporting
  main.yml                 # Main playbook to orchestrate patching and notification
  inventory/
    hosts.ini              # Inventory file listing Linux and Windows hosts
    group_vars/
      all.yml              # Shared variables for all hosts
      all/
        vault.yml          # Vault-encrypted secrets for all hosts (Mailjet API)
      linux/
        linux.yml          # Linux-specific group variables
      windows/
        vault.yml          # Vault-encrypted Windows credentials
        windows.yml        # Windows-specific group variables
```

## Getting Started

### Prerequisites
- Ansible 2.9+ installed on the control node
- Access to target Linux and Windows hosts
- Target hosts must have python installed
- Mailjet account for email notifications
- Ansible Vault for encrypting sensitive data

### Setup
1. **Clone the repository**
2. **Configure Inventory**
   - Edit `inventory/hosts.ini` to list your Linux and Windows hosts under the appropriate groups.

3. **Set Group Variables**
   - Edit `inventory/group_vars/all.yml` for shared variables (email, Mailjet, log paths, etc.)
   - Edit `inventory/group_vars/linux/linux.yml` and `inventory/group_vars/windows/windows.yml` for OS-specific settings.

4. **Configure Secrets**
   - Edit `inventory/group_vars/all/vault.yml` for Mailjet API credentials.
   - Edit `inventory/group_vars/windows/vault.yml` for Windows credentials.
   - Encrypt vault files using Ansible Vault:
     ```sh
     ansible-vault encrypt inventory/group_vars/all/vault.yml
     ansible-vault encrypt inventory/group_vars/windows/vault.yml
     ```

5. **Review and Adjust Playbooks**
   - `main.yml` is the entry point and includes both Linux and Windows patching logic.
   - `linux.yml` and `windows.yml` contain the detailed patching and reporting steps for each OS.

## Usage
Run the main playbook with your inventory:
```sh
ansible-playbook -i inventory/hosts.ini --limit linux main.yml --ask-vault-pass
ansible-playbook -i inventory/hosts.ini --limit windows main.yml --ask-vault-pass
```

## Security
- **Sensitive data** (API keys, passwords) must be stored in vault-encrypted files.
- Never commit unencrypted secrets to version control.

## Notification
- Email notifications are sent via Mailjet API after patching, with logs attached.
- Configure sender/recipient and Mailjet credentials in `all.yml` and `vault.yml`.

## Customization
- Adjust log paths, email templates, and patching logic as needed in the group_vars and playbooks.
- Add or remove tags to control playbook granularity.

## Structure
- **Pre-update:** Gathers facts, logs system state
- **Patching:** Updates packages, logs results, handles errors
- **Post-update:** Validates services, filesystems, and interfaces, logs outcomes
- **Reporting:** Assembles and displays a comprehensive report

## Authors
- Stilyan Slavkov <slavkov30@gmail.com>

---
For questions or contributions, please open an issue or pull request.

