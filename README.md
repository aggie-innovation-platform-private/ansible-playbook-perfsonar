playbook for perfSONAR deployment and config

[![Git](https://app.soluble.cloud/api/v1/public/badges/d3f6711f-fa42-4a28-9077-dffd802d0d8b.svg?orgId=635054152320)](https://app.soluble.cloud/repos/details/github.com/aggie-innovation-platform-private/ansible-playbook-perfsonar?orgId=635054152320)  

**Quick Start**:

Clone this repository:

```
git clone https://github.com/perfsonar/ansible-playbook-perfsonar.git
cd ansible-playbook-perfsonar
```

Get the required roles (note that we ignore errors so we can run this multiple times):

```
ansible-galaxy install -r  requirements.yml --ignore-errors
```

Set up your inventory.  Connection variables can be added here as well.

```
vi inventory/hosts
```

Set up perfSONAR variables by running the defaults.sh script and then editing them:

```
./defaults.sh
vi inventory/group_vars/ps_*
```

Set up individual host variables with the lsregistration.yml template

```
mkdir inventory/host_vars/myhostname
cp roles/ansible-role-perfsonar-testpoint/defaults/lsregistration.yml \
  inventory/host_vars/myhostname
vi inventory/host_vars/myhostname/lsregistration.yml
```

Run the playbook:

```
ansible-playbook perfsonar.yml
```

---

**Some useful commands to manage the environment**

Use Ansible ping to verify connectivity to targets:

```
ansible all -m ping
```

Manage PWA users:
```
vi /inventory/group_vars/ps_pconfig_web_admin.yml
ansible-playbook \
  --limit 'ps-psconfig-web-admin' --tags 'ps::pwa_users'\
  perfsonar.yml
```

Display PWA users:
```
ansible ps-psconfig-web-admin \
  -a "docker exec -it sca-auth pwa_auth listuser --short"
```

Display PWA users, all atributes:
```
ansible ps-psconfig-web-admin \
  -a "docker exec -it sca-auth pwa_auth listuser"
```
  
Reset Password for PWA user from the command line:
```
ansible ps-psconfig-web-admin \
  -a "docker exec -it sca-auth pwa_auth setpass \
  --username user --password newpasswd"
```

Reset Password for PWA user interactively:
```
ansible-playbook \
  roles/ansible-role-perfsonar-psconfig-web-admin/playbooks/pwa_passwd_reset.yml
```
