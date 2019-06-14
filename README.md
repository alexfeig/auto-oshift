# OpenShift Deployment Automation
This project is a series of Ansible playbooks to set up and configure an OpenShift or OKD environment
on top of ESXi or GCP from scratch. This means no creating VM templates or setting up an OS by hand.

This will:

* Set up as many master and regular node instances as you'd like in your cluster
* Register them with RHSM (if RHEL)
* Update them
* Install OpenShift or OKD

## OpenShift or OKD?
If you'd prefer to install OKD, please set the `openshift_type` variable to `origin` and not `openshift-enterprise` in your inventory file

## Requirements
* Download CentOS or RHEL ISO and place it in the `files/` folder
* Grab an example host/group_vars directory from the `inventories/` directory and edit variable and inventory files as necessary

**Note:** You must fill out ansible_host in the `hosts` file for this to work properly.

## Creating creds.yml
It is recommended that you use Ansible Vault to create the credential file. 

To do so, type `ansible-vault create creds.yml` and vim will open.

Add the following variables:

|           Variable          |                                                     Description                                                    |   Example                                |
|-----------------------------|--------------------------------------------------------------------------------------------------------------------|------------------------------------------|
| `rhsm_username`             | Red Hat Subscription Username                                                                                      | `foo@bar.com`                            |
| `rhsm_password`             | Red Hat Subscription Password                                                                                      | `password`                               |
| `rhsm_master_pool_id`       | Red Hat Subscription pool ID for master/infra nodes                                                                | `1234-abcdef`                            |
| `rhsm_node_pool_id`         | Red Hat Subscription pool ID for worker nodes                                                                      | `5678-abcdef`                            |
| `oreg_auth_user`            | Username for registry.redhat.io - likely the same as rhsm_username                                                 | `foo@bar.com`                            |
| `oreg_auth_password`        | Username for registry.redhat.io - likely the same as rhsm_password                                                 | `password`                               |
| `gcp_project`               | If GCP is selected, GCP project                                                                                    | `26455-foo`                              |
| `gcp_dns_zone`              | DNS zone to use. This project will create A records for each instance.                                             | `foo.bar.com`                            |
| `gcp_service_account_email` | GCP service account email address                                                                                  | `foo@bar.iam.gserviceaccount.com`        |
| `gcp_service_account_file`  | JSON file GCP gives you when creating a new service account                                                        | `/gcp/foo-abcdef123.json`                |
| `gcp_ssh_key`               | Path to SSH key to use for GCP instances, or SSH public key. If public key, just replace this string with the key. | `{{ lookup('file', '~/.ssh/k8s.pub') }}` |

## Variables
Variables are stored in `group_vars/all.yml` and associated `host_vars` files.

### hypervisor
This can be set to either `esxi` or `gcp`.

### esxi_*
For now, you will need to add your vSwitch/DVS information into `roles/esxi/packer/packer_template.j2`

TODO: Fix this

# Running
To run, issue `ansible-playbook -i <INVENTORY FILE> --ask-vault-pass main.yml`

# Bug Tracking
oshift_reg_url: https://github.com/openshift/openshift-ansible/issues/11419