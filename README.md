## ManageIQ Ansible collection
This is collection contains roles and playbooks to install a ManageIQ appliance in Openstack.

Roles:
 - deploy_manageiq_appliance
   - This role deploys the Openstack instance itself via the openstack.cloud ansible module.
   - The openstack.cloud ansible module uses the Openstack API to interact with openstack. Sourcing an openrc file is all that is required.
- configure_manageiq_appliance
  - This role connects to the ManageIQ appliance instance and executes the initial configuration for ManageIQ
  - Adds cloud resource providers defined in the 'manageiq_providers' variable in the role's defaults/main.yml file.


Playbooks:
 - deploy_appliance.yml
   - This playbook will deploy ManageIQ as a stand-alone appliance as an Openstack VM.

### Setup test environment to run the playbook

###### Setup a working directory:
```
mkdir /root/manageiq
cd /root/manageiq/
```

###### Clone the repo
```
git clone https://github.com/sdhardy/ManageIQ-Ansible.git manageiq-ansible
```

###### symlink the collection inside an 'ansible_collections' directory with the galaxy namespace
```
mkdir -p ansible_collections/sdhardy
ln -s manageiq-ansible ansible_collections/sdhardy/manageiq
```

###### Create a local ansible.cfg
```
cat << 'EOF' > ansible.cfg
[defaults]
timeout = 120
collections_path = /root/manageiq
display_skipped_hosts = false
host_key_checking = false
[ssh_connection]
pipelining = True
```

###### Install python and create a virtual env
```
apt install -y python3.8 python3.8-venv
python3 -m venv venv
source venv/bin/activate
pip3 install ansible openstacksdk
```

###### Install the openstack.cloud Ansible Collection
```
ansible-galaxy collection install openstack.cloud --collections-path /root/manageiq
```

###### Run the deploy_appliance.yml playbook to create and configure the ManageIQ all-in-one appliance
```
source /root/openrc
ansible-playbook manageiq-ansible/playbooks/deploy_appliance.yml
```

