Cloud Infra
=========

Role to create & destroy simple cloud infra via Terraform
Provide the terraform github repo to use and some vars

Requirements
------------

### Environment Variables

GOOGLE_APPLICATION_CREDENTIALS: GCP Service Account Key file (path)
GCP_SERVICE_ACCOUNT_FILE: GCP Service Account Key file (path)

GCLOUD_PROJECT : The GCP Project ID to use

Examples :
```bash
export GOOGLE_APPLICATION_CREDENTIALS=~/.gcp/creds/ci-123456-213.json
export GCP_SERVICE_ACCOUNT_FILE=$GOOGLE_APPLICATION_CREDENTIALS

export GCLOUD_PROJECT=ci-123456
```
see python_requirements.txt and / or ansible_requirements.yml

Role Variables
--------------

WIP

Dependencies
------------

None

Example Playbook
----------------

### Create infra
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

### Destroy infra
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