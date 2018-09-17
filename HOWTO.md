# From ansible installation to peertube working

## Debian

```shell
$ sudo apt install ansible
$ mkdir mypeertube
$ cd mypeertube
$ mkdir {play,roles,host_vars}
$ cat > inventory.ini <<EOF
[peertube]
peertube.mydomain.io  ansible_user=root ansible_pass=mYpAssw00rd
EOF
$ cat > ansible.cfg <<EOF
[defaults]
inventory = ./inventory.ini
roles_path = roles
vault_password_file = vault_pass
gather_facts = True
EOF
$ cat > requirements.yml <<EOF
---
- name: peertube
  src: git+https://framagit.org/jeromeavond/ansible-role-peertube
EOF
$ ansible-galaxy install -r requirements
$ cat > play/peertube.yml <<EOF
---
## playbook to install and configure peertube latest beta on Debian

## passwords for app_user and postgresql are generated dynamically and 
## written in ~/<username>.credentials.txt in your HOME

- hosts: peertube
  become: yes
  roles:
    - ansible-role-peertube
EOF
$ cat > vault_pass <<EOF
myTremend00sVAULTpassw9rd
EOF
$ cat > .gitignore <<EOF
vault_pass
EOF
$ mkdir host_vars/peertube.mydomain.io
$ cat > host_vars/peertube.mydomain.io/vars.yml <<EOF
---

#app_domain: 
#peer_version: 
  # v1.0.0-beta.13
  # v1.0.0-beta.9

config:
  smtp:
    comment: |
      port 465  If you use StartTLS 587
      tls true  If you use StartTLS false
      ca_file null  Used for self signed certificates
    hostname: my.smtp.server.net
    port: 587 # If you use StartTLS: 587
    username: '{{ smtp_login }}'
    password: '{{ smtp_pass }}'
    tls: false # If you use StartTLS: false
    disable_starttls: false
    ca_file: null # Used for self signed certificates
    from_address: 'my_peertube_alias@mydomain.io'
  log:
    level: 'info' 
    comment: debug/info/warning/error

# config that may be changed on load in the web interface

accounts:
  twitter: "{{ twitt_acc }}"
  adminmail: "{{ adminmail }}"

EOF
$ ansible-vault create host_vars/peertube.mydomain.io/vault.yml
```
```yaml
---
# vault.yml

smtp_login: mylogin
smtp_pass: mYl00g1n
twitt_acc: \@myTwitterAcc00nt
adminmail: 'my_admin_mail@mydomain.io'
```
```shell
$ ansible-playbook play/peertube.yml
```

Now get some bier and snack, maybe the last episode of Bojack Horseman (or Archer) and stay hydrated

On a 2 cpu with 2 Go RAM it takes at most 20 min, and the disk space of peertube itself is below 2 Go

read carefully the prompts after the installation, there's important informations.



