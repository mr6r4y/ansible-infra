# ansible-infra

A collection of ansible roles and playbooks used in setting up various environments

## Init

After cloning the repo there are three things you should consider:
* making a `vault-passwd` file
* generating `id_rsa` and `id_rsa.pub` key files
* fixing `host_vars/local` file for your local machine

### Making a `vault-passwd` file

Just make a file containing your vault password (no new-line characters). The file
should be `keys/vault-passwd` at `keys` directory. The vault password file is
used whenever you encrypt/decrypt data throughout `.yml` of the project.

### Generating `id_rsa` and `id_rsa.pub` key files

Public and private key files should exist in `keys/` folder. They are used by
the default `ansadm` if you build machines with first playing `init-ansadm` 
playbook.

```bash
cd ansible-infra/keys/
ssh-keygen -t rsa -C ansadm
```

### Fixing `host_vars/local` file for your local machine

If you run playbooks locally, you are using the credentials listed in `host_vars/local`
file:

```yaml
---

ansible_host: localhost
ansible_user: user
ansible_become_pass: ...
ansible_connection: local
```

If you do not want to list your password (you can protect this file using `ansible-vault` and
the `keys/vault-passwd` file) you have to be asked for it by including `-K, --ask-become-pass`
option when executing `ansible` or `ansible-playbook` commands.

## Playbooks

* **build-inetsim.yml** - installing Inetsim latest package from the officially specified
sources to a fresh Ubuntu. Inetsim is a network simulation program used in malware analysis laboratories
* **build-re.yml** - install/update/build various tools and packages used for reverse engineering to fresh
Ubuntu. Currently Radare2, Yara and z3 are within
* **init-ansadm.yml** - initializes `ansadm` as standard user to a fresh Ubuntu.