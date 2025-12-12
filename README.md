# Orbit

Configuring my domestic fleet of machines.

## Setting up

Not using podman because sometimes it requires quite a fiddling to set up.

```bash
$ virtualenv environment/python
$ . environment/python/bin/activate
$ pip install -r environment/python/dependencies.list
$ ansible-builder build -f environment/ansible/execution.yml -c workspace/ansible/environment --container-runtime docker --tag ansible.management.orbit.mkanes.me
$ executables/compile
```

## Converging

### Metal

```bash
$ . environment/python/bin/activate
$ export ANSIBLE_NAVIGATOR_CONFIG="$(pwd)/environment/ansible/navigator.yml"
$ ansible-navigator run sources/management/metal/playbook/packages.yml --b -K
```
