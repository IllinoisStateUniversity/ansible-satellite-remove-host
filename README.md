# Satellite - Remove Host

## Overview

This Ansible playbook removes a host from Red Hat Satellite servers in the Test or Production environments.

## Features

- **Multi-Environment Support**: Removes hosts from Test or Production Satellite servers.
- **Secure Credentials**: Uses Ansible Vault to store Satellite server credentials securely.
- **Selective Execution**: Utilize tags to target specific environments.

## Prerequisites

- **Ansible Collection**: `redhat.satellite` (install via `ansible-galaxy collection install -r collections/requirements.yml`).
- **Credentials**: Satellite server URLs and credentials stored in `vars/vault.yml`, encrypted with Ansible Vault.
- **Inventory**: Target hosts defined in your Ansible inventory or specified via `survey_hostname` or `survey_hosts`.

## Variables

- **host_to_remove**: Defaults to the lowercase FQDN of the host (`{{ ansible_fqdn | lower }}`).

### Credentials in `vars/vault.yml`
``` yaml
# vars/vault.yml
test_satellite_server_url: https://your-test-satellite-url
test_satellite_username: your_test_username
test_satellite_password: your_test_password

prod_satellite_server_url: https://your-prod-satellite-url
prod_satellite_username: your_prod_username
prod_satellite_password: your_prod_password


satellite_validate_certs: false  # Set to true if using valid SSL certificates
```

Note: Encrypt vars/vault.yml using Ansible Vault:

```bash
ansible-vault encrypt vars/vault.yml
```

## Usage

### Update Variables

- Replace URLs and Login Creds in your vault file.
    
### Run the Playbook

- Run the playbook targeting your hosts in AAP/AWX using tags to determine which environment to remove the host from.

## Important Notes

- **Delegation**: Tasks are delegated to `localhost` to interact with Satellite APIs.
- **Tags**: The `never` tag prevents accidental execution; specify the environment tag to run tasks.
- **SSL Validation**: Set `satellite_validate_certs` to `true` if using valid SSL certificates.
- **Security**: Always encrypt sensitive information with Ansible Vault.