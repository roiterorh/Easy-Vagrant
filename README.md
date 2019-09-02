## Why

Purpose is to simplify creation of usable VMs with accessible DNS names and same user as host


## Requirements

[Virtualbox](https://www.virtualbox.org/wiki/Downloads)

[Vagrant](https://www.vagrantup.com/downloads.html)

SSH keys in `~/.ssh` (generate with `ssh-keygen` if missing)

## How to 

### Configure

machine.yml
```yaml
# simply set defaults for your VMs
defaults:
  cpus: 4
  image: "bento/ubuntu-16.04"
  memory: 2048

# create a VM using default values
machine1: 

# create a VM using different memory settings + default for other values
machine2: 
  memory: 1024

```

### Run 

vagrant up

### Access

```bash
ssh machine1.local
```

or 

```bash
vagrant ssh machine1
```