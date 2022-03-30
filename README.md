


# Installing Ansible using pip
```shell=
python -m pip install --upgrade pip
python -m pip install --${USER} ansible

if SSH is not available
python -m pip install --${USER} paramiko

alternatives --set python /usr/bin/python

sudo python -m pip install argcomplete
sudo activate-global-python-argcomplete

sudo apt-get install ansible-lint
```


# Inventory
Regardless of ini or yaml format, 'hosts' is the file name
```shell=
sudo vi /etc/ansible/hosts
```

## yaml format
```shell=
all:
  children:
    primaries:
    workers:

primaries:
  hosts:
    p0:

workers:
  hosts:
    w0:
    w1:
    w2:
```

## SSH keys
### From Control Node copy public key to each Managed Node

```shell=
ssh-copy-id <host>
```


# Configuration
### Using multiple inventory files

If there is a conflict in the 2 files, items in `hosts2.yml` will take precidence:

```shell=
ansible-playbook deploy-stuff.yml -i <hosts1.yml> -i <hosts2.yml>
```

### Show config
```shell=
ansible config
```

### Show default params
```shell=
ansible-config dump
```

### Configuration options
```shell=
- ansible.conf
  - lowest priority
  - most common
- $ANSIBLE_CONFIG
- ENV vars
- cli options
- playbook keywords
- ansible vars
```

# Common cli arguments
```shell=
-b, --become
-k, --ask-pass
-K, --ask-sudo-pass (make sure current ${USER} has sudo privs)
--step
-C, --check
-u, --user
```

# Ad-hoc commands
```shell=
ansible all --list-hosts

ansible workers -m ping

ansible -m ping all -u ${USER}

ansible -m ping all -u ${USER} --become (become sudo/root w/o prompt)

ansible -a "uptime" all -u ${USER}

ansible w0 -m file -a "path=/var/tmp/foo state=touch mode=0764" -u ${USER}

ansible w0 -m find -a "path=/var/tmp/" -u ${USER}

ansible w0 -m setup -a "gather_subset=hardware" -u ${USER}

ansible w0 -m service_facts -u ${USER}

ansible primaries -m service -a "name=salt-master enabled=no state=stopped" -u ${USER} --become

ansible workers -m service -a "name=salt-minion enabled=no state=stopped" -u ${USER} --become
```

# Running playbooks

```shell=
ansible-playbook kafka-broker-volumes.yml -u ubuntu -vv --check

ansible-playbook -i w0 kafka-broker-volumes.yml -u ubuntu -vv --check
```
