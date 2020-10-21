Cloud Infra
=========

Role to create & destroy simple cloud infra via Terraform
Provide the terraform github repo to use and some vars

Requirements
------------

### Environment Variables

GOOGLE_APPLICATION_CREDENTIALS: GCP Service Account Key file (path)

GCLOUD_PROJECT : The GCP Project ID to use

Examples :
```bash
export GOOGLE_APPLICATION_CREDENTIALS=~/.gcp/creds/ci-123456-213.json

export GCLOUD_PROJECT=ci-123456
```

terraform >= 0.13
ansible >= 2.10
jmespath
google-auth

Role Variables
--------------

| Variable                 | Required | Default                                                                  | Comments                                        |
| ------------------------ | -------- | ------------------------------------------------------------------------ | ----------------------------------------------- |
| `target_env`                | No | `test`                                                               | Environment used for inventory filters |
| `cloud_provider`            | No | `gcp`                                                                | Cloud provider to use to deploy infra |
| `ssh_files_path`            | No | `{{ lookup('env','HOME') }}/.ssh`                                    | Path to the ssh directory, to store ssh config & ssh keys files (usually ~/.ssh) |
| `ssh_privkey_name`          | No | `ansible_sa`                                                         | SSH private key name |
| `ssh_publkey_name`          | No | `ansible_sa.pub`                                                     | SSH public key name |
| `ssh_config_name`           | No | `ansible_sa.config`                                                  | SSH Config file name |
| `terraform_project_repo`    | No | `https://github.com/klusters/terraform-gcp-infra`                    | Git repo url to use for terraform scripts |
| `terraform_project_version` | No | `main`                                                               | Git repo branch/tag to use for terraform scripts |
| `terraform_env_vars`        | No | `TF_VAR_ssh_public_key: {{ ssh_files_path }}/{{ ssh_publkey_name }}` | Environment variables to use when terraform apply (dict) |
| `terraform_dir`             | No | `{{ lookup('env','HOME') }}/terraform`                               | Path to store terraform scripts to |
| `terraform_workspace`       | No | `default`                                                            | Terraform workspace to use |
| `template_inventory`        | No | `gce_instances.gcp.yml.j2`                                           | Dynamic inventory template to use |
| `template_tfvars`           | No | `tfvars.json.j2`                                                     | Terraform variables template |
| `template_ssh_config`       | No | `ssh_config.j2`                                                      | SSH config file template to use |
| `inventory_dest`            | No | `{{ playbook_dir }}/../../../inventories/{{ target_env }}`           | Path to store generated dynamic inventory to |
| `jump_group`                | No | `es_jump`                                                            | Jump / Bastion Group |
| `ansible_sa_name`           | No | `ansible-sa`                                                         | Ansible Service Account name used for SSH |

Dependencies
------------

None

Example Playbook
----------------

### Create infra
```yaml
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
```yaml
- name: Destroy infra
  hosts: localhost
  connection: local
  gather_facts: no

  roles:
  - role: cloud-infra
    vars:
      state: absent
````