---
layout: post
title: "Ansible Cheatsheet"
categories:
  - guide
tags:
  - guide
---

Compilation of things that I found useful:

### Useful Variables
Private key file location: `ansible_ssh_private_key_file`

### Refreshing Dynamic inventory
```
- name: refresh inventory
  hosts: localhost
  connection: local
  gather_facts: False
  tasks:
    - name: Refresh EC2 cache
      command: /etc/ansible/ec2.py --refresh-cache
    - name: Refresh in-memory EC2 cache
      meta: refresh_inventory
```
### Check availability of a module
``ansible-doc -t module <module name>``

**References:**

1. https://stackoverflow.com/questions/29003420/reload-ansibles-dynamic-inventory
