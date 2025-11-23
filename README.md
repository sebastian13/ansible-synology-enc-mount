# Ansible Mount encrypted Shares on Synology

Use this role to mount encrypted shares on a Synology NAS.

### Install

```shell
ansible-galaxy install sebastian13.synology-enc-mount --force 
```

### Example Playbook

```yaml
---

- name: Mount encrypted shares
  hosts: nas

  roles:
    - role: sebastian13.synology-enc-mount

  vars:
    encpwd: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      656161...

  # Alternatively, uncomment the following to be prompted for the password at runtime
  #
  #vars_prompt:
  # - name: "encpwd"
  #    prompt: "What's the shares' encryption password?"
  #    private: yes
```

Add the following to your `inventory.yml`. As SSH Key authentication and NO_PASSWD for sudo
is not persisting on Synology NAS, we need to define the SSH user and password here.

```yaml
nas:
  hosts:
    SPU.file01:
      ansible_host: 10.11.12.13
      ansible_port: 2222
      ansible_user: example
      ansible_ssh_pass: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          432334...
      ansible_become_pass: "{{ ansible_ssh_pass }}"
      ansible_python_interpreter: /bin/python3
```

Passwords and secret keys can be easily encrypted using ansible-vault.

```shell
ansible-vault encrypt_string --stdin-name 'ansible_ssh_pass'
ansible-vault encrypt_string --stdin-name 'encpwd'
```

### Usage

The playbook must then be run as follows:

```shell
ansible-playbook [playbook-name].yml --ask-vault-pass
```
