Cloud Infra
=========

Role to create & destroy simple cloud infra via Terraform
Provide the terraform github repo to use and some vars

Requirements
------------

see python_requirements.txt and / or ansible_requirements.yml

Role Variables
--------------

WIP

Dependencies
------------

None

Example Playbook
----------------

Create infra
```bash
- name: Create infra
  hosts: localhost
  connection: local
  gather_facts: no

  roles:
  - role: cloud-infra
    vars:
      state: present
  

- name: Check SSH availability
  hosts: all
  gather_facts: no

  tasks:
  - name: Wait for system to become reachable
    wait_for_connection:
      timeout: 60
```

Destroy infra
```bash
- name: Destroy infra
  hosts: localhost
  connection: local
  gather_facts: no

  roles:
  - role: cloud-infra
    vars:
      state: absent
````