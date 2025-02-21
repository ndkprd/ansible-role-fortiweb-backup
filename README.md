# ansible-role-fortiweb-backup

Ansible role to backup Fortiweb configuration and send it via email or SFTP/Ansible copy module.

## Installation

```
ansible-galaxy install ndkprd.fortiweb_backup
```

## Usage

### Hosts Example

```ini
[fortiwebs]
fwb-01 ansible_host=fwb-01.example.com ansible_user=ansible ansible_password=supersecretpassword

[fortiwebs:vars]
ansible_network_os=fortinet.fortiweb.fwebos
ansible_httpapi_use_ssl="yes"
ansible_httpapi_validate_certs="no"
ansible_httpapi_port=443
```

### Playbook Example

```yaml
---

- name: Backup Fortiweb configuration.
  hosts: fortiwebs
  become: false
  gather_facts: false
  vars:
    fortiweb_backup_files_suffix: "_EXAMPLE_INC_FWB_BACKUP"
    fortiweb_backup_files_retrieval:
    email:
        enabled: true
        host: smtp.example.com
        port: 587
        username: bot.devops@example.com
        password: super-secret-password
        secure: starttls
        from: "bot.devops@example.com (EXAMPLE DevOps Bot)"
        footer: |
        Best regards,
        EXAMPLE DevOps Bot

        (This email is automatically generated, please do not reply to this email.)
    subject: "Fortiweb Configuration Backup"
    to:
    - it@example.com
    sftp:
        enabled: true
        remote_host: logger@logging.svc.example.com
        remote_path: "/home/ansible/backups/fortiweb"
        remote_user: ansible

  roles:
    - ndkprd.fortiweb_backup

```

## License

[MIT](./LICENSE)
