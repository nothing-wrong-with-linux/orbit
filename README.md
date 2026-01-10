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
$ ansible-navigator run sources/management/metal/playbook/%playbook%.yml -b -k -K [-u user] [--limit <host>]
```

## Full scenario

It is implied that any machine provides SSH access to a sudo-capable user from 
the very beginning.

### Maintenance users

These are the system users to perform all administrative tasks. `relay` provides 
remote access and requires both password and a registered SSH key, `airlock` is
created as an escape hatch for local rescue operations with physical access.

1. Copy sources/definitions/secrets/users/*.template and fill the password and 
keys.
2. Run `executables/compile` to generate the variable files.
3. Apply `access` playbook:

   ```
   ansible-navigator run sources/management/metal/playbook/access.yml -b -k -K [--limit <host>]
   ```
   
Starting from this point, all further operations should be performed through 
`relay` user.

### Hardening

SSH is restricted to LAN by allowing only ipv4 connections, and password 
authentication is disabled by default:

```
ansible-navigator run sources/management/metal/playbook/hardening.yml -b -k -K [--limit <host>]
```

### Networking

The second part is assigning stable network interface names on the hosts so that
hardware changes would not affect the configuration and potential name bindings.

```
ansible-navigator run sources/management/metal/playbook/network.yml -b -k  -K [--limit <host>]
```

### System configuration

```
ansible-navigator run sources/management/metal/playbook/sysctl.yml -b -k  -K [--limit <host>]
```

### System packages

```
ansible-navigator run sources/management/metal/playbook/packages.yml -b -k -K [--limit <host>]
```
