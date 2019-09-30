# ansible-playbooks

```shell
/ansible-playbooks/tools/awx/install_awx.sh && \
/ansible-playbooks/tools/awx/configure_tower_cli.sh && \
/ansible-playbooks/tools/awx/set_test_config.sh
```

* https://stackoverflow.com/questions/18195142/safely-limiting-ansible-playbooks-to-a-single-machine/18195217#18195217
* https://github.com/adamrushuk/ansible-azure/blob/master/vagrant/scripts/configure_ansible_awx.sh

```shell
/ansible-playbooks/tools/update_known_hosts.sh

# --check                 Dry run
# --module-path /ansible-playbooks/library
# --limit ubuntu-xenial   Run only on selected hosts
# Run locally (note the trailing comma after 'localhost')
# -i localhost, --connection=local
ansible-playbook /ansible-playbooks/tools/run_role.yml --extra-vars "role_name=hello-world"

ansible localhost -m setup
# https://docs.ansible.com/ansible/latest/modules/setup_module.html#parameters
ansible centos-7 -m setup -a 'gather_subset=min'
```
`ansible_virtualization_role`, `ansible_virtualization_type`
 * https://github.com/ansible/ansible/blob/devel/lib/ansible/module_utils/facts/virtual/linux.py
 
 ```yaml
- name: Gather package facts
  package_facts:
    manager: apt

- name: "Check if 'postfix' package is installed"
  set_fact:
    linux_mta_postfix_is_installed: "{{ ('postfix' in ansible_facts.packages)|bool }}"
```
 
Pywinrm
```shell
# Pre-requisites
pip install kerberos requests_kerberos
# python-dev for Python 2
sudo apt install gcc python3-dev libkrb5-dev

# Execute BEFORE running Python script
# Enter domain name exactly like specified in /etc/krb5.conf (e.g. DOMAIN.TLD, not DOMAIN.tld)
kinit user@DOMAIN.TLD
klist
```

```python
import winrm

s = winrm.Session('host.domain.tld', auth=(None, None), transport='kerberos')
r = s.run_cmd('ipconfig', ['/all'])
print(r.std_out.decode("windows-1251"))
```

```shell
# Destroy all kerberos tickets
klist
kdestroy
```
```shell
ansible win_hosts -m "win_command" -a "cmd /c set"
ansible win_hosts -m raw -a "cmd /c set"
```
