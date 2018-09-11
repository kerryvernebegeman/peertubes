# ansible-role-peertube

## Basics
### requirements.yml

```yaml
---
- name: peertube
  src: https://framagit.org/jeromeavond/ansible-role-peertube.git

```

```bash

$ ansible-galaxy -r requirements.yml install peertube

```

### Inventory

```dosini

[peertube]
example
```

### Playbook peertube.yml

```yaml
---

- hosts: peertube
  roles:
    - peertube

```

### Installation

```bash

$ ansible-playbook peertube.yml

```

## In details

### Variables

See in file [defaults](defaults/main.yml)




