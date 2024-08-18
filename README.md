# Install RHCSv4 in a test environment

- [ceph-ansible 4.0.70.18-1.el8cp](https://access.redhat.com/downloads/content/ceph-ansible/4.0.70.18-1.el8cp/noarch/fd431d51/package) [github](https://github.com/ceph/ceph-ansible/tree/v4.0.70.18)
- [rhceph/rhceph-4-rhel8](https://catalog.redhat.com/software/containers/rhceph/rhceph-4-rhel8/5e39df7cd70cc54b02baf33f)
- [docs](https://docs.redhat.com/en/documentation/red_hat_ceph_storage/4/html/installation_guide/installing-red-hat-ceph-storage-using-ansible#prerequisites_4)

Get repo
```
git clone git@github.com:ceph/ceph-ansible.git -b v4.0.70.18
cd ceph-ansible
```
ceph-ansible v4.0.70.18 requires ansible 2.9 (old!). Use a venv to
make this available on the RHEL9 system where I'm running ansible
from.
```
python3 -m venv ansible-venv
source ansible-venv/bin/activate
pip install ansible==2.9.*
ansible --version
pip install netaddr six
```

Run all commands inside `ceph-ansible` directory unless otherwise
noted.

Put [inventory.ini](inventory.ini) in `ceph-ansible` directory.
```
ansible -i inventory.ini -m ping mgrs,osds,mons
```
Copy site-container.yml.
```
cp site-container.yml.sample site-container.yml
```
Ensure [all.yml](all.yml) and [osds.yml](osds.yml) are in `groups_vars`.
```
group_vars/all.yml
group_vars/osds.yml
```

Prior to running ceph-ansible I ran the following on my rhel8 systems.
```
  subscription-manager register
  sudo dnf install podman
  podman login registry.redhat.io
  podman pull registry.redhat.io/rhceph/rhceph-4-rhel8:latest
```

Run playbook

``` 
ansible-playbook site-container.yml -i inventory.ini
```
