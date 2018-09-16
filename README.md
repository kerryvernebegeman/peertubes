# ansible-role-peertube

tl;dr => Go straight to the [HOWTO](HOWTO.md)

## Basics

### requirements.yml

```yaml
---
- name: peertube
  src: git+https://framagit.org/jeromeavond/ansible-role-peertube.git

```

```bash

$ ansible-galaxy -r requirements.yml install peertube

```

### Inventory

```dosini

[peertube]
vm.example.net	# You are able to ssh and become root in this 

```

### Playbook peertube.yml

```yaml
---

- hosts: peertube
  become: yes
  roles:
    - peertube

```

### Installation

```bash

$ ansible-playbook peertube.yml

```

### Upgrade

WARNING : Still In Dev ! Use at your own risk, there's a backup of your database in /tmp 

```bash

$ ansible-playbook peertube.yml -t upgrade

```

## In details

### Variables

See in file [defaults](defaults/main.yml)

#### Basics variables to set

```yaml

# config that can't be changed on load in the web interface

app_domain:

# If backend behind a HTTPS proxy (default)
webserver_https: True
webserver_port: 443
# If backend behind a HTTP proxy
webserver_https: False
webserver_port: 80

config:
  smtp:
    comment: |
      port 465  If you use StartTLS 587
      tls true  If you use StartTLS false
      ca_file null  Used for self signed certificates
    hostname: null
    port: 465 # If you use StartTLS: 587
    username: null
    password: null
    tls: true # If you use StartTLS: false
    disable_starttls: false
    ca_file: null # Used for self signed certificates
    from_address: 'admin@example.com'
  log:
    level: 'info' 
    comment: debug/info/warning/error

# config that may be changed on load in the web interface

accounts:
  twitter: "@twitteraccount"
  adminmail: "admin@example.com"

config_start:

  signup:
    enabled: false
    requires_email_verification: false
    limit: 10 
    comment: |
      When the limit is reached, registrations are disabled.
      -1 == unlimited
    filters:
      cidr: 
        comment: |
          You can specify CIDR ranges to whitelist 
          (empty = no filtering) or blacklist
        whitelist: []
        blacklist: []
  
  user:
    comment: |
      Default value of maximum video BYTES the user can upload
      (does not take into account transcoded files).
      -1 == unlimited
    video_quota: -1
    video_quota_daily: -1
  
  import:
    comment: |
      Add ability for your users to import 
      remote videos (from YouTube, torrent...)
    videos:
      http: 
        comment: |
          Classic HTTP or all sites supported by youtube-dl 
          https://rg3.github.io/youtube-dl/supportedsites.html
        enabled: false
      torrent: 
        comment: |
          Magnet URI or torrent file 
          (use classic TCP/UDP/WebSeed to download the file)
        enabled: false

  instance:
    comment: Instance settings
    name: 'PeerTube'
    short_description: 'PeerTube, a federated (ActivityPub) video streaming platform using P2P (BitTorrent) directly in the web browser with WebTorrent and Angular.'
    description: ' # Support markdown'
    terms: ' # Support markdown'
    default_client_route: '/videos/trending'
    comment: |
      By default, "do_not_list" or "blur" or "display" NSFW videos
      Could be overridden per user with a setting
    default_nsfw_policy: 'do_not_list'
    securitytxt: |
      "# If you would like to report a security issue\n# you may report it to:\nContact: https://github.com/Chocobozzz/PeerTube/blob/develop/SECURITY.md\nContact: mailto:{{ accounts.adminmail }}"
    customizations:
      comment: |
        javascript - Directly your JavaScript code (without <script> tags).  Will be eval at runtime
        css -  Directly your CSS code (without <style> tags). Will be injected at runtime
      javascript: '' 
      css: '' 
    robots: |
      User-agent: *
      Disallow: ''
    robots_comment: |
      Robot.txt rules. 
      To disallow robots to crawl your instance 
      and disallow indexation of your site, 
      add '/' to Disallow:
```

### Tags

#### List of tags:

* tags: install_nodejs           : Installation of nodejs8 (default)
* tags: former_install           : Install package, create user, dirs etc.
* tags: postgresql               : Create PostgreSQL user and empty database
* tags: install_peertube         : download, unzip, link and execute peertube installation
* tags: config                   : Write configuration for peertube 
* tags: nginx                    : Write configuration for nginx 
* tags: start_peertube           : set systemd service for peertube and start service
* tags: info                     : Show last statements (misc.)
* tags: upgrade                  : Never played until explicit tag, upgrade peertube to last version

